# teradataml Unbounded Array Framework Functions

Inputs to Unbounded Array Framework (UAF) functions
TDAnalyticResult
teradataml.dataframe.dataframe.TDAnalyticResult.__init__ = __init__(self, data, id_sequence=None, payload_ﬁeld=None, payload_content=None, layer=None)
    DESCRIPTION:
        1. Create a TDAnalyticResult object from a teradataml Dataframe created on an
           Analytic Result Table (ART) which can be used as input to
           Unbounded Array Framework functions.
           The primary use of an analytical result table (ART) is to
           associate function results with a name label, enabling us to
           easily retrieve the result data and pass the result to another
           UAF function.
           An ART can have multiple layers.
           Each layer has its own dedicated row composition for the series
           or matrix.
           TDSeries object created using teradataml DataFrame on ART has
           only data as the required argument, rest are optional.
        2. Any operations like filter, select, sum, etc. over TDAnalyticResult
           returns a teradataml DataFrame.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the teradataml Dataframe.
            Types: teradataml DataFrame
        id_sequence:
            Optional Argument.
            Specifies a sequence of series to plot.
            Types: str or list of str
        payload_field:
            Optional Argument.
            Specifies the names of the fields for payload.
            Types: str or list of str
        payload_content:
            Optional Argument.
            Specifies the payload content type.
            Permitted Values: "REAL", "COMPLEX", "AMPL_PHASE",
                              "AMPL_PHASE_RADIANS", "AMPL_PHASE_DEGREES",
                              "MULTIVAR_REAL", "MULTIVAR_COMPLEX",
                              "MULTIVAR_ANYTYPE", "MULTIVAR_AMPL_PHASE",
                              "MULTIVAR_AMPL_PHASE_RADIANS ",
                              "MULTIVAR_AMPL_PHASE_DEGREES"
            Types: str
        layer:
            Optional Argument.
            Specifies the layer name of the ART, if dataframe is
            created on ART.
            Types: str
    RAISES:
        None
    RETURNS:
        None
    EXAMPLES:
        # Example 1: Creating an Art Spec on an Analytic Result Table (ART).
        >>> load_example_data("dataframe", "ocean_buoys")
        # Create an Analytic Result Table (ART) by executing UAF function.
        >>> con.execute("EXECUTE FUNCTION INTO ART(TSINFO_RESULTS)
        TD_SINFO(SERIES_SPEC (TABLE_NAME(OCEAN_BUOYS),
        ROW_AXIS(TIMECODE(TD_TIMECODE)),
        SERIES_ID(BuoyID), PAYLOAD(FIELDS(Salinity),CONTENT(REAL))));")
        <sqlalchemy.engine.cursor.LegacyCursorResult object at 0x000002366F2D9460>
        # Create a DataFrame on an ART
        >>> art_df = DataFrame("TSINFO_RESULTS")
        # Check if the DataFrame 'art_table' is created on an ART.
        >>> art_df.is_art
        True
        # Create TDAnalyticResult object which can be used as ART_SPEC input in UAF functions.
        >>> result = TDAnalyticResult(data=art_df)
        # Check if 'result' is created on an ART.
        >>> result.is_art
        True
        >>> result
           buoyid  ROW_I      INDEX_DT                 INDEX_BEGIN                   INDEX_END  NUM_ENTRIES  DISCRETE          SAMP
        0      44      1  TIMESTAMP(6)  2014-01-06 10:00:24.000000  2014-01-
    06 10:52:00.000009           13         0  MICROSECONDS(258000001)    REAL              55.0              55.0              55.
        1       0      1  TIMESTAMP(6)  2014-01-06 08:00:00.000000  2014-01-
    06 08:10:00.000000            5         0             SECONDS(150)    REAL              55.0              55.0              55.
        2       2      1  TIMESTAMP(6)  2014-01-06 21:01:25.122200  2014-01-
    06 21:03:25.122200            3         1               MINUTES(1)    REAL              55.0              55.0              55.
        3       1      1  TIMESTAMP(6)  2014-01-06 09:01:25.122200  2014-01-
    06 09:03:25.122200            6         0              SECONDS(24)    REAL              55.0              55.0              55.
    TDGenSeries
    teradataml.dataframe.dataframe.TDGenSeries.__init__ = __init__(self, instances, data_types, start, offset, num_entries)
    Generate a series to be passed to a UAF function rather than using a
    pre-existing series instance.
    It contains all the information that would have been derivable
    from a TDSeries as well as the information required to generate the series.
    The TDGenSeries can only be passed to a function that accepts a single series as input.
    Generated series have an indexing mechanism which starts at integer 0 and increments by 1 for each
    additional generated entry.
    PARAMETERS:
        instances:
            Required Argument.
            Specifies the columns and values for the generated series.
            Types: dict
        data_types:
            Required Argument.
            Specifies the data types of the identifying columns for the generated series.
            Types: teradatasqlalchemy.types
        start:
            Required Argument.
            Specifies Start value for the information about how the series payload, containing
            successive real magnitude values.
            Types: float, int
        offset:
            Required Argument.
            Specifies offset value for the information about how the series payload, containing
            successive real magnitude values.
            Types: float, int
        num_entries:
            Required Argument.
            Specifies number of entries for the information about how the series payload, containing
            successive real magnitude values.
            Types: int
    RAISES:
        None
    RETURN:
        None
    EXAMPLES:
        # Import TDGenSeries.
        >>> from teradataml import TDGenSeries
        # Import INTEGER type from teradatasqlalchemy.
        >>> from teradatasqlalchemy.types import INTEGER
        # Create a TDGenSeries object to be passed as input to UAF functions.
        >>> series = TDGenSeries(instances = {"BuoyID": 3}, data_types = INTEGER(), start=0, offset=1, num_entries=5)
    TDMatrix
    teradataml.dataframe.dataframe.TDMatrix.__init__ = __init__(self, data, id, row_index, column_index, row_index_style='TIMECODE', column_index_style='TIMECODE',
    id_sequence=None, payload_ﬁeld=None, payload_content=None, layer=None)
    DESCRIPTION:
        Create a TDMatrix object from a teradataml DataFrame representing
        a MATRIX in time series
        which is used as input to Unbounded Array Framework,
        time series functions.
        A matrix is a two-dimensional array that has rows and columns.
        A matrix is identified by its matrix id, i.e., "id" argument,
        and is indexed by "row_index" and "column_index" arguments.
        A matrix can be a one of the following types:
            * Row-major matrix: Each row is a wavelet that is grouped by
              its matrix id and "row_index", and ordered by its "column_index".
            * Column-major matrix: Each column is a wavelet that is grouped
              by its matrix id and "column_index", and ordered by its "row_index".
    PARAMETERS:
        data:
            Required Argument.
            Specifies the teradataml Dataframe.
            Types: teradataml DataFrame
        id:
            Required Argument.
            Specifies the name of the column in "data" containing the
            identifier values.
            Types: str or list of str
        row_index:
            Required Argument.
            Specifies the name of the column in "data" containing the
            row indexing values.
            Types: str
        column_index:
            Required Argument.
            Specifies the name of the column in "data" containing
            the column.
            indexing values.
            Types: str
        row_index_style:
            Optional Argument.
            Specifies the style of row indexing.
            Default Value: "TIMECODE"
            Permitted Values: "TIMECODE", "SEQUENCE"
            Types: str
        column_index_style:
            Optional Argument.
            Specifies the style of column indexing.
            Default Value: "TIMECODE"
            Permitted Values: "TIMECODE", "SEQUENCE"
            Types: str
        id_sequence:
            Optional Argument.
            Specifies a sequence of series to plot.
            Types: str or list of str
        payload_field:
            Optional Argument.
            Specifies the names of the fields for payload.
            Types: str or list of str
        payload_content:
            Optional Argument.
            Specifies the payload content type.
            Permitted Values: "REAL", "COMPLEX", "AMPL_PHASE",
                              "AMPL_PHASE_RADIANS", "AMPL_PHASE_DEGREES",
                              "MULTIVAR_REAL", "MULTIVAR_COMPLEX",
                              "MULTIVAR_ANYTYPE", "MULTIVAR_AMPL_PHASE",
                              "MULTIVAR_AMPL_PHASE_RADIANS ",
                              "MULTIVAR_AMPL_PHASE_DEGREES"
            Types: str
        layer:
            Optional Argument.
            Specifies the layer name of the ART table, if dataframe is created on ART table.
            Types: str
    RAISES:
        None
    RETURNS:
        None
    EXAMPLES:
        # Create a TDMatrix object.
        >>> from teradataml import create_context, load_example_data, DataFrame, TDMatrix
        >>> con = create_context(host = host, user=user, password=passw)
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame to be passed as input to TDMatrix.
        >>> data = DataFrame("admissions_train")
        # Create a TDMatrix object to be passed as input to UAF functions.
        >>> res = TDMatrix(data=data, id='admitted', row_index='id',
                           column_index = 'admitted', row_index_style="TIMECODE",
                           payload_field='payload_field',payload_content='REAL')
        >>> res
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
    TDSeries
    teradataml.dataframe.dataframe.TDSeries.__init__ = __init__(self, data, id, row_index, row_index_style='TIMECODE', id_sequence=None, payload_ﬁeld=None,
    payload_content=None, layer=None, interval=None)
    DESCRIPTION:
        1. Create a TDSeries object from a teradataml DataFrame
           representing a SERIES in time series which is used
           as input to Unbounded Array Framework, time series functions.
           A series is a one-dimensional array. They are the basic input
           of UAF functions.
           A series is identified by its series ID, i.e., "id" argument,
           and indexed by "row_index" argument.
           Series is passed to and returned from UAF functions as wavelets.
           Wavelets are collections of rows, grouped by one or more fields,
           and ordered on the "row_index" argument.
        2. Any operations like filter, select, sum, etc. over TDSeries
           returns a teradataml DataFrame.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the teradataml Dataframe.
            Types: teradataml DataFrame
        id:
            Required Argument.
            Specifies the name of the column in "data" containing the
            identifier values.
            Types: str or list of str
        row_index:
            Required Argument.
            Specifies the name of the column in "data" containing the
            row indexing values.
            Types: str
        row_index_style:
            Optional Argument.
            Specifies the style of row indexing.
            Default Value: "TIMECODE"
            Permitted Values: "TIMECODE", "SEQUENCE"
            Types: str
        id_sequence:
            Optional Argument.
            Specifies a sequence of series to plot.
            Types: str or list of str
        payload_field:
            Optional Argument.
            Specifies the names of the fields for payload.
            Types: str or list of str
        payload_content:
            Optional Argument.
            Specifies the payload content type.
            Permitted Values: "REAL", "COMPLEX", "AMPL_PHASE",
                              "AMPL_PHASE_RADIANS", "AMPL_PHASE_DEGREES",
                              "MULTIVAR_REAL", "MULTIVAR_COMPLEX",
                              "MULTIVAR_ANYTYPE", "MULTIVAR_AMPL_PHASE",
                              "MULTIVAR_AMPL_PHASE_RADIANS ",
                              "MULTIVAR_AMPL_PHASE_DEGREES"
            Types: str
        layer:
            Optional Argument.
            Specifies the layer name of the ART table, if dataframe is
            created on ART table.
            Types: str
        interval:
            Optional Argument.
            Specifies the indicator to divide a series into a collection of
            intervals along its row-axis.
            "interval" is categorised in to 4 types:
                * Values represent time-duration
                    * Allowed Values:
                        * CAL_YEARS
                        * CAL_MONTHS
                        * CAL_DAYS
                        * WEEKS
                        * DAYS
                        * HOURS
                        * MINUTES
                        * SECONDS
                        * MILLISECONDS
                        * MICROSECONDS
                * Values represent time-zero
                    * Allowed Values:
                        * DATE
                        * TIMESTAMP
                        * TIMESTAMP WITH TIME ZONE
                * Values represent an integer or floating number
                    * Allowed Values: A positive integer or float,
                     range from 1 to 32767, inclusively.
                * sequence-zero:
                    An expression which evaluates to an INTEGER or FLOAT.
                    Used when row_index_style is SEQUENCE.
            Allowed Values:
                Individual values or combined values from below:
                * time-duration
                * time-duration, time-zero
                * integer
                * float, integer
                * sequence-zero
                * float, sequence-zero
            Types: str
    RAISES:
        None
    RETURNS:
        None
    EXAMPLES:
        # Example 1: Creating TDSeries object.
        >>> from teradataml import create_context, load_example_data, DataFrame, TDSeries
        >>> con = create_context(host = host, user=user, password=passw)
        >>> load_example_data("dataframe", "admissions_train")
        # Create a DataFrame to be passed as input to TDSeries.
        >>> data = DataFrame("admissions_train")
        # Create TDSeries object which can be used as input in UAF functions.
        >>> result = TDSeries(data=data, id="admitted", row_index="admitted",
                              payload_field="abc", payload_content="REAL")
        >>> result
           masters   gpa     stats programming  admitted
        id
        5       no  3.44    Novice      Novice         0
        34     yes  3.85  Advanced    Beginner         0
        13      no  4.00  Advanced      Novice         1
        40     yes  3.95    Novice    Beginner         0
        22     yes  3.46    Novice    Beginner         0
        19     yes  1.98  Advanced    Advanced         0
        36      no  3.00  Advanced      Novice         0
        15     yes  4.00  Advanced    Advanced         1
        7      yes  2.33    Novice      Novice         1
        17      no  3.83  Advanced    Advanced         1
    Supported on Database Version: 17.20.xx
    MODEL PREPARATIONS and PARAMETER ESTIMATION functions
    ACF
    ACF
    Functions
    ACF(data=None, data_ﬁlter_expr=None, max_lags=None, func_type=False, unbiased=False, demean=True, qstat=False, alpha=None, **generic_arguments)
    DESCRIPTION:
        The ACF() function calculates the autocorrelation or
        autocovariance of a time series. The autocorrelation and
        autocovariance show how the time series correlates or
        covaries with itself when delayed by a lag in time or space.
        When the ACF() function is computed, a coefficient corresponding
        to a particular lag is affected by all the previous lags.
        For example, the coefficient for lag 4 includes effects of
        activity at lags 3, 2, and 1.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input time series with payload 
            content value as 'REAL' or 'MULTIVAR_REAL'.
            Types: TDSeries
        data_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data".
            Types: ColumnExpression
        max_lags:
            Optional Argument.
            Specifies the maximum number of lags to calculate the
            autocorrelation or autocovariance, a positive integer
            less than or equal to N-1, where N is the number of
            observations in the time series. The default is 10*log10(N).
            When "max_lags" value exceeds N-1, the function ignores "max_lags"
            and uses the system-defined value.
            Note:
                For the function to resolve, 
                the number-of-entries-per-series * max_lags must be less than
                7,864,200,000. For a series having more than 88,600 entries,
                the "max_lags" value must be a number smaller than 88,600 for
                the function to complete.
            Types: int
        func_type:
            Optional Argument.
            Specifies the calculation type, that is whether to use
            autocorrelation or autocovariance method. 
            When set to False, calculation type as autocorrelation,
            otherwise it is autocovariance.
            Default Value: False
            Types: bool
        unbiased:
            Optional Argument.
            Specifies the formula for the denominator
            to calculate the autocovariance,
            When set to False, Jenkins-Watts formula is used,
            otherwise Box-Jenkins is used.
            Default Value: False
            Types: bool
        demean:
            Optional Argument.
            Specifies whether to subtract the mean X from 
            each element of X in the formula before 
            calculating the autocorrelation or autocovariance. 
            When set to False, mean value is not subtracted 
            from each element, otherwise subtracted.
            Default Value: True
            Types: bool
        qstat:
            Optional Argument.
            Specifies whether to provide the Ljung-Box 
            q-statistic and its associated p-value for each
            autocorrelation coefficient. When set to True,
            the Ljung-Box q-statistic and its associated 
            p-value included in the result, otherwise not.
            Default Value: False
            Types: bool
        alpha:
            Optional Argument.
            Specifies the level to return confidence interval.
            Use a positive float to return the interval. The
            function computes the standard deviation for confidence
            intervals with Bartlett's formula. For example,
            if "alpha" value is '0.05' meaning the 95% level, then 
            confidence intervals (CONFINT) are included in the results
            where the standard deviation is computed according to
            Bartlett’s formula.
            Default behavior when "alpha" avoided or not a positive
            float: 
                * The function does not return confidence intervals.
            Types: int OR float
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an 
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results. 
                    If not specified, a unique table name is internally 
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output 
                    table into. If not specified, table is created into 
                    database specified by the user at the time of context 
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of ACF.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as ACF_obj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Load the example data.
        load_example_data("uaf", ["ocean_buoy2"])
        # Create teradataml DataFrame object.
        data = DataFrame.from_table("ocean_buoy2")
        # Example 1: Apply the ACF() function to calculate the autocorrelation 
        #            of a time series with itself by using "max_lags".
        # Create teradataml TDSeries object.
        data_series_df = TDSeries(data=data,
                                  id="buoy_id",
                                  row_index_style="SEQUENCE",
                                  row_index="n_seq_no",
                                  payload_field="magnitude1",
                                  payload_content="REAL")
        # Execute ACF for TDSeries.
        uaf_out = ACF(data=data_series_df,
                      max_lags=2)
        # Print the result DataFrame.
        print(uaf_out.result)
    ArimaEstimate
    ArimaEstimate
    Functions
    ArimaEstimate(data1=None, data1_ﬁlter_expr=None, data2=None, data2_ﬁlter_expr=None, nonseasonal_model_order=None, seasonal_model_order=None,
    seasonal_period=None, lags_ar=None, lags_sar=None, lags_ma=None, lags_sma=None, init=None, ﬁxed=None, constant=False, algorithm=None,
    max_iterations=100, coeff_stats=False, ﬁt_percentage=100, ﬁt_metrics=False, residuals=False, input_fmt_input_mode=None,
    output_fmt_index_style='NUMERICAL_SEQUENCE', **generic_arguments)
    DESCRIPTION:
        The ArimaEstimate() function estimates the coefficients corresponding to an
        ARIMA (AutoRegressive Integrated Moving Average) model, and to
        fit a series with an existing ARIMA model. The function can also
        provide the "goodness of fit" and the residuals of the fitting operation.
        The function generates model layer used as input for the ArimaValidate() 
        and ArimaForecast() functions. This function is for univariate series.
        The ArimaEstimate() function takes one or two inputs, the second input is optional.
        The first input is a time series. The second input references
        the model context. When only one input is passed in, the ArimaEstimate() 
        function operates in a coefficient estimate mode. When two inputs are passed in,
        ArimaEstimate() function operates in a model apply mode. When the second input
        is passed in, user must include an input format mode argument.
        User can use the "fit_percentage" argument to pass a portion of the data,
        such as 80%, to the ArimaEstimate() function. The ART produced
        includes the ARTVALDATA layer which contains the remaining 20%,
        and can be used with ArimaValidate() function for the validation exercise.
        The following functions are run after ArimaEstimate() function to determine
        if the residuals are zero mean, have no serial correlation or exhibit
        homoscedastic variance:
            * CumulPeriodogram
            * HoltWintersForecaster
            * SignifPeriodicities
            * SimpleExp
        The following procedure is an example of how to use ArimaEstimate() function:
            1. Run the ArimaEstimate() function to get the coefficients
               for the ARIMA model.
            2. [Optional] Run ArimaValidate() function to validate the
               'goodness of fit' of the ARIMA model, when "fit_percentage" argument value
               is not 100 in ArimaEstimate() function.
            3. Run the ArimaForecast() function with input from step 1
               or step 2 to forecast the future periods beyond the last
               observed period.
        Notes:
            The following arguments are ignored when using two input files:
                * algorithm
                * constant
                * fixed
                * init
                * lags_ar
                * lags_sar
                * lags_ma
                * lags_sma
                * nonseasonal_model_order
                * seasonal_model_order
                * seasonal_period
            However, "algorithm", "constant", and "nonseasonal_model_order" arguments
            must be included in the ArimaEstimate() function, as they are mandatory.
    PARAMETERS:
        data1:
            Required Argument.          
            Specifies the input time series whose payload content is 'REAL'.
            Types: TDSeries
        data1_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data1".
            Types: ColumnExpression
        data2:
            Optional Argument.
            Specifies the TDSeries or TDAnalyticResult object created
            over the output of ArimaEstimate() function.
            Notes:
                When the "data2" is passed, user must include
                "input_fmt_input_mode" argument with the following behavior:
                    * MATCH: If no matching identifiers are found, then an
                      empty dataframe is returned.
                    * MANY2ONE or ONE2ONE: If the "data1" input is an empty
                      time series, then the function returns an empty result 
                      dataframe. If the "data2" input is an empty series, 
                      then an error is returned from the function.
            Types: TDSeries, TDAnalyticResult
        data2_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data2".
            Types: ColumnExpression
        nonseasonal_model_order:
            Required Argument.
            Specifies the non-seasonal values for the model. A list 
            containing three integer values, where each value is greater
            than or equal to 0:
                * First value is 'p', the order of the non-seasonal
                  auto-regression (AR) component.
                * Second value is 'd', the order of the non-seasonal differences
                  between consecutive components.
                * Third value is 'q', the order of the non-seasonal
                  moving average (MA) component.
            Types: int, list of int
        seasonal_model_order:
            Optional Argument.
            Specifies the seasonal values for the model. A list 
            containing three integer values, where each value is greater
            than or equal to 0:
                * First value is 'P', the order of the seasonal
                  auto-regression (SAR) component.
                * Second value is 'D', the order of the seasonal differences between
                  consecutive components.
                * Third value is 'Q', the order of the seasonal moving
                  average (SMA) component.
            Types: int, list of int
        seasonal_period:
            Optional Argument.
            Specifies the number of periods per season.
            Types: int
        lags_ar:
            Optional Argument.
            Specifies the p-length-lag-list is the lag values for the non-seasonal
            auto-regression component. The position-sensitive list that specifies the lags
            to be associated with the non-seasonal auto-regressive (AR)
            regression terms. Default is length of "nonseasonal_model_order" ([1,2,3,...p]).
            Types: int, list of int
        lags_sar:
            Optional Argument.
            Specifies the P-length-lag-list is the seasonal auto-regression components.
            the position-sensitive list that specifies the lags 
            associated with the seasonal auto-regressive (SAR) terms.
            Default is length of "seasonal_model_order" ([1xS, 2xS, ...., PxS]). 
            Types: int, list of int
        lags_ma:
            Optional Argument.
            Specifies The q-length-lag-list is the values for the moving average
            component. The position-sensitive list that specifies the lags associated
            with the moving average (MA) terms. Default is length
            of "nonseasonal_model_order" ([1,2,3,...q]).
            Types: int, list of int
        lags_sma:
            Optional Argument.
            Specifies the Q-length-lag-list is the values for the seasonal
            moving average component. The position-sensitive list that specifies
            the lags associated with the seasonal moving average (SMA) terms. Default
            is length of "seasonal_model_order" ([1xS, 2xS, ..., PxS]). 
            Types: int, list of int
        init:
            Optional Argument.
            Specifies the position-sensitive list that specifies the 
            initial values to be associated with the 'p' non-seasonal
            AR regression coefficients, followed by the 'q'
            non-seasonal MA coefficients, the 'P' seasonal SAR
            regression coefficients and the 'Q' SMA coefficients.
            If an intercept is needed, one more value is added
            at the end to specify the intercept coefficient initial
            value, then the formula is as follows:
                p+q+P+Q+constant
            Types: int, list of int, float, list of float
        fixed:
            Optional Argument.
            Specifies the position-sensitive list that specifies the 
            fixed values to be associated with the 'p' non-seasonal
            AR regression coefficients, followed by the 'q'
            non-seasonal MA coefficients, the 'P' seasonal 
            SAR regression coefficients and the 'Q' SMA coefficients.
            If an intercept is needed, one more value is added
            at the end to specify the intercept coefficient initial
            value, then the formula is as follows:
                p+q+P+Q+constant
            Types: int, list of int, float, list of float
        constant:
            Optional Argument.
            Specifies whether to calculate an intercept. When set to False, function
            does not calculate the intercept, otherwise calculates intercept.
            Default Value: False
            Types: bool
        algorithm:
            Required Argument.
            Specifies the approach used to estimate the coefficients.
            Permitted Values:
                * OLE: Use the sum of ordinary least squares approach.
                  Then, "fixed" and "init" are disabled.
                * MLE: Use maximum likelihood approach.
                * CSS_MLE: Use the conditional sum-of-squares to 
                  determine a start value and then do maximum likelihood.
                * CSS:  Use conditional sum-of-squares approach.
            Types: str
        max_iterations:
            Optional Argument.
            Specifies the limit on the maximum number of iterations to 
            estimate the ARIMA argument.
            Notes:
                * Applicable only when "algorithm" is set to 'MLE' processing.
                * If not present, then default is 100 iterations.
            Default Value: 100
            Types: int
        coeff_stats:
            Optional Argument.
            Specifies whether to return coefficient statistical columns
            TSTAT_VALUE and TSTAT_PROB. When set to True, function
            returns the columns, otherwise not. 
            Default Value: False
            Types: bool
        fit_percentage:
            Optional Argument.
            Specifies the percentage of passed in sample points that
            are used for the model fitting and parameter estimation.
            Default Value: 100
            Types: int
        fit_metrics:
            Optional Argument.
            Specifies whether to generate the model metadata statistics.
            The generated result set can be retrieved using the attribute
            'fitmetadata' of the function output. When set to True,
            function generate the model metadata statistics,
            otherwise not.
            Default Value: False
            Types: bool
        residuals:
            Optional Argument.
            Specifies whether to generate the residuals data.
            The generated residuals result can be viewed using
            the 'fitresiduals' attribute on the function output.
            When set to False, function does not generate the residual
            data, otherwise generates it.
            Default Value: False
            Types: bool
        input_fmt_input_mode:
            Optional Argument.
            Specifies the input mode supported by the function.
            Note:
                The "input_fmt_input_mode" argument is supported, when both
                "data1" and "data2" are passed.
            Permitted Values: MANY2ONE, ONE2ONE, MATCH
            Types: str
        output_fmt_index_style:
            Optional Argument.
            Specifies the index style of the output format.
            Permitted Values: NUMERICAL_SEQUENCE, FLOW_THROUGH
            Default Value: NUMERICAL_SEQUENCE
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an 
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results. 
                    If not specified, a unique table name is internally 
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output 
                    table into. If not specified, table is created into 
                    database specified by the user at the time of context 
                    creation or configuration argument. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of ArimaEstimate.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as ArimaEstimate_obj.<attribute_name>.
        Output teradataml DataFrame attribute names are:
            1. result
            2. fitmetadata - Available when "fit_metrics" is set to True, otherwise not.
            3. fitresiduals - Available when "residuals" is set to True, otherwise not.
            4. model
            5. valdata
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Load the example data.
        load_example_data("uaf", ["stock_data"])
        # Create teradataml DataFrame object.
        data = DataFrame.from_table("stock_data")
        # Example 1 : Execute ArimaEstimate() function to estimate the coefficients
        #             and statistical ratings corresponding to an ARIMA model.
        # Create teradataml TDSeries object.
        data_series_df = TDSeries(data=data,
                                  id="data_set_id",
                                  row_index="seq_no",
                                  row_index_style="SEQUENCE",
                                  payload_field="magnitude",
                                  payload_content="REAL")
        # Execute ArimaEstimate function.
        uaf_out = ArimaEstimate(data1=data_series_df,
                                nonseasonal_model_order=[2,0,0],
                                constant=False,
                                algorithm="OLE",
                                coeff_stats=True,
                                fit_metrics=True,
                                residuals=True,
                                fit_percentage=80)
        # Print the result DataFrames.
        print(uaf_out.result)
        print(uaf_out.fitmetadata)
        print(uaf_out.fitresiduals)
        print(uaf_out.model)
        print(uaf_out.valdata)
    ArimaValidate
    ArimaValidate
    Functions
    ArimaValidate(data=None, data_ﬁlter_expr=None, ﬁt_metrics=False, residuals=False, output_fmt_index_style='NUMERICAL_SEQUENCE', **generic_arguments)
    DESCRIPTION:
        The ArimaValidate() function performs an in-sample
        forecast for both seasonal and non-seasonal auto-regressive (AR),
        moving-average (MA), ARIMA models and Box-Jenkins seasonal
        ARIMA model formula followed by an analysis of the produced
        residuals. The aim is to provide a collection of metrics useful to select the model
        and expose the produced residuals such that multiple model validation
        and statistical tests can be conducted.
        The following procedure is an example of how to use ArimaValidate():
            * Run the ArimaEstimate() function to get the coefficients for the ARIMA model.
            * Run the ArimaValidate() function to validate the "goodness of fit" of the ARIMA model,
              when "fit_percentage" is not 100 in ArimaEstimate().
    PARAMETERS:
        data:
            Required Argument.
            Specifies the TDAnalyticResult object over the output
            of ArimaEstimate() function.
            Types: TDAnalyticResult
        data_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data".
            Types: ColumnExpression
        fit_metrics:
            Optional Argument.
            Specifies whether to generate the model metadata statistics.
            When set to True, metadata statistics are generated otherwise, it is not.
            Default Value: False
            Types: bool
        residuals:
            Optional Argument.
            Specifies whether to generate the model residuals.
            When set to True, residuals are generated,
            otherwise it is not.
            Default Value: False
            Types: bool
        output_fmt_index_style:
            Optional Argument.
            Specifies the index_style of the output format.
            Default Value: NUMERICAL_SEQUENCE
            Permitted Values: NUMERICAL_SEQUENCE, FLOW_THROUGH
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results.
                    If not specified, a unique table name is internally
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output
                    table into. If not specified, table is created into
                    database specified by the user at the time of context
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of ArimaValidate.
        Output teradataml DataFrames can be accessed using attribute
        references, such as ArimaValidate_obj.<attribute_name>.
        Output teradataml DataFrame attribute names are:
            1. result
            2. fitmetadata - Available when "fit_metrics" is set to True, otherwise not.
            3. fitresiduals - Available when "residuals" is set to True, otherwise not.
            4. model
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Load the example data.
        load_example_data("uaf", ["timeseriesdatasetsd4"])
        # Create teradataml DataFrame object.
        data = DataFrame.from_table("timeseriesdatasetsd4")
        # Execute ArimaEstimate() function to estimate the coefficients
        # and statistical ratings corresponding to an ARIMA model.
        # Create teradataml TDSeries object.
        data_series_df = TDSeries(data=data,
                                  id="dataset_id",
                                  row_index="seqno",
                                  row_index_style="SEQUENCE",
                                  payload_field="magnitude",
                                  payload_content="REAL")
        # Execute ArimaEstimate function.
        arima_estimate_op = ArimaEstimate(data1=data_series_df,
                                          nonseasonal_model_order=[2,0,0],
                                          constant=False,
                                          algorithm="MLE",
                                          coeff_stats=True,
                                          fit_metrics=True,
                                          residuals=True,
                                          fit_percentage=80)
        # Example 1: Validate the "goodness of fit" of the ARIMA model.
        # Create teradataml TDAnalyticResult object over the result attribute of 'arima_estimate_op'.
        data_art_df = TDAnalyticResult(data=arima_estimate_op.result)
        uaf_out = ArimaValidate(data=data_art_df, 
                                fit_metrics=True, 
                                residuals=True)
        # Print the result DataFrames.
        print(uaf_out.result)
        print(uaf_out.fitmetadata)
        print(uaf_out.fitresiduals)
        print(uaf_out.model)
    AutoArima
    AutoArima
    Functions
    AutoArima(data=None, data_ﬁlter_expr=None, max_pq_nonseasonal=[5, 5], max_pq_seasonal=[2, 2], start_pq_nonseasonal=[0, 0], start_pq_seasonal=[0, 0],
    d=-1, ds=-1, max_d=2, max_ds=1, period=1, stationary=False, seasonal=True, constant=True, algorithm='MLE', ﬁt_percentage=100, infor_criteria='AIC',
    stepwise=False, nmodels=94, max_iterations=100, coeff_stats=False, ﬁt_metrics=False, residuals=False, arma_roots=False, test_nonseasonal='ADF',
    test_seasonal='OCSB', output_fmt_index_style='NUMERICAL_SEQUENCE', **generic_arguments)
    DESCRIPTION:
        AutoArima() function searches the possible models within the order
        constrains in the function parameters, and returns the best ARIMA
        model based on the criterion provided by the "infor_criteria"
        parameter. AutoArima() function creates a six-layered ART table.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the time series whose value can be REAL.
            Types: TDSeries
        data_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data".
            Types: ColumnExpression
        max_pq_nonseasonal:
            Optional Argument.
            Specifies the (p,q) order of the maximum autoregression (AR) and
            moving average (MA) parameters.
            Default Value: [5,5]
            Types: list
        max_pq_seasonal:
            Optional Argument.
            Specifies the (P,Q) order of the max seasonal AR and MA
            parameters.
            Default Value: [2,2]
            Types: list
        start_pq_nonseasonal:
            Optional Argument.
            Specifies the start value of (p,q). Only used when "stepwise"=1.
            Default Value: [0,0]
            Types: list
        start_pq_seasonal:
            Optional Argument.
            Specifies the start value of seasonal (P,Q). Only used when
            "stepwise"=1.
            Default Value: [0,0]
            Types: list
        d:
            Optional Argument.
            Specifies the order of first-differencing.
            Default Value: -1 (auto search d).
            Types: int
        ds:
            Optional Argument.
            Specifies the order of seasonal-differencing.
            Default Value: -1 (auto search Ds).
            Types: int
        max_d:
            Optional Argument.
            Specifies the maximum number of non-seasonal differences.
            Default Value: 2
            Types: int
        max_ds:
            Optional Argument.
            Specifies the maximum number of seasonal differences.
            Default Value: 1
            Types: int
        period:
            Optional Argument.
            Specifies the number of periods per season. For non-seasonal
            data, period is 1.
            Default Value: 1
            Types: int
        stationary:
            Optional Argument.
            Specifies whether to restrict search to stationary models.
            If True, the  function restricts search to stationary models.
            Default Value: False
            Types: bool
        seasonal:
            Optional Argument.
            Specifies whether to restrict search to non-seasonal models.
            If False, then the function restricts search to non-seasonal
            models.
            Default Value: True
            Types: bool
        constant:
            Optional Argument.
            Specifies whether an indicator that AutoArima() function includes
            an intercept. If True, means CONSTANT/intercept
            should be included. If False, means
            CONSTANT/intercept should not be included.
            Default Value: True
            Types: bool
        algorithm:
            Optional Argument.
            Specifies the approach used by TD_AUTOARIMA to estimate the
            coefficients.
            Permitted Values:
                * MLE: Use maximum likelihood approach.
                * CSS_MLE: Use the conditional sum-of-squares to determine a
                            start value and then do maximum likelihood.
                * CSS: Use the conditional sum-of squares approach.
            Default Value: MLE
            Types: str
        fit_percentage:
            Optional Argument.
            Specifies the percentage of passed-in sample points used for the
            model fitting (parameter estimation).
            Default Value: 100
            Types: int
        infor_criteria:
            Optional Argument.
            Specifies the information criterion to be used in model selection.
            Permitted Values: AIC, AICC, BIC
            Default Value: AIC
            Types: str
        stepwise:
            Optional Argument.
            Specifies whether the function does stepwise selection or not.
            If True, then the function does stepwise selection otherwise the
            function selects all models.
            Default Value: False
            Types: bool
        nmodels:
            Optional Argument.
            Specifies the maximum number of models considered in the stepwise
            search.
            Default Value: 94
            Types: int
        max_iterations:
            Optional Argument.
            Specifies the maximum number of iterations that can be employed
            to non-linear optimization procedure.
            Default Value: 100
            Types: int
        coeff_stats:
            Optional Argument.
            Specifies the indicator to return coefficient statistical columns
            TSTAT_VALUE and TSTAT_PROB. If True, means return
            the columns otherwise do not return the
            columns.
            Default Value: False
            Types: bool
        fit_metrics:
            Optional Argument.
            Specifies the indicator to generate the secondary result set that
            contains the model metadata statistics. If True,
            means generate the secondary result set otherwise
            do not generate the secondary result set.
            Default Value: False
            Types: bool
        residuals:
            Optional Argument.
            Specifies the indicator to generate the tertiary result set that
            contains the model residuals. If True, means
            generate the tertiary result set, otherwise
            do not generate the tertiary result set.
            Default Value: False
            Types: bool
        arma_roots:
            Optional Argument.
            Specifies the indicator to generate the senary result set that
            contains the inverse AR and MA roots of result best 
            model that AutoArima() selected (the model in the
            primary output layer). There should be no inverse 
            roots showing outside of the unit circle. If True,
            means generate result set otherwise do not
            generate a result set.
            Default Value: False
            Types: bool
        test_nonseasonal:
            Optional Argument.
            Specifies the nonseasonal unit root test used to choose
            differencing number "d".
            AutoArima() function only uses ADF test for
            nonseasonal unit root test.
            Permitted Values: ADF
            Default Value: ADF
            Types: str
        test_seasonal:
            Optional Argument.
            Specifies the seasonal unit root test used to choose differencing 
            number "d". AutoArima() function only uses OCSB test for
            seasonal unit root test.
            Permitted Values: OCSB
            Default Value: OCSB
            Types: str
        output_fmt_index_style:
            Optional Argument.
            Specifies the index style of the output format.
            Permitted Values: NUMERICAL_SEQUENCE, FLOW_THROUGH
            Default Value: NUMERICAL_SEQUENCE
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an 
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results. 
                    If not specified, a unique table name is internally 
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output 
                    table into. If not specified, table is created into 
                    database specified by the user at the time of context 
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of AutoArima.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as AutoArima_obj.<attribute_name>.
        Output teradataml DataFrame attribute names are:
            1. result
            2. fitmetadata
            3. fitresiduals
            4. model
            5. icandorder
            6. armaroots
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage, before importing the
        #        function in user space.
        #     2. User can import the function, if it is available on
        #        Vantage user is connected to.
        #     3. To check the list of UAF analytic functions available
        #        on Vantage user connected to, use
        #        "display_analytic_functions()".
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Import function AutoArima.
        from teradataml import AutoArima
        # Load the example data.
        load_example_data("uaf", ["blood2ageandweight", "covid_confirm_sd"])
        # Create teradataml DataFrame object.
        data = DataFrame.from_table("blood2ageandweight")
        # Create teradataml TDSeries object.
        data_series_df = TDSeries(data=data,
                                  id="PatientID",
                                  row_index="SeqNo",
                                  row_index_style="SEQUENCE",
                                  payload_field="BloodFat",
                                  payload_content="REAL")
        # Example 1: Execute AutoArima with start_pq_nonseasonal as [1,1], algorithm = "MLE" and
        #            fit_percentage=80 to find the best ARIMA model.
        uaf_out = AutoArima(data=data_series_df,
                            start_pq_nonseasonal=[1, 1],
                            seasonal=False,
                            constant=True,
                            algorithm="MLE",
                            fit_percentage=80,
                            stepwise=True,
                            nmodels=7,
                            fit_metrics=True,
                            residuals=True)
        # Print the result DataFrames.
        print(uaf_out.result)
        # Example 2: Execute AutoArima with max_pq_nonseasonal as [3,3], arma_roots = True,
        #            to find thhe best ARIMA model.
        covid_confirm_sd = DataFrame("covid_confirm_sd")
        data_series_df = TDSeries(data=covid_confirm_sd,
                                  id="city",
                                  row_index="row_axis",
                                  row_index_style="SEQUENCE",
                                  payload_field="cnumber",
                                  payload_content="REAL")
        uaf_out = AutoArima(data=data_series_df,
                            max_pq_nonseasonal=[3, 3],
                            stationary=False,
                            stepwise=False,
                            arma_roots=True,
                            residuals=True)
        # Print the result DataFrames.
        print(uaf_out.result)
        print(uaf_out.fitresiduals)
        print(uaf_out.model)
        print(uaf_out.icandorder)
        print(uaf_out.armaroots)
    DIFF
    DIFF
    Functions
    DIFF(data=None, data_ﬁlter_expr=None, lag=None, differences=None, seasonal_multiplier=None, output_fmt_index_style='NUMERICAL_SEQUENCE',
    **generic_arguments)
    DESCRIPTION:
        The DIFF() function transforms a stationary, seasonal, or non-stationary
        time series into a differenced time series by performing both status-quo
        time series differencing, seasonal based differencing, and multiplicative
        transforms. Thus, the output of this transform function is always a new
        time series.
        The following procedure is an example of how to use DIFF() function:
            1. Detect the unit roots using DickeyFuller() function.
            2. Use DIFF() function to eliminate unit roots.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input time series with payload content as 'REAL' or 'MULTIVAR_REAL',
            or specifies the output of UNDIFF in ART Spec. When passed in a multivariate
            series, DIFF() function is executed separately against each identified series in
            the collection and produce a coalesced multivariate style analytical result set.
            Types: TDSeries, TDAnalyticResult
        data_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data".
            Types: ColumnExpression
        lag:
            Required Argument.
            Specifies the lag between the series elements.
            It accepts positive integer value, including zero.
            Types: int
        differences:
            Required Argument.
            Specifies the difference between time series elements
            'Yt' and 'Yt-lag'. It accepts positive integer value,
            including zero.
            Types: int
        seasonal_multiplier:
            Required Argument.
            Specifies whether a time series is seasonal or not.
            It accepts positive integer value, including zero.
            When set to 0, indicates time series is nonseasonal.
            Positive value indicates it is seasonal.
            The "seasonal_multiplier determines the formula to
            be used by function to transform each input time
            series element, 'Yt', to a differenced time series
            element, 'Ydt'.
            Types: int
        output_fmt_index_style:
            Optional Argument.
            Specifies the index style of the output format.
            Permitted Values: NUMERICAL_SEQUENCE
            Default Value: NUMERICAL_SEQUENCE
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an 
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results. 
                    If not specified, a unique table name is internally 
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output 
                    table into. If not specified, table is created into 
                    database specified by the user at the time of context 
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of DIFF.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as DIFF_obj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Load the example data.
        load_example_data("uaf", ["ocean_buoy2"])
        # Create teradataml DataFrame object.
        data = DataFrame.from_table("ocean_buoy2")
        # Example 1: Execute DIFF() function with TDSeries having
        #            REAL payload content to transform time series
        #            into a differenced time series.
        # Create teradataml TDSeries object.
        data_series_df_real = TDSeries(data=data,
                                       id="buoy_id",
                                       row_index="n_seq_no",
                                       row_index_style= "SEQUENCE",
                                       payload_field="magnitude1",
                                       payload_content="REAL")
        uaf_out_1 = DIFF(data=data_series_df_real,
                         lag=1,
                         differences=2,
                         seasonal_multiplier=0)
        # Print the result DataFrame.
        print(uaf_out_1.result)
        # Example 2: Execute DIFF() function with TDSeries having
        #            MULTIVAR_REAL payload content to transform time
        #            series into a differenced time series.
        # Create teradataml TDSeries object.
        data_series_df_multivar = TDSeries(data=data,
                                           id="buoy_id",
                                           row_index="n_seq_no",
                                           row_index_style= "SEQUENCE",
                                           payload_field=["magnitude1", "magnitude2"],
                                           payload_content="MULTIVAR_REAL")
        uaf_out_2 = DIFF(data=data_series_df_multivar,
                         lag=1,
                         differences=2,
                         seasonal_multiplier=0)
        # Print the result DataFrame.
        print(uaf_out_2.result)
    LinearRegr
    LinearRegr
    Functions
    LinearRegr(data=None, data_ﬁlter_expr=None, variables_count=2, weights=False, formula=None, algorithm=None, coeff_stats=False, conf_int_level=0.9,
    model_stats=False, residuals=False, **generic_arguments)
    DESCRIPTION:
        The LinearRegr() function is a simple linear regression function.
        It fits data to a curve using a formula that defines 
        the relationship between the explanatory variable and the 
        response variable.
        The following procedure is an example of how to use 
        LinearRegr() to develop an ARIMA model:
            * Determine that the series to be modeled includes a trend.
            * Use LinearRegr() to remove the trend from the series.
            * Use the "fitmetadata" attribute from the function output,
              to determine the trend by fitting the data set.
            * Use GenseriesFormula() to generate a trend series.
            * Use BinarySeriesOp() to subtract the generated trend
              from the original series.
    PARAMETERS:
        data:
            Required Argument.
            Specifies an input time series with the following payload characteristics:
                * "payload_content" value is MULTIVAR_REAL.
                * "payload_fields" has two required fields (response variable and 
                  explanatory variable, in that order) and one optional 
                  field (weights).
            Types: TDSeries
        data_filter_expr:
            Optional Argument.
            Specifies filter expression for "data".
            Types: ColumnExpression
        variables_count:
            Optional Argument.
            Specifies the number of parameters present
            in the payload. For linear regression with no weighting, 
            there are 2 parameters (the response variable and the explanatory
            variable). For linear regression with weighting, there are
            3 variables (the response variable, the explanatory variable,
            and the weights).
            Default Value: 2
            Permitted Values: 2, 3
            Types: int
        weights:
            Optional Argument.
            Specifies whether a third series is present
            in MULTIVAR series-specifications. The third series is
            interpreted as a series of weights that can be used to 
            perform a weighted least-squares regression solution.
            When set to False, no third series is present, 
            otherwise it is present.
            Default Value: False
            Types: bool
        formula:
            Required Argument.
            Specifies the formula that is to be used in the regression operation.
            The formula defines the relationship between the explanatory
            variable and the response variable and, conforms to Formula Rules.
            Note:
                Use the following link to refer the formula rules in Teradata document(func_param):
                "https://docs.teradata.com/r/4k28qKyhFXQ3DA~TULEIuw/Yp9oQ2nOzr70tKke4rCiAQ"
            Types: str
        algorithm:
            Required Argument.
            Specifies the algorithm used for the regression.
            Permitted Values:
                1. QR: means that QR decomposition is used for the regression.
                2. PSI: means that pseudo-inverse based on singular value
                   decomposition (SVD) is used to solve the regression.
            Types: str
        coeff_stats:
            Optional Argument.
            Specifies whether to include coefficient statistics columns in the results.
            When set to False, coefficient statistics columns are not included in 
            the results, otherwise, columns are included in the results.
            Default Value: False
            Types: bool
        conf_int_level:
            Optional Argument.
            Specifies the confidence interval level value used for coefficient
            statistics calculation. The value is greater than 0 and less than 1. 
            Note:
                Applicable only when "coeff_stats" is set to True.
            Default Value: 0.9
            Types: int OR float
        model_stats:
            Optional Argument.
            Specifies whether to generate the optional model statistics and
            available to access using the attribute "fitmetadata" of the function
            output. When set to True, function generates the model statistics,
            otherwise not.
            Default Value: False
            Types: bool
        residuals:
            Optional Argument.
            Specifies whether to generate the tertiary (residuals)
            layer and available to access using the attribute "fitresiduals" of
            the function output. When set to True, function generates the layer,
            Otherwise not.
            Default Value: False
            Types: bool
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an 
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results. 
                    If not specified, a unique table name is internally 
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output 
                    table into. If not specified, table is created into 
                    database specified by the user at the time of context 
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of LinearRegr.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as LinearRegr_obj.<attribute_name>.
        Output teradataml DataFrame attribute names are:
            1. result
            2. fitmetadata - Available when "model_stats" is set to True, otherwise not.
            3. fitresiduals - Available when "residuals" is set to True, otherwise not.
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Load the example data.
        load_example_data("uaf", ["house_values2"])
        # Create teradataml DataFrame object.
        data = DataFrame.from_table("house_values2")
        # Create teradataml TDSeries object.
        data_series_df = TDSeries(data=data,
                                  id="cid",
                                  row_index_style="SEQUENCE",
                                  row_index="s_no",
                                  payload_field=["house_value", "salary"],
                                  payload_content="MULTIVAR_REAL")
        # Example 1: The LinearRegr() function fits TDSeries data to
        #             the curve mentioned in the "formula." It returns
        #             a result containing solved coefficients, model statistics,
        #             and residuals statistics.
        uaf_out = LinearRegr(data=data_series_df,
                             variables_count=2,
                             weights=False,
                             formula="Y=B0+B1*X1",
                             algorithm='QR',
                             model_stats=True, 
                             coeff_stats=False,
                             residuals=True)
        # Print the result DataFrames.
        print(uaf_out.result)
        # Print the model statistics result.
        print(uaf_out.fitmetadata)
        # Print the residuals statistics result.
        print(uaf_out.fitresiduals)
    MultivarRegr
    MultivarRegr
    Functions
    MultivarRegr(data=None, data_ﬁlter_expr=None, variables_count=None, weights=False, formula=None, algorithm=None, coeff_stats=False, conf_int_level=0.9,
    model_stats=False, residuals=False, **generic_arguments)
    DESCRIPTION:
        The MultivarRegr() function is a multivariate linear regression function.
        Using a formula that defines the relationship between the explanatory
        variable and multiple response variables, it fits data to a multidimensional surface.
    PARAMETERS:
        data:
            Required Argument.
            Specifies series with payload characteristics as follows:
                * Payload content value is MULTIVAR_REAL.
                * Payload field value has "variables_count" required fields (response
                  variable followed by explanatory variables).
            Types: TDSeries
        data_filter_expr:
            Optional Argument.
            Specifies filter expression for "data".
            Types: ColumnExpression
        variables_count:
            Required Argument.
            Specifies how many parameters are present in the payload. For
            linear regression with no weighting, there are 2 variables (the response
            variable and the explanatory variable). For linear regression with weighting,
            there are 3 variables (the response variable, the explanatory variable, and
            the weights).
            Types: int
        weights:
            Optional Argument.
            Specifies whether a third series is present in MULTIVAR series-specifications.
            The third series is interpreted as a series of weights that can be used to
            perform a weighted least-squares regression solution.
            When set to False, no third series is present, otherwise it is present.
            Default Value: False
            Types: bool
        formula:
            Required Argument.
            Specifies the relationship between the explanatory variable and the response variables.
            Types: str
        algorithm:
            Required Argument.
            Specifies algorithm used for the regression.
            Permitted Values:
                1. QR: means that QR decomposition is used for the regression.
                2. PSI: means that pseudo-inverse based on singular value
                   decomposition (SVD) is used to solve the regression.
            Types: str
        coeff_stats:
            Optional Argument.
            Specifies whether to include coefficient statistics columns in the output or not.
            When set to False, coefficient statistics columns are not returned in the output,
            otherwise columns are returned in the output.
            Default Value: False
            Types: bool
        conf_int_level:
            Optional Argument.
            Specifies the confidence interval level value used for coefficient statistics
            calculation.
            The value should be greater than 0 and less than 1.
            Note:
                Applicable only when "coeff_stats" is set to 1.
            Default Value: 0.9
            Types: int OR float
        model_stats:
            Optional Argument.
            Specifies whether to generate the optional model statistics.
            The generated result set can be retrieved using the attribute
            fitmetadata of the function output. When set to False, function
            does not generate the statistics, otherwise generates it.
            Default Value: False
            Types: bool
        residuals:
            Optional Argument.
            Specifies whether to generate the tertiary (residuals) layer.
            The generated result set can be retrieved using the attribute
            fitresiduals of the function output. when set to False, function
            does not generate the residual data, otherwise generates it.
            Default Value: False
            Types: bool
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions. 
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an 
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results. 
                    If not specified, a unique table name is internally 
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output 
                    table into. If not specified, table is created into 
                    database specified by the user at the time of context 
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of MultivarRegr.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as MultivarRegr_obj.<attribute_name>.
        Output teradataml DataFrame attribute names are:
            1. result
            2. fitmetadata - Available when "model_stats" is set to True, otherwise not.
            3. fitresiduals - Available when "residuals" is set to True, otherwise not.
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Load the example data.
        load_example_data("uaf", ["house_values"])
        # Create teradataml DataFrame object.
        df = DataFrame.from_table("house_values")
        # Create teradataml TDSeries object.
        data_series_df = TDSeries(data=df,
                                  id="cityid",
                                  row_index="TD_TIMECODE",
                                  payload_field=["house_val","salary","mortgage"],
                                  payload_content="MULTIVAR_REAL")
        # Example 1 : Execute multivariate regression function to identify the degree of
        #             linearity the explanatory variable and multiple response variables.
        #             Generate the model statistics and residual data as well.
        uaf_out = MultivarRegr(data=data_series_df,
                               variables_count=3,
                               weights=False,
                               formula="Y = B0 + B1*X1 + B2*X2",
                               algorithm='QR',
                               coeff_stats=True,
                               model_stats=True,
                               residuals=True)
        # Print the result DataFrames.
        print(uaf_out.result)
        print(uaf_out.fitmetadata)
        print(uaf_out.fitresiduals)
    PACF
    PACF
    Functions
    PACF(data=None, data_ﬁlter_expr=None, input_type=None, algorithm=None, max_lags=None, unbiased=False, alpha=None, **generic_arguments)
    DESCRIPTION:
        The PACF() function provides insight as to whether the function
        being modeled is stationary or not. The partial auto correlations
        are used to measure the degree of correlation between series sample points.
        The algorithm removes the effects of the previous lag. For example,
        the coefficient for lag 4 focuses on the effect of activity based only
        at lag 4, with effects of lags 3, 2, and 1 removed.
    PARAMETERS:
        data:
            Required Argument.
            Specifies a series or an analytical result that contains previously
            computed auto correlation coefficients for lag and magnitude.
            Types: TDSeries, TDAnalyticResult
        data_filter_expr:
            Optional Argument.
            Specifies filter expression for "data".
            Types: ColumnExpression
        input_type:
            Optional Argument.
            Specifies the type of data in the series.
            Permitted Values:
                * DATA_SERIES: A one dimensional input array that contains
                               a time series or a spatial series.
                * ACF: A one dimensional input array that is indexed by
                       LAG values, and contains previously-generated ACF magnitudes.
            Types: str
        algorithm:
            Required Argument.
            Specifies the algorithm to generate the partial auto-correlation function
            "PACF" coefficients.
            Permitted Values: LEVINSON_DURBIN, OLS
            Types: str
        max_lags:
            Required Argument.
            Specifies the maximum number of lags to calculate the partial autocorrelation.
            The lag value is limited to one less than the number of observations in the series.
            If the specified lag value exceeds the limit, the value is replaced with the
            system-defined maximum value.
            Default is 10*log10(N) where N is the number of observations.
            Types: int
        unbiased:
            Optional Argument.
            Specifies the formula to calculate the autocorrelation intermediate values.
            When set to False, denominator for autocorrelation calculation uses the
            Jenkins & Watts formula, otherwise uses the Box & Jenkins formula.
            Note:
                Only valid when "input_type" is 'DATA_SERIES'.
            Default Value: False
            Types: bool
        alpha:
            Optional Argument.
            Specifies confidence intervals for the given level. For example, if 0.05 is entered,
            then 95% confidence intervals are returned for standard deviation computed according
            to Bartlett’s formula.
            Types: int OR float
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions. 
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an 
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results. 
                    If not specified, a unique table name is internally 
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output 
                    table into. If not specified, table is created into 
                    database specified by the user at the time of context 
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of PACF.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as PACF_obj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Load the example data.
        load_example_data("uaf", ["test_pacf_12"])
        # Create teradataml DataFrame object.
        df = DataFrame.from_table("test_pacf_12")
        # Create teradataml TDSeries object.
        data_series_df = TDSeries(data=df,
                                  id="buoy_id",
                                  row_index="n_seq_no",
                                  row_index_style="SEQUENCE",
                                  payload_field="magnitude1",
                                  payload_content="REAL")
        # Example 1 : Calculate the partial autocorrelation function coefficients using
        #             'LEVINSON_DURBIN' algorithm, with maximum of 10 lags.
        PACF_out = PACF(data=data_series_df,
                        algorithm='LEVINSON_DURBIN',
                        max_lags=10)
        # Print the result DataFrame.
        print(PACF_out.result)
    PowerTransform
    PowerTransform
    Functions
    PowerTransform(data=None, data_ﬁlter_expr=None, back_transform=False, p=None, b=None, lambda1=None,
    output_fmt_index_style='NUMERICAL_SEQUENCE', **generic_arguments)
    DESCRIPTION:
        The PowerTransform() function takes a time series or numerically-sequenced
        series and applies a power transform to the series to produce a one-dimensional
        array. The passed-in series can be either a univariate or multivariate series.
        This function is useful for transforming an input series that has heteroscedastic 
        variance into a result series that has homoscedastic variance.
        User can use the new time series to build an ARIMA forecasting model.
        The following procedure is an example of how to get forecast values
        for a heteroscedastic time series using PowerTransform() function:
            * Apply PowerTransform() function to the heteroscedastic time series.
            * Use the resulting homoscedastic time series to build an ARIMA forecasting model.
            * Use the model to produce the initial forecast of the homoscedastic time series.
            * Use the backward transform on the initial forecast to extract the forecast
              values for the heteroscedastic time series.
    PARAMETERS:
        data:
            Required Argument.
            Specifies an input series whose payload content value can be
            REAL or MULTIVAR_REAL.
            Types: TDSeries
        data_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data".
            Types: ColumnExpression
        back_transform:
            Optional Argument.
            Specifies whether to apply back transform. 
            When set to False, black transform is not applied, otherwise it is applied.
            Default Value: False
            Types: bool
        p:
            Required Argument.
            Specifies the power to use in the transform equation.
            Types: int OR float
        b:
            Required Argument.
            Specifies the logarithm to be applied for the transform equation.
            Types: int OR float
        lambda1:
            Required Argument.
            Specifies the parameter used to decide the preferred
            power transform operation during the Box-Cox transformation.
            Types: int OR float
        output_fmt_index_style:
            Optional Argument.
            Specifies the index style of the output format.
            Permitted Values: NUMERICAL_SEQUENCE, FLOW_THROUGH
            Default Value: NUMERICAL_SEQUENCE
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an 
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results. 
                    If not specified, a unique table name is internally 
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output 
                    table into. If not specified, table is created into 
                    database specified by the user at the time of context 
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of PowerTransform.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as PowerTransform_obj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Load the example data.
        load_example_data("uaf", ["production_data"])
        # Create teradataml DataFrame object.
        data = DataFrame.from_table("production_data")
        # Create teradataml TDSeries object.
        data_series_df = TDSeries(data=data,
                                  id="product_id",
                                  row_index="TD_TIMECODE",
                                  payload_field=["beer_sales", "wine_sales"],
                                  payload_content="MULTIVAR_REAL")
        # Example 1: The function returns the results, which transforms
        #            heteroscedastic time series to homoscedastic time series.
        uaf_out = PowerTransform(data=data_series_df,
                                 back_transform=True,
                                 p=0.0,
                                 b=0.0,
                                 lambda1=0.5)
        # Print the result DataFrame.
        print(uaf_out.result)
    SeasonalNormalize
    SeasonalNormalize
    Functions
    SeasonalNormalize(data=None, data_ﬁlter_expr=None, season_cycle=None, cycle_duration=None, season_info=0,
    output_fmt_index_style='NUMERICAL_SEQUENCE', **generic_arguments)
    DESCRIPTION:
        SeasonalNormalize() takes a non-stationary series and normalizes the
        series by first dividing the series into cycles and intervals, then averaging
        and normalizing with respect to each interval over all cycles.
        This form of normalization is effective relative to eliminating
        non-stationary properties such as unit roots and periodicities.
        The following procedure is an example of how to use SeasonalNormalize():
            1. Detect the unit roots using DickeyFuller().
            2. Use SeasonalNormalize() to create a series with potentially the unit roots eliminated.
            3. Use DickeyFuller() to verify that unit roots were eliminated from the newly-formed normalized series.
            4. Use ArimaEstimate() and ArimaValidate() to create an ARIMA model from the normalized series.
            5. Use ArimaForecast() to forecast the normalized series.
            6. Use Unnormalize() passing in the forecasted series and the original unnormalized series to
               produce a forecasted series with the effects of SeasonalNormalize() removed.
    PARAMETERS:
        data:
            Required Argument.
            Single input source that contains univariate series instances.
            The associated "payload_content" is 'REAL'.
            The payload is the series element magnitude.
            TDSeries must include the usage of the "interval" parameter
            that is the interval to used by the function to divide the
            series cycles into intervals.
            Types: TDSeries
        data_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data".
            Types: ColumnExpression
        season_cycle:
            Required Argument.
            Specifies the logical time-unit.
            Permitted Values:
                * CAL_YEARS
                * CAL_MONTHS
                * CAL_DAYS
                * WEEKS
                * DAYS
                * HOURS
                * MINUTES
                * SECONDS
                * MILLISECONDS
                * MICROSECONDS
            Types: str
        cycle_duration:
            Required Argument.
            Specifies the number of time-units as the duration of each seasonal
            cycle.
            Types: int
        season_info:
            Optional Argument.
            Specifies whether to generate additional columns CYCLE_NO and
            SEASON_NO. CYCLE_NO is the n-th cycle of the season. SEASON_NO
            is the season for the data.
            Permitted Values:
                * 0 indicates no extra columns being generated.
                * 1 indicates SEASON_NO column being generated.
                * 2 indicates CYCLE_NO column being generated.
                * 3 indicates both SEASON_NO and CYCLE_NO columns being generated.
            Default Value: 0
            Types: int
        output_fmt_index_style:
            Optional Argument.
            Specifies the index style of the output format.
            Default Value: NUMERICAL_SEQUENCE
            Permitted Values:
                * NUMERICAL_SEQUENCE
                * FLOW_THROUGH
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an 
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results. 
                    If not specified, a unique table name is internally 
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output 
                    table into. If not specified, table is created into 
                    database specified by the user at the time of context 
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of SeasonalNormalize.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as SeasonalNormalize_obj.<attribute_name>.
        Output teradataml DataFrame attribute names are:
            1. result
            2. metadata
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Load the example data.
        load_example_data("uaf", ["river_data"])
        # Create teradataml DataFrame object.
        data = DataFrame.from_table("river_data")
        # Create teradataml TDSeries object.
        data_series_df = TDSeries(data=data,
                                  id="station_id",
                                  row_index="timevalue",
                                  row_index_style="TIMECODE",
                                  payload_field="flow_velocity",
                                  payload_content="REAL",
                                  interval="CAL_MONTHS(1)")
        # Example 1 : Normalize the series by removing the unit roots.
        uaf_out = SeasonalNormalize(data=data_series_df,
                                    season_cycle="CAL_MONTHS",
                                    cycle_duration=1)
        # Print the result DataFrames.
        print(uaf_out.result)
        print(uaf_out.metadata)
    Smoothma
    Smoothma
    Functions
    Smoothma(data=None, data_ﬁlter_expr=None, ma=None, window=None, one_sided=False, lambda1=None, weights=None, well_known=None, pad=None,
    output_fmt_index_style='NUMERICAL_SEQUENCE', **generic_arguments)
    DESCRIPTION:
        The Smoothma() function applies a smoothing function to a time series
        which results in a series that highlights the time series mean. For
        non-stationary time series with non-constant means, the smoothing
        function is used to create a result series. When the result series
        is subtracted from the original series, it removes the non-stationary
        mean behavior.
        User can use the new time series to build an ARIMA forecasting model.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input series.
            Note:
                * Payload content value of the series must be REAL or MULTIVAR_REAL.
            Types: TDSeries
        data_filter_expr:
            Optional Argument.
            Specifies filter expression for "data".
            Types: ColumnExpression
        ma:
            Required Argument.
            Specifies the type of moving average algorithm to use.
            Permitted Values:
                * CUMULATIVE: Cumulative moving average.
                * MEAN: Simple Moving Average with the arguments "window", "one_sided",
                        "pad", "well_known", "weights".
                * MEDIAN: Simple moving median with the arguments "window", "pad".
                * EXPONENTIAL: Exponential moving average with the argument "lambda1".
            Types: str
        window:
            Optional Argument.
            Specifies the order (window size) of the moving average to be applied.
            Odd numbers use the Simple Moving Average formula. Even numbers use
            the Centered Moving Average formula.
            For example, window='6' with ma='MEAN' uses the Centered Moving average
            algorithm.
            Note:
                * Applicable only when "ma" is set to 'MEAN' or 'MEDIAN'.
            Types: int
        one_sided:
            Optional Argument.
            Specifies whether to use centering or not. When set to False, it means use
            centering, otherwise do not use centering. The last "window" entries are
            averaged to form the current entry.
            Note:
                * Applicable only when "ma" is set to 'MEAN'.
            Default Value: False
            Types: bool
        lambda1:
            Optional Argument.
            Specifies a value between 0 and 1, which represents the degree of weighting
            decrease. A higher "lambda1" has the effect of discounting older observations
            faster.
            Note:
                * Applicable only when "ma" is set to 'EXPONENTIAL'.
            Types: int OR float
        weights:
            Optional Argument.
            Specifies the list of weights to be applied when calculating the moving average.
            The weights should sum up to 1, and be symmetric. If the weights don’t sum up to 1,
            then the sum from the list of weights divides each element in the list to get
            a fraction for each element to achieve the same effect of the sum up to 1.
            The number of elements in the "weights" list must match the "window" value.
            Function uses the number of the "weights" element as the default "window"
            value if it is not specified.
            Note:
                * Applicable only when "ma" is set to 'MEAN'.
            Types: int OR float
        well_known:
            Optional Argument.
            Specifies one of the supported well-known weighted MA combinations to be applied to
            the input series.
            If "window" is not specified, then the function provides the value as follows:
                * For 3MA, "window" value is 3.
                * For 3X3MA, and H5MA, "window" value is 5.
                * For 3X5MA, "window" value is 7.
                * For H9MA, "window" value is 9.
                * For 2X12MA, and H13MA, "window" value is 13.
                * For S15MA, "window" value is 15.
                * For S21MA, "window" value is 21.
                * For H23MA, "window" value is 23.
            Notes:
                * "well_known" and "weights" must be used in a mutually exclusive fashion.
                * Applicable only to SMA.
            Permitted Values: 3MA, 5MA, 2x12MA, 3x3MA, 3x5MA,
                              S15MA, S21MA, H5MA, H9MA, H13MA, H23MA
            Types: str
        pad:
            Optional Argument.
            Specifies the produced output series has the magnitudes set to PAD value for
            an element less than "window".
            For example, pad=4.5 applies a pad value of 4.5 for a series less than "window".
            Note:
                * Applicable only when "ma" is set to 'MEAN' or 'MEDIAN'.
            Types: int, float
        output_fmt_index_style:
            Optional Argument.
            Specifies the index style of the output format.
            Default Value: NUMERICAL_SEQUENCE
            Permitted Values: NUMERICAL_SEQUENCE, FLOW_THROUGH
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an 
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results. 
                    If not specified, a unique table name is internally 
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output 
                    table into. If not specified, table is created into 
                    database specified by the user at the time of context 
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of Smoothma.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as Smoothma_obj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Load the example data.
        load_example_data("uaf", ["orders1_12"])
        # Create teradataml DataFrame object.
        df=DataFrame.from_table("orders1_12")
        # Create teradataml TDSeries object.
        data_series_df= TDSeries(data=df,
                                 id="OrderID",
                                 row_index="SEQ",
                                 row_index_style="SEQUENCE",
                                 payload_field="Qty1",
                                 payload_content="REAL")
        # Example 1 :  Perform exponential moving average.
        uaf_out = Smoothma(data=data_series_df,
                           ma='EXPONENTIAL',
                           lambda1=0.5)
        # Print the result DataFrame.
        print(uaf_out.result)
    UNDIFF
    UNDIFF
    Functions
    UNDIFF(data1=None, data1_ﬁlter_expr=None, data2=None, data2_ﬁlter_expr=None, lag=None, differences=None, seasonal_multiplier=None,
    initial_values=None, input_fmt_input_mode=None, **generic_arguments)
    DESCRIPTION:
        The UNDIFF() function is the reverse of the DIFF() function.
        It takes in a previously differenced series processed by DIFF(),
        and produces the original series that existed prior to the differencing.
    PARAMETERS:
        data1:
            Required Argument.
            Specifies the differenced series or TDAnalyticResult
            object created on the output of DIFF() function.
            Types: TDSeries, TDAnalyticResult
        data1_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data1".
            Types: ColumnExpression
        data2:
            Optional Argument.
            Specifies the original series.
            This series is needed to reconstruct the series completely.
            If the series was differenced with a lag of 1, then the initial
            value of the original series must be present for a full
            reconstruction. With a lag of 2, the initial 2 values must be present,
            and so on. If the series was differenced multiple times, then the
            initial values of the intermediate steps must be given.
            Types: TDSeries
        data2_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data2".
            Types: ColumnExpression
        lag:
            Required Argument.
            Specifies the lag between series elements.
            Value must be greater than or equal to 0.
            Types: int
        differences:
            Required Argument.
            Specifies the difference between time series elements Yt and Yt-lag.
            Value must be greater than or equal to 0.
            Types: int
        seasonal_multiplier:
            Required Argument.
            Specifies whether time series is seasonal.
            When set to 0, indicates time series is nonseasonal,
            otherwise indicates seasonal time series.
            Value must be greater than or equal to 0.
            Types: int
        initial_values:
            Optional Argument.
            Specifies the starting values for the undifferencing operation.
            Types: int, list of int, float OR list of float
        input_fmt_input_mode:
            Optional Argument.
            Specifies the input mode supported by the function.
            Permitted Values: MANY2ONE, ONE2ONE, MATCH
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results.
                    If not specified, a unique table name is internally
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output
                    table into. If not specified, table is created into
                    database specified by the user at the time of context
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of UNDIFF.
        Output teradataml DataFrames can be accessed using attribute
        references, such as UNDIFF_obj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Load the example data.
        load_example_data("uaf", ["ocean_buoy2"])
        # Create teradataml DataFrame objects.
        data = DataFrame.from_table("ocean_buoy2")
        # Create teradataml TDSeries object.
        data_series = TDSeries(data=data,
                               id="buoy_id",
                               row_index="n_seq_no",
                               row_index_style= "SEQUENCE",
                               payload_field="magnitude1",
                               payload_content="REAL")
        # Transform time series into a differenced time series.
        uaf_out_1 = DIFF(data=data_series,
                         lag=1,
                         differences=1,
                         seasonal_multiplier=0)
        # Example 1 : Retrieve the original series that existed prior to the differencing
        #             by taking the differenced series processed by DIFF() as input.
        # Create teradataml TDSeries over the output generated from DIFF.
        data_series_1 = TDSeries(data=uaf_out_1.result,
                                 id="buoy_id",
                                 row_index="ROW_I",
                                 row_index_style= "SEQUENCE",
                                 payload_field="OUT_magnitude1",
                                 payload_content="REAL")
        uaf_out = UNDIFF(data1=data_series_1,
                         data2=data_series,
                         lag=1,
                         differences=1,
                         seasonal_multiplier=0,
                         input_fmt_input_mode="MATCH")
        # Print the result DataFrame.
        print(uaf_out.result)
    Unnormalize
    Unnormalize
    Functions
    Unnormalize(data1=None, data1_ﬁlter_expr=None, data2=None, data2_ﬁlter_expr=None, ﬁelds=None, input_fmt_input_mode=None,
    output_fmt_index_style='NUMERICAL_SEQUENCE', **generic_arguments)
    DESCRIPTION:
        The Unnormalize() function reconstructs a series created by SeasonalNormalize().
        The function is usually used for the forecasting phase of modeling.
        The following procedure is an example of how to use function when the series
        to be modeled is found to be unstationary:
            * Use SeasonalNormalize() to make the series stationary.
            * Develop the ARIMA forecast model.
            * Use the ARIMA model to forecast the normalized series.
            * Use function on the forecasted normalized series to undo the
              effects of normalization and produce the final forecasted series result.
    PARAMETERS:
        data1:
            Required Argument.
            Specifies the input series to unnormalize.
            Input payload content type can be REAL, MUTLIVAR_REAL, or
            MUTLTIVAR_ANYTYPE. TDSeries must include "interval" argument.
            Types: TDSeries
        data1_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data1".
            Types: ColumnExpression
        data2:
            Required Argument.
            Specifies the output series. Output series type matches input series type.
            This should be a metadata output of SeasonalNormalize() function or
            TDAnalyticResults created on the teradataml dataframe that contains
            previously normalized results with the (mean, standardDeviation) pairs
            needed to unnormalize the first input series.
            Types: TDSeries, TDAnalyticResult
        data2_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data2".
            Types: ColumnExpression
        fields:
            Optional Argument.
            Specifies fields to unnormalize. The fields should be separated by commas.
            When specified, only the specified payload fields in the list are unnormalized,
            otherwise all payload fields of the first input are unnormalized.
            Values should be 0 or positive numbers. The payload field indices start
            at 1.
            Types: int
        input_fmt_input_mode:
            Required Argument.
            Specifies the input mode supported by the function.
            Permitted Values: MANY2ONE, ONE2ONE, MATCH
            Types: str
        output_fmt_index_style:
            Optional Argument.
            Specifies the INDEX_STYLE of the output format.
            Permitted Values: NUMERICAL_SEQUENCE, FLOW_THROUGH
            Default Value: NUMERICAL_SEQUENCE
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an 
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results. 
                    If not specified, a unique table name is internally 
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output 
                    table into. If not specified, table is created into 
                    database specified by the user at the time of context 
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of Unnormalize.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as Unnormalize_obj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Load the example data.
        load_example_data("uaf", ["store_sales"])
        # Create teradataml DataFrame objects.
        df = DataFrame("store_sales")
        # Create teradataml TDSeries objects.
        td_series = TDSeries(data=df,
                             row_index="ts",
                             row_index_style="TIMECODE",
                             id="StoreID",
                             payload_field="Sales",
                             payload_content="REAL",
                             interval="CAL_MONTHS(1)"
                             )
        # Produce the stationary series.
        seasonalnormalize = SeasonalNormalize(data=td_series,
                                              season_cycle="CAL_YEARS",
                                              cycle_duration=1,
                                              output_fmt_index_style="FLOW_THROUGH"
                                              )
        # Create teradataml TDSeries objects.
        td_series1 = TDSeries(data=seasonalnormalize.result,
                              id="StoreID",
                              row_index="ROW_I",
                              row_index_style="TIMECODE",
                              payload_field="Sales",
                              payload_content="REAL",
                              interval="CAL_MONTHS(1)"
                              )
        # Example 1 : Function reverse the effects of normalization and
        #             produce the final forecasted series result using TDSeries.
        # Create teradataml TDSeries objects.
        td_series2 = TDSeries(data=seasonalnormalize.metadata,
                              id="StoreID",
                              row_index="ROW_I",
                              row_index_style="SEQUENCE",
                              payload_field=["MEAN_Sales", "SD_Sales"],
                              payload_content="MULTIVAR_REAL"
                              )
        uaf_out = Unnormalize(data1=td_series1,
                              data2=td_series2,
                              input_fmt_input_mode="MATCH",
                              output_fmt_index_style="FLOW_THROUGH")
        # Print the result DataFrame.
        print(uaf_out.result)
        # Example 2 : Function reverse the effects of normalization and
        #             produce the final forecasted series result using TDAnalyticResult.
        # Create teradataml TDAnalyticResult objects.
        td_art = TDAnalyticResult(data=seasonalnormalize.result,
                                  layer='ARTMETADATA')
        uaf_out = Unnormalize(data1=td_series1,
                              data2=td_art,
                              input_fmt_input_mode="MATCH",
                              output_fmt_index_style="FLOW_THROUGH")
        # Print the result DataFrame.
        print(uaf_out.result)
    SERIES FORECASTING functions
    ArimaForecast
    ArimaForecast
    Functions
    ArimaForecast(data=None, data_ﬁlter_expr=None, forecast_periods=None, output_fmt_index_style='NUMERICAL_SEQUENCE', **generic_arguments)
    DESCRIPTION:
        The ArimaForecast() function is used to forecast a user-defined number of periods based on
        models fitted from the ArimaEstimate() function.
        The following procedure is an example of how to use ArimaForecast:
            1. Run the ArimaEstimate() function to get the coefficients for the ARIMA model.
            2. Run the ArimaValidate() function to validate the "goodness of fit" of the ARIMA model,
               when "fit_percentage" is not 100 in ArimaEstimate.
            3. Run the ArimaForecast() function with input from step 1 or step 2 to
               forecast the future periods beyond the last observed period.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the TDAnalyticResult object over the output
            generated by either ArimaEstimate() or ArimaValidate() function.
            Types: TDAnalyticResult
        data_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data".
            Types: ColumnExpression
        forecast_periods:
            Required Argument.
            Specifies the number of periods to forecast ahead.
            Types: int
        output_fmt_index_style:
            Optional Argument.
            Specifies the index style of the output format.
            Permitted Values: NUMERICAL_SEQUENCE, FLOW_THROUGH
            Default Value: NUMERICAL_SEQUENCE
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results.
                    If not specified, a unique table name is internally
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output
                    table into. If not specified, table is created into
                    database specified by the user at the time of context
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of ArimaForecast.
        Output teradataml DataFrames can be accessed using attribute
        references, such as ArimaForecast_obj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Load the example data.
        load_example_data("uaf", ["timeseriesdatasetsd4"])
        # Create teradataml DataFrame object.
        data = DataFrame.from_table("timeseriesdatasetsd4")
        # Execute ArimaEstimate() function to estimate the coefficients
        # and statistical ratings corresponding to an ARIMA model.
        # Create teradataml TDSeries object.
        data_series_df = TDSeries(data=data,
                                  id="dataset_id",
                                  row_index="seqno",
                                  row_index_style="SEQUENCE",
                                  payload_field="magnitude",
                                  payload_content="REAL")
        # Example 1: Forecast 2 periods based on the model fitted by ArimaEstimate.
        #            As the fit_percentage is greater than or equal to 100,
        #            output of ArimaEstimate is used for ArimaForecast.
        # Execute ArimaEstimate function.
        arima_estimate_op = ArimaEstimate(data1=data_series_df,
                                          nonseasonal_model_order=[2,0,0],
                                          constant=False,
                                          algorithm="MLE",
                                          coeff_stats=True,
                                          fit_metrics=True,
                                          residuals=True,
                                          fit_percentage=100)
        # Create teradataml TDAnalyticResult object over the result attribute of 'arima_estimate_op'
        data_art_df = TDAnalyticResult(data=arima_estimate_op.result)
        uaf_out = ArimaForecast(data=data_art_df, 
                                forecast_periods=2)
        # Print the result DataFrame.
        print(uaf_out.result)
    # Example 2: Forecast 2 periods based on the model fitted by ArimaValidate.
    #            As the fit_percentage is less than 100,
    #            output of ArimaEstimate is used for ArimaValidate and 
    #            output of ArimaValidate is used for ArimaForecast.
    # Execute ArimaEstimate function.
    arima_estimate_op = ArimaEstimate(data1=data_series_df,
                                      nonseasonal_model_order=[2,0,0],
                                      constant=False,
                                      algorithm="MLE",
                                      coeff_stats=True,
                                      fit_metrics=True,
                                      residuals=True,
                                      fit_percentage=80)
    # Create TDAnalyticResult object over the result attribute of 'arima_estimate_op'
    data_art_df = TDAnalyticResult(data=arima_estimate_op.result)
    # Execute ArimaValidate function.
    arima_validate_op = ArimaValidate(data=data_art_df,
                                      fit_metrics=TRUE,
                                      residuals=TRUE)
    data_art_df1 = TDAnalyticResult(data=arima_validate_op.result)
    uaf_out = ArimaForecast(data=data_art_df1, 
                            forecast_periods=2)
    # Print the result tbl.
    print(uaf_out.result)
    ArimaXEstimate
    ArimaXEstimate
    Functions
    ArimaXEstimate(data1=None, data1_ﬁlter_expr=None, data2=None, data2_ﬁlter_expr=None, nonseasonal_model_order=None, seasonal_model_order=None,
    seasonal_period=None, xreg=None, init=None, ﬁxed=None, constant=False, algorithm=None, max_iterations=100, coeff_stats=False, ﬁt_percentage=100,
    ﬁt_metrics=False, residuals=False, input_fmt_input_mode=None, output_fmt_index_style='NUMERICAL_SEQUENCE', **generic_arguments)
    DESCRIPTION:
        ArimaXEstimate() function extends the capability of ArimaEstimate() by
        allowing to include external regressors or covariates to an ARIMA model.
        The external regressors are specified in TDSeries payload specification
        after targeting the univariate series.
        The following procedure is an example of how to use:
            1. Run the ArimaXEstimate() function to estimate the coefficients
               of ARIMAX model.
            2. Run the ArimaXForecast() function with the estimated coefficient
               as first input, and the regular input time series table (TDSeries) that
               contains the future value of exogenous variables as second input.
    PARAMETERS:
        data1:
            Required Argument.
            Specifies the input series.
            Types: TDSeries
        data1_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data1".
            Types: ColumnExpression
        data2:
            Optional Argument.
            Specifies a logical univariate
            series and an art table from previous 
            ArimaXEstimate() call. This allows the user to fit
            the interested series in TDSeries by existing model
            in TDAnalyticResult. In this case, the function's primary
            result set will be based on the existing model's 
            coefficients.
            Types: TDSeries, TDAnalyticResult
        data2_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data2".
            Types: ColumnExpression
        nonseasonal_model_order:
            Required Argument.
            Specifies the non-seasonal values for the model.
            A list containing three integer values, which are each greater than or equal to 0:
                • p-value: The order of the non-seasonal autoregression
                (AR) component.
                • d-value: The order of the non-seasonal differences
                between consecutive components.
                • q-value: The order of the non-seasonal moving
                average (MA) component.
            Types: int, list of int
        seasonal_model_order:
            Required Argument.
            Specifies the seasonal values for the model.
            A list containing three integer values, which are each greater than or equal to 0:
                • P-value: The order of the seasonal auto-regression
                (SAR) component.
                • D-value: The order of the seasonal differences
                between consecutive components.
                • Q-value: The order of the seasonal moving average
                (SMA) component.
            Types: int, list of int
        seasonal_period:
            Optional Argument.
            Specifies the number of periods per season.
            Types: int
        xreg:
            Required Argument.
            Specifies the number of covariates in external regressors.
            Note:
                * If value is 0, then it suggests to use ArimaEstimate().
                  The input number should match with the number
                  of (payload-1). Otherwise, an error occurs with
                  the message “Unexpected XREG input.”
                * Maximum number for this argument is 10.
            Types: int
        init:
            Optional Argument.
            Specifies the position-sensitive list that specifies the initial
            values to be associated with the non-seasonal AR
            regression coefficients, followed by the non-seasonal
            MA coefficients, the seasonal SAR regression
            coefficients and the SMA coefficients. The formula is
            as follows: 'p+q+P+Q+CONSTANT-length-init-list'
            Types: int, list of int, float, list of float
        fixed:
            Optional Argument.
            Specifies the position-sensitive list that contains the
            fixed values to be associated with the non-seasonal
            AR regression coefficients, followed by the nonseasonal
            MA coefficients, the SAR coefficients and
            the SMA coefficients.
            If an intercept is needed, one more value is added at
            the end to specify the intercept coefficient initial value.
            The formula is as follows: 'p+q+P+Q+CONSTANT-length-fixed-list'
            Types: int, list of int, float, list of float
        constant:
            Optional Argument.
            Specifies the indicator for the ArimaXEstimate() function to
            calculate an intercept. When set to True, it indicates intercept
            should be calculated otherwise it indicates no
            intercept should be calculated.
            Default Value: False
            Types: bool
        algorithm:
            Required Argument.
            Specifies the method to estimate the coefficients.
            Permitted Values: OLE, MLE, MLE_CSS, CSS
            Types: str
        max_iterations:
            Optional Argument.
            Specifies the limit on the maximum number of
            iterations that can be employed to estimate the
            ARIMA parameters. Only relevant for "algorithm" value 'MLE'
            processing.
            Default Value: 100
            Types: int
        coeff_stats:
            Optional Argument.
            Specifies the flag indicating whether to return coefficient
            statistical columns STD_ERROR, TSTAT_VALUE and
            TSTAT_PROB. When set to True, function returns the columns,
            otherwise does not return the columns.
            Default Value: False
            Types: bool
        fit_percentage:
            Optional Argument.
            Specifies the percentage of passed-in sample points
            that are used for the model fitting and parameter estimation.
            Default Value: 100
            Types: int
        fit_metrics:
            Optional Argument.
            Specifies the indicator to generate the secondary result
            set that contains the model metadata statistics.
            When set to True, the function generates the secondary result set
            otherwise does not generate the secondary result set.
            The generated result set is retrieved by issuing the
            ExtractResults function on the analytical result
            table containing the results.
            Default Value: False
            Types: bool
        residuals:
            Optional Argument.
            Specifies the indicator to generate the tertiary result set
            that contains the model residuals. When set to True, function
            generates the tertiary result set otherwise, does
            not generate the tertiary result set.
            Default Value: False
            Types: bool
        input_fmt_input_mode:
            Required Argument.
            Specifies the input mode supported by the function.
            Permitted Values: MANY2ONE, ONE2ONE, MATCH
            Types: str
        output_fmt_index_style:
            Optional Argument.
            Specifies the "index_style" of the output format.
            Permitted Values: NUMERICAL_SEQUENCE, FLOW_THROUGH
            Default Value: NUMERICAL_SEQUENCE
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an 
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results. 
                    If not specified, a unique table name is internally 
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output 
                    table into. If not specified, table is created into 
                    database specified by the user at the time of context 
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of ArimaXEstimate.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as ArimaXEstimate_obj.<attribute_name>.
        Output teradataml DataFrame attribute names are:
            1. result
            2. fitmetadata
            3. fitresiduals
            4. model
            5. valdata
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage, before importing the
        #        function in user space.
        #     2. User can import the function, if it is available on
        #        Vantage user is connected to.
        #     3. To check the list of UAF analytic functions available
        #        on Vantage user connected to, use
        #        "display_analytic_functions()".
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Import function ArimaXEstimate.
        from teradataml import ArimaXEstimate
        # Load the example data.
        load_example_data("uaf", "blood2ageandweight")
        # Create teradataml DataFrame objects.
        data1 = DataFrame.from_table("blood2ageandweight")
        # Create teradataml TDSeries objects.
        data1_series_df = TDSeries(data=data1,
                                   id="PatientID",
                                   row_index="SeqNo",
                                   row_index_style="SEQUENCE",
                                   payload_field=["BloodFat", "Age"],
                                   payload_content="MULTIVAR_REAL")
        # Example 1: Execute ArimaXEstimate with single input.
        uaf_out = ArimaXEstimate(data1=data1_series_df,
                                 nonseasonal_model_order=[2,0,1],
                                 xreg=True,
                                 fit_metrics=True,
                                 residuals=True,
                                 constant=True
                                 algorithm=MLE,
                                 fit_percentage=80
                                 )
        # Print the result DataFrames.
        print(uaf_out.result)
        print(uaf_out.fitmetadata)
        print(uaf_out.fitresiduals)
        print(uaf_out.model)
        print(uaf_out.valdata)
    DTW
    DTW
    index
    Functions
    DTW(data1=None, data1_ﬁlter_expr=None, data2=None, data2_ﬁlter_expr=None, radius=None, distance=None, warp_path=None, input_fmt_input_mode=None,
    **generic_arguments)
    DESCRIPTION:
        The DTW() function measures the similarity of two time series.
        The Dynamics Time Warping (DTW) algorithm is used for space and
        time. The underlying algorithm used by DTW() function is the FastDTW
        algorithm. It is not recommended for large datasets. This algorithm
        can find the optimal, or a close to optimal warp path between two
        series, depending on the search radius used.
    PARAMETERS:
        data1:
            Required Argument.
            Specifies the first series input out of two.
            Note:
                The input series specified in "data1" and "data2" must
                have the same number of payload columns.
            Types: TDSeries
        data1_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data1".
            Types: ColumnExpression
        data2:
            Required Argument.
            Specifies the second series input out of two.
            Note:
                The input series specified in "data1" and "data2" must
                have the same number of payload columns.
            Types: TDSeries
        data2_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data2".
            Types: ColumnExpression
        radius:
            Optional Argument.
            Specifies the search radius for the FastDTW algorithm.
            This value should be a positive integer within the range
            0 to 1000. Using a small radius is sufficient for finding
            the optimal match. Larger value of radius can cause significant
            performance issues without providing a better result.
            Types: int
        distance:
            Optional Argument.
            Specifies the distance function to be used.
            Permitted Values:
                * EUCLIDEAN - Euclidean distance function.
                * MANHATTAN - Manhattan distance function.
                * BINARY - Binary distance function.
            Types: str
        warp_path:
            Optional Argument.
            Specifies the type of warp paths.
            Permitted Values:
                * 0 - No warp paths to be generated.
                      Only the warp distance is calculated.
                * 1 - Warp paths to be generated with WarpX and
                      WarpY output columns as the path index.
                * 2 - Warp paths to be generated with WarpX_I and
                      WarpY_I using the series 1 and series 2
                      ROW_I values at the path index.
                * 3 - Warp paths to be generated with both the
                      output columns 1 and 2.
            Types: int
        input_fmt_input_mode:
            Required Argument.
            Specifies the input mode supported by the function.
            Permitted Values: MANY2ONE, ONE2ONE, MATCH
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an 
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results. 
                    If not specified, a unique table name is internally 
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output 
                    table into. If not specified, table is created into 
                    database specified by the user at the time of context 
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of DTW.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as DTW_obj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Load the example data.
        load_example_data("uaf", ["dtw_t1", "dtw_t2"])
        # Create teradataml DataFrame objects.
        data1 = DataFrame.from_table("dtw_t1")
        data2 = DataFrame.from_table("dtw_t2")
        # Create teradataml TDSeries objects.
        data_series_df = TDSeries(data=data1,
                                  id="id",
                                  row_index="seq",
                                  row_index_style= "TIMECODE",
                                  payload_field="v",
                                  payload_content="REAL")
        data2_series_df = TDSeries(data=data2,
                                  id="id",
                                  row_index="seq",
                                  row_index_style= "TIMECODE",
                                  payload_field="v",
                                  payload_content="REAL")
        # Example 1 : Execute DTW() function to measure the
        #             similarity between two time series.
        uaf_out = DTW(data1=data_series_df,
                      data2=data2_series_df,
                      input_fmt_input_mode='MANY2ONE',
                      warp_path=2,
                      radius=1,
                      data2_filter_expr=data2_series_df.id==1)
        # Print the result DataFrame.
        print(uaf_out.result)
    HoltWintersForecaster
    HoltWintersForecaster
    Functions
    HoltWintersForecaster(data=None, data_ﬁlter_expr=None, forecast_periods=None, alpha=None, beta=None, gamma=None, seasonal_periods=None,
    init_level=None, init_trend=None, init_season=None, model=None, ﬁt_percentage=100, prediction_intervals='BOTH', ﬁt_metrics=False, selection_metrics=False,
    residuals=False, output_fmt_index_style='NUMERICAL_SEQUENCE', **generic_arguments)
    DESCRIPTION:
        The HoltWintersForecaster() function uses triple exponential smoothing
        on a forecast model with seasonal data.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the time series to forecast using historical data
            with series content type as 'REAL' or 'MULTIVAR_REAL'.
            Types: TDSeries
        data_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data".
            Types: ColumnExpression
        forecast_periods:
            Required Argument.
            Specifies the number of periods to forecast.
            Value must be greater than or equal to 1.
            Types: int
        alpha:
            Optional Argument.
            Specifies a value to control the smoothing relative to
            the level component of the forecasting equation. If
            specified, this value is used in the equation to perform
            the forecasting, else the "alpha" value is estimated using
            goodness-of-fit metrics. Value must be greater than or equal
            to 0 and less than or equal to 1.
            Types: int OR float 
        beta:
            Optional Argument.
            Specifies a value to control the smoothing relative to
            the trend component of the forecasting equation. If
            specified, this value is used in the equation to perform
            the forecasting, else the "beta" value is estimated using
            goodness-of-fit metrics. Value must be greater than or equal
            to 0 and less than or equal to 1.
            Types: int OR float
        gamma:
            Optional Argument.
            Specifies a value to control the smoothing relative to
            the seasonal component of the forecasting equation. If
            specified, this value is used in the equation to perform
            the forecasting, else the "gamma" value is estimated using
            goodness-of-fit metrics. Value must be greater than or equal
            to 0 and less than or equal to 1.
            Types: int OR float
        seasonal_periods:
            Optional Argument.
            Specifies the number of periods or sample points for one season.
            For example, for yearly data with monthly sample points, the parameter
            is 12; and for quarterly data with monthly sample points, the
            parameter is 3. Value must be greater than or equal to 1.
            Note:
                Required when "gamma" or "init_season" is specified.
            Types: int
        init_level:
            Optional Argument.
            Specifies the initialization value used as part of the fitting
            and forecasting operations. If not specified, then the initialization
            value is calculated as an additive level.
            Types: int OR float
        init_trend:
            Optional Argument.
            Specifies the initialization value used as part of the fitting
            and forecasting operations. If not specified, then the initialization
            value is calculated as an additive trend.
            Types: int OR float
        init_season:
            Optional Argument.
            Specifies a list of initialization values, one per period. If
            specified, the initialization value is used as part of the
            fitting and forecasting operations, else the initialization
            value is calculated as a multiplicative seasonality.
            Types: int, list of int, float, list of float
        model:
            Required Argument.
            Specifies the type of Holt Winters forecasting.
            Permitted Values:
                * ADDITIVE: It is based on Holt Winters Additive approach.
                * MULTIPLICATIVE: It is based on Holt Winters Multiplicative approach.
            Types: str
        fit_percentage:
            Optional Argument.
            Specifies percentage of passed-in sample points to use for the
            model fitting or parameter estimation. Value must be greater
            than or equal to 0 and less than or equal to 100.
            Default Value: 100
            Types: int
        prediction_intervals:
            Optional Argument.
            Specifies the confidence level for the prediction.
            Permitted Values:
                * NONE
                * 80
                * 95
                * BOTH
            Default Value: BOTH
            Types: str
        fit_metrics:
            Optional Argument.
            Specifies whether to generate the result set that contains the
            model metadata statistics. When set to True, function generates
            the model statistics, otherwise not. The generated model
            statistics can be retrieved using the attribute "fitmetadata"
            of the function output.
            Default Value: False
            Types: bool
        selection_metrics:
            Optional Argument.
            Specifies whether to generate the result set that contains the
            selection metrics. When set to True, function generates the
            selection metrics, otherwise not. The generated selection metrics
            can be retrieved using the attribute "selmetrics" of the function
            output.
            Default Value: False
            Types: bool
        residuals:
            Optional Argument.
            Specifies whether to generate the result set that contains the
            model residuals. When set to True, the function generates the
            residuals, otherwise not. The generated residuals can be retrieved
            using the attribute "fitresiduals" of the function output.
            Default Value: False
            Types: bool
        output_fmt_index_style:
            Optional Argument.
            Specifies the index style of the output format.
            Permitted Values:
                * NUMERICAL_SEQUENCE
                * FLOW_THROUGH
            Default Value: NUMERICAL_SEQUENCE
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an 
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results. 
                    If not specified, a unique table name is internally 
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output 
                    table into. If not specified, table is created into 
                    database specified by the user at the time of context 
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of HoltWintersForecaster.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as HoltWintersForecaster_obj.<attribute_name>.
        Output teradataml DataFrame attribute names are:
            1. result
            2. fitmetadata - Available when "fit_metrics" is set to True, otherwise not.
            3. selmetrics - Available when "selection_metrics" is set to True, otherwise not.
            4. fitresiduals - Available when "residuals" is set to True, otherwise not.
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Load the example data.
        load_example_data("uaf", ["us_air_pass"])
        # Create teradataml DataFrame object.
        data = DataFrame.from_table("us_air_pass")
        # Create teradataml TDSeries object.
        data_series_df = TDSeries(data=data,
                                  row_index="idx",
                                  row_index_style="SEQUENCE",
                                  id="id",
                                  payload_field="international",
                                  payload_content="REAL")
        # Example 1: Generate forecast for 12 periods using multiplicative model.
        uaf_out = HoltWintersForecaster(data=data_series_df,
                                        forecast_periods=12,
                                        model="MULTIPLICATIVE",
                                        residuals=True,
                                        fit_metrics=True,
                                        selection_metrics=True)
        # Print the result DataFrames.
        print(uaf_out.result)
        # Print the model statistics result.
        print(uaf_out.fitmetadata)
        # Print the selection metrics result.
        print(uaf_out.selmetrics)
        # Print the residuals statistics result.
        print(uaf_out.fitresiduals)
    MAMean
    MAMean
    Functions
    MAMean(data=None, data_ﬁlter_expr=None, forecast_periods=None, algorithm=None, prediction_intervals='BOTH', k_order=None, ﬁt_metrics=False,
    residuals=False, output_fmt_index_style='NUMERICAL_SEQUENCE', **generic_arguments)
    DESCRIPTION:
        The MAMean() function forecasts a user-defined number of periods into the future,
        that is the number of periods beyond the last observed sample point in the series.
    PARAMETERS:
        data:
            Required Argument.
            Specifies a logical univariate series with historical data.
            Types: TDSeries
        data_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data".
            Types: ColumnExpression
        forecast_periods:
            Required Argument.
            Specifies the number of periods to forecast ahead.
            The argument "forecast_periods" must be a positive integer value in the range [1, 32000].
            Types: int
        algorithm:
            Required Argument.
            Specifies the type of algorithm to use.
            Permitted Values:
                * MA: Moving Average
                * MEAN: Mean average
                * NAIVE: Naive, also known as random walk forecast
            Types: str
        prediction_intervals:
            Optional Argument.
            Specifies the confidence level for the prediction, such that 85 means 85% confidence.
            Permitted Values: NONE, 80, 95, BOTH
            Default Value: BOTH
            Types: str
        k_order:
            Optional Argument.
            Specifies the moving average forecast.
            The argument "k_order" must be a positive int value in the range [1, 32000].
            Note:
                * This is a required argument when "alogorithm" is set to 'MA'.
            Types: int
        fit_metrics:
            Optional Argument.
            Specifies a flag to generate the secondary result set that contains the model metadata
            statistics. When set to True, function generate the secondary result set, otherwise
            not. The generated result set can be retrieved using the attribute fitmetadata of
            the function output.
            Default Value: False
            Types: bool
        residuals:
            Optional Argument.
            Specifies a flag to generate the tertiary result set that contains the model residuals.
            When set to True, means generate the tertiary result set, otherwise not.
            The generated result set can be retrieved using the attribute fitresiduals of
            the function output.
            Default Value: False
            Types: bool
        output_fmt_index_style:
            Optional Argument.
            Specifies the index style of the output format.
            Default Value: NUMERICAL_SEQUENCE
            Permitted Values: NUMERICAL_SEQUENCE
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an 
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results. 
                    If not specified, a unique table name is internally 
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output 
                    table into. If not specified, table is created into 
                    database specified by the user at the time of context 
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of MAMean.
        Output teradataml DataFrames can be accessed using attribute
        references, such as MAMean_obj.<attribute_name>.
        Output teradataml DataFrame attribute names are:
            1. result
            2. fitmetadata - Available when "model_stats" is set to True, otherwise not.
            3. fitresiduals - Available when "residuals" is set to True, otherwise not.
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Load the example data.
        load_example_data("uaf", ["orders1"])
        # Create teradataml DataFrame object.
        df = DataFrame.from_table("orders1")
        # Create teradataml TDSeries object.
        result = TDSeries(data=df,
                          id="store_id",
                          row_index="seq",
                          row_index_style="SEQUENCE",
                          payload_field="sales",
                          payload_content="REAL")
        # Example 1: Forecast 8 number of periods into the future beyond the last observed sample
        #            point in the series using the moving average algorithm and generate metadata as
        #            well as residuals data.
        uaf_out = MAMean(data=result,
                         forecast_periods=8,
                         algorithm='MA',
                         prediction_intervals='BOTH',
                         k_order=3,
                         fit_metrics=True,
                         residuals=True)
        # Print the result DataFrames.
        print(uaf_out.result)
        print(uaf_out.fitresiduals)
        print(uaf_out.fitmetadata)
    SimpleExp
    SimpleExp
    Functions
    SimpleExp(data=None, data_ﬁlter_expr=None, forecast_periods=None, alpha=None, prediction_intervals='BOTH', forecast_starting_value='FIRST',
    ﬁt_metrics=False, residuals=False, output_fmt_index_style='NUMERICAL_SEQUENCE', **generic_arguments)
    DESCRIPTION:
        The SimpleExp() function uses simple exponential smoothing for
        the forecast model for univariate data. It does not use seasonality
        or trends for the model.
    PARAMETERS:
        data:
            Required Argument.
            Specifies a logical univariate series with historical data.
            Input value should be 'REAL'.
            Types: TDSeries
        data_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data".
            Types: ColumnExpression
        forecast_periods:
            Required Argument.
            Specifies number of periods to forecast.
            This value must be a positive integer in the range of [1, 32000].
            Types: int
        alpha:
            Optional Argument.
            Specifies if this argument is specified, its value is used
            in the equation to perform the forecasting. If this argument
            is not specified, the value of 'alpha' is estimated by using
            goodness-of-fit metrics. Value must be greater than or equal to
            0 and less than or equal to 1.
            Types: int OR float
        prediction_intervals:
            Optional Argument.
            Specifies the confidence level for the prediction. For example,
            85 means 85% confidence.
            Permitted Values: NONE, 80, 95, BOTH
            Default Value: BOTH
            Types: str
        forecast_starting_value:
            Optional Argument.
            Specifies the starting value for the interval.
            Permitted Values: FIRST, MEAN
            Default Value: FIRST
            Types: str
        fit_metrics:
            Optional Argument.
            Specifies whether to generate the result set that
            contains the model metadata statistics. When set to True,
            function generates the model statistics, otherwise not.
            The generated model statistics can be retrieved using the
            attribute "fitmetadata" of the function output.
            Default Value: False
            Types: bool
        residuals:
            Optional Argument.
            Specifies whether to generate the result set that
            contains the model residuals. When set to True, the function
            generates the residuals, otherwise not. The generated residuals
            can be retrieved using the attribute "fitresiduals" of
            the function output.
            Default Value: False
            Types: bool
        output_fmt_index_style:
            Optional Argument.
            Specifies the index style of the output format.
            Permitted Values: NUMERICAL_SEQUENCE, FLOW_THROUGH
            Default Value: NUMERICAL_SEQUENCE
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an 
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results. 
                    If not specified, a unique table name is internally 
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output 
                    table into. If not specified, table is created into 
                    database specified by the user at the time of context 
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of SimpleExp.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as SimpleExp_obj.<attribute_name>.
        Output teradataml DataFrame attribute names are:
            1. result
            2. fitmetadata - Available when "fit_metrics" is set to True, otherwise not.
            3. fitresiduals - Available when "residuals" is set to True, otherwise not.
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Load the example data.
        load_example_data("uaf","inflation")
        # Create teradataml DataFrame object.
        df=DataFrame.from_table("inflation")
        # Create teradataml TDSeries object.
        data_series_df = TDSeries(data=df,
                                  id="countryid",
                                  row_index="year_recorded",
                                  row_index_style= "TIMECODE",
                                  payload_field="inflation_rate",
                                  payload_content="REAL")
        # Example 1 : Execute SimpleExp() function which uses simple exponential
        #             smoothing for the forecast model for univariate data to
        #             produce historical observed values, forecasted value,
        #             predicted values, residuals and metrics containing goodness
        #             of fit.
        uaf_out = SimpleExp(data=data_series_df,
                            forecast_periods=4,
                            prediction_intervals="80",
                            fit_metrics=True,
                            residuals=True)
        # Print the result DataFrames.
        print(uaf_out.result)
        print(uaf_out.fitresiduals)
        print(uaf_out.fitmetadata)
    DATA PREPARATIONS functions
    BinaryMatrixOp
    BinaryMatrixOp
    Functions
    BinaryMatrixOp(data1=None, data1_ﬁlter_expr=None, data2=None, data2_ﬁlter_expr=None, math_op=None, input_fmt_input_mode=None,
    output_fmt_index_style='NUMERICAL_SEQUENCE', **generic_arguments)
    DESCRIPTION:
        The BinaryMatrixOp() function performs a point-wise mathematical
        operation on two matrices with an equal number of wavelets and for
        which corresponding wavelets have an equal number of data points.
        A point-wise operation signifies that a mathematical operation is
        performed on one sample point at a time. Allowed mathematical
        operations are addition, subtraction, multiplication and division.
        The BinaryMatrixOp() function takes two equally sized logical-runtime
        matrices as input - the first matrix referenced in the function call
        ("data1") is a primary matrix, whereas the second matrix referenced
        ("data2") is a secondary matrix. The significance of being a primary
        or secondary matrix in each of the allowed mathematical operation is as
        follows:
            * SUB: The secondary matrix is subtracted from the primary matrix.
            * ADD: The secondary matrix is added to the primary matrix.
            * MUL: The primary matrix is multiplied by the secondary matrix.
            * DIV: The primary matrix is divided by the secondary matrix.
        The result matrix produced by the BinaryMatrixOp() function is of 
        the same size, 'N * M', as of its inputs and inherits the TDMatrix
        identifier (id), from the primary matrix.
        The BinaryMatrixOp() function can be configured to operate in one
        of three input modes - ONE2ONE, MANY2ONE, and MATCH. These modes
        determine the number of primary matrices and number of secondary
        matrices involved in the operation, as well as determining how the
        primary and secondary matrix will be matched together.
        The common uses of BinaryMatrixOp() function are:
            * Addition:       Restore trends to a time matrix before using the
                              model for forecasting.
            * Subtraction:    Subtract trends from a time matrix to create a
                              model from it.
            * Multiplication: Apply a low pass, band pass, or high pass
                              filter to a time matrix.
            * Division:       Divide the primary matrix by the secondary matrix.
    PARAMETERS:
        data1:
            Required Argument.
            Specifies the primary matrix in the mathematical operation.
            Note:
                The matrices specified in "data1" and "data2" must have
                the same dimensions.
            Types: TDMatrix
        data1_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data1".
            Types: ColumnExpression
        data2:
            Required Argument.
            Specifies the secondary matrix in the mathematical operation.
            Note:
                The matrices specified in "data1" and "data2" must have
                the same dimensions.
            Types: TDMatrix
        data2_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data2".
            Types: ColumnExpression
        math_op:
            Required Argument.
            Specifies the mathematical operation to be performed
            between "data1" and "data2".
            Permitted Values:
                * SUB: The secondary matrix is subtracted from the primary matrix.
                       This subtracts trends from a time matrix to create a model
                       from it.
                * ADD: The secondary matrix is added to the primary matrix.
                       This restores trends to a time matrix before using the model
                       for forecasting.
                * MUL: The primary matrix is multiplied by the secondary matrix.
                       This applies a low pass, band pass, or high pass filter to
                       a time matrix.
                * DIV: The primary matrix is divided by the secondary matrix.
                       This divides the primary matrix by the secondary matrix.
            Types: str
        input_fmt_input_mode:
            Required Argument.
            Specifies the input mode supported by the function.
            Permitted Values:
                * ONE2ONE: Both the primary and secondary matrix
                           specifications contain a matrix name
                           which identifies each of the two matrices
                           in the function.
                * MANY2ONE: The MANY specification is the primary matrix
                            declaration. The secondary matrix specification
                            contains a matrix name that identifies the
                            single secondary matrix.
                * MATCH: Both matrices are defined by their respective matrix id.
            Types: str
        output_fmt_index_style:
            Optional Argument.
            Specifies the index style of the output format.
            Default Value: NUMERICAL_SEQUENCE
            Permitted Values: NUMERICAL_SEQUENCE, FLOW_THROUGH
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an 
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results. 
                    If not specified, a unique table name is internally 
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output 
                    table into. If not specified, table is created into 
                    database specified by the user at the time of context 
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of BinaryMatrixOp.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as BinaryMatrixOp_obj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Load the example data.
        load_example_data("uaf", ["binary_matrix_complex_left", "binary_matrix_complex_right"])
        load_example_data("uaf", ["binary_matrix_real_left", "binary_matrix_real_right"])
        # Create teradataml DataFrame objects.
        data1 = DataFrame.from_table("binary_matrix_complex_left")
        data2 = DataFrame.from_table("binary_matrix_complex_right")
        data3 = DataFrame.from_table("binary_matrix_real_left")
        data4 = DataFrame.from_table("binary_matrix_real_right")
        # Create teradataml TDMatrix objects.
        binary_matrix_complex_left = TDMatrix(data=data1,
                                              id="id",
                                              row_index="seq",
                                              column_index="tick",
                                              row_index_style="SEQUENCE",
                                              column_index_style="SEQUENCE",
                                              payload_field=["real_val", "imaginary_val"],
                                              payload_content="COMPLEX")
        binary_matrix_complex_right = TDMatrix(data=data2,
                                               id="id",
                                               row_index="seq",
                                               column_index="tick",
                                               row_index_style="SEQUENCE",
                                               column_index_style="SEQUENCE",
                                               payload_field=["real_val", "imaginary_val"],
                                               payload_content="COMPLEX")
        # Example 1 : Perform addition operation in MANY2ONE mode between
        #             two matrices holding COMPLEX payload.
        uaf_out_1 = BinaryMatrixOp(data1=binary_matrix_complex_left,
                                   data2=binary_matrix_complex_right,
                                   data2_filter_expr=binary_matrix_complex_right.id==1,
                                   math_op="ADD",
                                   input_fmt_input_mode="MANY2ONE")
        # Print the result DataFrame.
        print(uaf_out_1.result)
        # Example 2 : Perform multiplication operation in MATCH mode between
        #             two matrices holding REAL payload.
        # Create teradataml TDMatrix objects.
        binary_matrix_real_left = TDMatrix(data=data3,
                                           id="id",
                                           row_index="seq",
                                           column_index="tick",
                                           row_index_style="SEQUENCE",
                                           column_index_style="SEQUENCE",
                                           payload_field="a",
                                           payload_content="REAL")
        binary_matrix_real_right = TDMatrix(data=data4,
                                            id="id",
                                            row_index="seq",
                                            column_index="tick",
                                            row_index_style="SEQUENCE",
                                            column_index_style="SEQUENCE",
                                            payload_field="b",
                                            payload_content="REAL")
        uaf_out_2 = BinaryMatrixOp(data1=binary_matrix_real_left,
                                   data2=binary_matrix_real_right,
                                   math_op="MUL",
                                   input_fmt_input_mode="MATCH")
        # Print the result DataFrame.
        print(uaf_out_2.result)
    BinarySeriesOp
    BinarySeriesOp
    Functions
    BinarySeriesOp(data1=None, data1_ﬁlter_expr=None, data2=None, data2_ﬁlter_expr=None, math_op=None, input_fmt_input_mode=None,
    output_fmt_index_style='NUMERICAL_SEQUENCE', **generic_arguments)
    DESCRIPTION:
        The BinarySeriesOp() function performs a point wise mathematical
        operation on two time series of equal size.
        The principal mathematical operation can be
        subtraction, addition, multiplication and division.
        It is called a point wise operation because it performs
        the mathematical operation one sample point at a time.
        For example, if the mathematical operation being performed is
        subtraction, then for two series each with N sample points,
        a new series is generated in which:
            * ResultSeries1 =  MinuendSeries1 - SubtrahendSeries1
            * ResultSeries2 =  MinuendSeries2 - SubtrahendSeries2
            * ResultSeries3 =  MinuendSeries3 -SubtrahendSeries3
            ... up to ...
            * ResultSeriesN  = MinuendSeriesN  - SubtrahendSeriesN.
        The result series/1D array produced by the BinarySeriesOp()
        is of the same size 'N' as its inputs.
        BinarySeriesOp() takes two equally sized logical-runtime series
        as input:
            * The first series referenced in the function call as "data1"
              is referred as the primary series.
            * The second series referenced in the function call as "data2"
              is referred as the secondary series.
        The result series always inherits the identifiers,
        series_id from the primary series.
        The BinarySeriesOp() function can be configured to
        operate in one of three input modes - ONE2ONE, MANY2ONE,
        and MATCH. These modes determine the number of primary
        series and number of secondary series involved in the
        operation, as well as determining how the primary and
        secondary series will be matched together.
        Common uses of BinarySeriesOp() are:
            1. Subtracting trends from a time series to create a model from it.
            2. Restoring trends to a time series before using the model for
               forecasting.
            3. As a building block to formulate more complex
               functions.
        For example, convolving in the time domain is point-wise multiplication
        in the frequency domain.
        The following procedure is an example of how
        to use BinarySeriesOp() to convolve two series with
        digital signal processing:
            1. Use DFFT() function on series 1 and series 2 to get dataframes
               named 'dfftRes1' and 'dfftRes2', respectively.
            2. Use BinarySeriesOp() to do point-wise multiplication
               using 'dfftRes1' and 'dfftRes2'.
            3. Use IDFFT() on the output of BinarySeriesOp() to get the
               convolved result of the two series.
    PARAMETERS:
        data1:
            Required Argument.
            Specifies the primary series in the mathematical operation.
            The first input is measured against the second input.
            This series must have the same size as that of the
            second input series specified in "data2".
            Input values are REAL and MUTLIVAR_REAL.
            Function supports one input being univariate and the other being
            multivariate. In particular, the following combinations are
            supported:
                1. For all MULTIVAR varieties: Both multivariate series
                   are of the same content type and both series have
                   the same number of payload fields.
                2. For MULTIVAR_REAL: One input is a MULTIVAR content series
                   having greater than one payload field and the other series
                   is a REAL content series having just one payload field.
                3. For the MULTIVAR_AMPL_PHASE and MULTIVAR_COMPLEX varieties:
                   One input is a MULTIVAR content series having greater than one
                   pair of fields and the other series is a MULTIVAR content of
                   the same type having one pair of payload fields.
            Types: TDSeries
        data1_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data1".
            Types: ColumnExpression
        data2:
            Required Argument.
            Specifies the secondary series of equal size to be operated on.
            This series must have the same size as that of the
            first input series specified in "data1".
            Input values are REAL and MUTLIVAR_REAL.
            Function supports one input being univariate and the other being
            multivariate. In particular, the following combinations are
            supported:
                1. For all MULTIVAR varieties: Both multivariate series
                   are of the same content type and both series have
                   the same number of payload fields.
                2. For MULTIVAR_REAL: One input is a MULTIVAR content series
                   having greater than one payload field and the other series
                   is a REAL content series having just one payload field.
                3. For the MULTIVAR_AMPL_PHASE and MULTIVAR_COMPLEX varieties:
                   One input is a MULTIVAR content series having greater than one
                   pair of fields and the other series is a MULTIVAR content of
                   the same type having one pair of payload fields.
            Types: TDSeries
        data2_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data2".
            Types: ColumnExpression
        math_op:
            Required Argument.
            Specifies the mathematical operation to be performed
            between the series passed in "data1" and "data2".
            Permitted Values:
                SUB: Subtract trends from a time series to create a model from it.
                     The series in "data2" is subtracted from the series
                     in "data1".
                ADD: Restore trends to a time series before using the model for forecasting.
                MUL: Apply a low pass, band pass, or high pass filter to a time series.
                DIV: The series in "data1" is divided by the series
                     in "data2".
            Types: str
        input_fmt_input_mode:
            Required Argument.
            Specifies the input mode supported by the function.
            Permitted Values:
                * ONE2ONE: Both the primary and secondary series
                           specifications contain a series name
                           which identifies the two series in the function.
                * MANY2ONE: The MANY specification is the primary series declaration.
                            The secondary series specification contains a series name
                            that identifies the single secondary matrix.
                * MATCH: Both series are defined by their respective
                         series id declarations.
            Types: str
        output_fmt_index_style:
            Optional Argument.
            Specifies the index style of the output format.
            Permitted Values: NUMERICAL_SEQUENCE, FLOW_THROUGH
            Default Value: NUMERICAL_SEQUENCE
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results.
                    If not specified, a unique table name is internally
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output
                    table into. If not specified, table is created into
                    database specified by the user at the time of context
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of BinarySeriesOp.
        Output teradataml DataFrames can be accessed using attribute
        references, such as BinarySeriesOp_obj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Load the example data.
        load_example_data("uaf", ["binary_complex_left", "binary_complex_right"])
        # Create teradataml DataFrame objects.
        data1 = DataFrame.from_table("binary_complex_left")
        data2 = DataFrame.from_table("binary_complex_right")
        # Create teradataml TDSeries objects.
        data1_series_df = TDSeries(data=data1,
                                   id="id",
                                   row_index="seq",
                                   row_index_style="SEQUENCE",
                                   payload_field=["real_val", "imaginary_val"],
                                   payload_content="COMPLEX")
        data2_series_df = TDSeries(data=data2,
                                   id="id",
                                   row_index="seq",
                                   row_index_style= "SEQUENCE",
                                   payload_field=["real_val", "imaginary_val"],
                                   payload_content="COMPLEX")
        # Form the filter expressions to filter the series with id=1.
        data1_filter_expr=data1_series_df.id==1
        data2_filter_expr=data2_series_df.id==1
        # Example 1: Perform addition of two time series of equal size.
        uaf_out = BinarySeriesOp(data1=data1_series_df,
                                 data1_filter_expr=data1_filter_expr,
                                 data2=data2_series_df,
                                 data2_filter_expr=data2_filter_expr,
                                 math_op="ADD",
                                 input_fmt_input_mode="MANY2ONE")
        # Print the result DataFrame.
        print(uaf_out.result)
    GenseriesFormula
    GenseriesFormula
    Functions
    GenseriesFormula(data=None, data_ﬁlter_expr=None, formula=None, estimate_mode=False, output_fmt_index_style='NUMERICAL_SEQUENCE',
    **generic_arguments)
    DESCRIPTION:
        The GenseriesFormula() function allows you to define and apply
        a formula to generate a time series. This function has many use
        cases, including the ability to generate a series that is 
        subtracted from a non-stationary series to make it stationary,
        and to estimate beyond the sample data points.
        The following procedure is an example of how to use the
        GenseriesFormula() function:
            * Determine that the series to be modeled includes
              a trend.
            * Use LinearRegr() function to remove the trend from 
              the series.
            * Use the "fitmetadata" attribute from the function output,
              to determine the trend by fitting the data set.
            * Use GenseriesFormula() function to generate a
              trend series.
            * Use BinarySeriesOp() function to subtract the
              generated trend from the original series.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input time series or self-generating
            time series.
            Notes:
                * Series specification is a time series or 
                  spatial series. Series can be REAL or MULTIVAR_REAL.
                * Generated series specification uses a self-generated series.
            Types: TDSeries, TDGenseries
        data_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data".
            Types: ColumnExpression
        formula:
            Required Argument.
            Specifies the formula to apply to the input time series.
            The formula represents the trend or periodicity in the input time 
            series and conforms to formula rules.
            If you specify generated series, TDGenSeries, the formula
            can have only one explanatory variable. The function assigns
            the starting value to the explanatory variable.
            Note:
                Use the following link to refer the formula rules in
                Teradata document:
                "https://docs.teradata.com/r/Teradata-VantageTM-Unbounded-Array-Framework-Time-Series-
    Reference/Mathematic-Operators-and-Functions/Formula-Rules"
            Types: str
        estimate_mode:
            Optional Argument.
            Specifies whether to include the input parameters in the results.
            When set to True, function includes input parameters in the results,
            otherwise does not include.
            Default Value: False
            Types: bool
        output_fmt_index_style:
            Optional Argument.
            Specifies the index style of the output format.            
            Permitted Values: NUMERICAL_SEQUENCE, FLOW_THROUGH
            Default Value: NUMERICAL_SEQUENCE
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an 
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results. 
                    If not specified, a unique table name is internally 
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output 
                    table into. If not specified, table is created into 
                    database specified by the user at the time of context 
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of GenseriesFormula.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as GenseriesFormula_obj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        from teradataml import FLOAT, INTEGER
        # Load the example data.
        load_example_data("uaf", ["production_data2"])
        # Create teradataml DataFrame object.
        data = DataFrame.from_table("production_data2")
        # Example 1: Execute the GenseriesFormula() function on TDSeries input 
        #            for index style 'FLOW_THROUGH' to generate a time series.
        #            New time series with the same type as the series
        #            specification payload content value.
        # Create teradataml TDSeries object.
        data_series_df_1 = TDSeries(data=data,
                                    id="product_id",
                                    row_index="TD_TIMECODE",
                                    payload_field="beer_sales",
                                    payload_content="REAL")
        # Execute GenseriesFormula for TDSeries.
        uaf_out_1 =  GenseriesFormula(data=data_series_df_1,
                                      formula="Y = 2.0*X1 + SIN(X1)",
                                      output_fmt_index_style='FLOW_THROUGH')
        # Print the result DataFrame.
        print(uaf_out_1.result)
        # Example 2: Execute the GenseriesFormula() function on TDGenSeries input 
        #            for index style value 'NUMERICAL_SEQUENCE' and TDGenSeries
        #            data types as FLOAT to generate a time series.
        #            Functions returns new time series with the same type as 
        #            the generated series specification payload content value.
        #            TDGenSeries datatypes is float.
        # Create teradataml TDGenSeries object.
        data_series_df_2 = TDGenSeries(instances = {"BuoyID": 3},
                                       data_types = FLOAT(),
                                       start=0.0,
                                       offset=1.0,
                                       num_entries=5)
        # Execute GenseriesFormula for TDGenSeries.
        uaf_out_2 =  GenseriesFormula(data=data_series_df_2,
                                      formula="Y = 3.0 + 8.0*X1",
                                      output_fmt_index_style='NUMERICAL_SEQUENCE')
        # Print the result DataFrame.
        print(uaf_out_2.result)
        # Example 3: Execute the GenseriesFormula() function on TDGenSeries input 
        #            for index style value 'NUMERICAL_SEQUENCE' and TDGenSeries
        #            data types as INTEGER to generate a time series.
        #            Functions returns new time series with the same type as 
        #            the generated series specification payload content value.
        #            TDGenSeries datatypes is integer.
        # Create teradataml TDGenSeries object.
        data_series_df_3 = TDGenSeries(instances = {"BuoyID": 3},
                                       data_types = INTEGER(),
                                       start=0,
                                       offset=1,
                                       num_entries=5)
        # Execute GenseriesFormula for TDGenSeries.
        uaf_out_3 =  GenseriesFormula(data=data_series_df_3,
                                      formula="Y = 3.0 + 8.0*X1",
                                      output_fmt_index_style='NUMERICAL_SEQUENCE')
        # Print the result DataFrame.
        print(uaf_out_3.result)
    MatrixMultiply
    MatrixMultiply
    Functions
    MatrixMultiply(data1=None, data1_ﬁlter_expr=None, data2=None, data2_ﬁlter_expr=None, input_fmt_input_mode=None, **generic_arguments)
    DESCRIPTION:
        The MatrixMultiply() function enables users to create a data series based on two matrixes.
        The source matrixes must have the same number of data points and the same
        number of wavelets.
    PARAMETERS:
        data1:
            Required Argument.
            Specifies the primary matrix.
            Types: TDMatrix
        data1_filter_expr:
            Optional Argument.
            Specifies filter expression for "data1".
            Types: ColumnExpression
        data2:
            Required Argument.
            Specifies secondary matrix of size equal to the size of "data1" matrix to be operated on.
            Types: TDMatrix
        data2_filter_expr:
            Optional Argument.
            Specifies filter expression for "data2".
            Types: ColumnExpression
        input_fmt_input_mode:
            Required Argument.
            Specifies the input mode supported by the function.
            Permitted Values:
                1. ONE2ONE: Both the primary and secondary matrix specifications contain a matrix
                            name which identifies the two matrixes in the function.
                2. MANY2ONE: The MANY specification is the primary matrix declaration. The
                             secondary matrix specification contains a matrix name that identifies the
                             single secondary matrix.
                3. MATCH: Both matrixes are defined by their respective matrix id.
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
             Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an 
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results. 
                    If not specified, a unique table name is internally 
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output 
                    table into. If not specified, table is created into 
                    database specified by the user at the time of context 
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of MatrixMultiply.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as MatrixMultiply_obj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Load the example data.
        load_example_data("uaf", ["mtx1", "mtx2"])
        # Create teradataml DataFrame objects.
        df1 = DataFrame.from_table("mtx1")
        df2 = DataFrame.from_table("mtx2")
        # Create teradataml TDMatrix objects.
        data1_matrix_df = TDMatrix(data=df1, 
                                   id='buoy_id', 
                                   row_index='row_i',
                                   column_index='column_i', 
                                   row_index_style="SEQUENCE",
                                   column_index_style="SEQUENCE", 
                                   payload_field='speed1',
                                   payload_content='REAL')
        data2_matrix_df = TDMatrix(data=df2, 
                                   id='buoy_id', 
                                   row_index='row_i',
                                   column_index='column_i', 
                                   row_index_style="SEQUENCE",
                                   column_index_style="SEQUENCE", 
                                   payload_field='speed2',
                                   payload_content='REAL')
        # Example 1 : Perform a point-wise mathematical operation against two matrixes
        #             having the same number of wavelets and having the same number of data-points
        #             within a same wavelet-point from the two matrices.
        uaf_out = MatrixMultiply(data1=data1_matrix_df,
                                 data2=data2_matrix_df,
                                 input_fmt_input_mode='MATCH')
        # Print the result DataFrame.
        print(uaf_out.result)
    Resample
    Resample
    Functions
    Resample(data=None, data_ﬁlter_expr=None, timecode_start_value=None, timecode_duration=None, sequence_start_value=None, sequence_duration=None,
    interpolate=None, weight=None, spline_params_method='NOT_A_KNOT', spline_params_yp1=0.0, spline_params_ypn=0.0,
    output_fmt_index_style='FLOW_THROUGH', **generic_arguments)
    DESCRIPTION:
        The Resample() function transforms an irregular time series into a
        regular time series. It can also be used to alter the sampling interval
        for a time series.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the irregular time series that is to be altered.
            Types: TDSeries
        data_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data".
            Types: ColumnExpression
        timecode_start_value:
            Optional Argument.
            Specifies the first sampling index to interpolate.
            Note:
                Provide either arguments "timecode_start_value" and
                "timecode_duration", or arguments "sequence_start_value"
                and "sequence_duration".
            Types: str
        timecode_duration:
            Optional Argument.
            Specifies the sampling interval associated with the result series.
            Note:
                Provide either arguments "timecode_start_value" and
                "timecode_duration", or arguments "sequence_start_value"
                and "sequence_duration".
            Permitted Values:
                * CAL_YEARS
                * CAL_MONTHS
                * CAL_DAYS
                * WEEKS
                * DAYS
                * HOURS
                * MINUTES
                * SECONDS
                * MILLISECONDS
                * MICROSECONDS
            Types: str
        sequence_start_value:
            Optional Argument.
            Specifies the first sampling index to interpolate.
            Note:
                Provide either arguments "timecode_start_value" and
                "timecode_duration", or arguments "sequence_start_value"
                and "sequence_duration".
            Types: int OR float
        sequence_duration:
            Optional Argument.
            Specifies the sampling interval associated with the result series.
            Note:
                Provide either arguments "timecode_start_value" and
                "timecode_duration", or arguments "sequence_start_value"
                and "sequence_duration".
            Types: int OR float
        interpolate:
            Required Argument.
            Specifies the interpolation strategies.
            Permitted Values:
                * LINEAR
                * LAG
                * LEAD
                * WEIGHTED
                * SPLINE
            Types: str
        weight:
            Optional Argument.
            Specifies the interpolated weighted value.
            Note:
                * Applicable only when "interpolate" set to 'WEIGHTED'.
                * The interpolated value is calculated as: Y_t  =  Y_{t_LEFT} * (1 - WEIGHT) + (Y-{t_RIGHT} * WEIGHT).
            Types: int OR float
        spline_params_method:
            Optional Argument.
            Specifies the type of spline method to use.
            Note:
                * Applicable only when "interpolate" set to 'SPLINE'.
            Permitted Values:
                * CLAMPED
                * NATURAL
                * NOT_A_KNOT
            Default Value: NOT_A_KNOT
            Types: str
        spline_params_yp1:
            Optional Argument.
            Specifies the value of the first derivative for the left boundary
            condition.
            Notes:
                * Used only when "interpolate" set to 'SPLINE'.
                * Used only when "spline_params_method" set to 'CLAMPED'.
            Default Value: 0.0
            Types: int OR float
        spline_params_ypn:
            Optional Argument.
            Specifies the value of the first derivative for the right boundary
            condition.
            Notes:
                * Used only when "interpolate" set to 'SPLINE'.
                * Used only when "spline_params_method" set to 'CLAMPED'.
            Default Value: 0.0
            Types: int OR float
        output_fmt_index_style:
            Optional Argument.
            Specifies the index style of the output format.
            Permitted Values: NUMERICAL_SEQUENCE, FLOW_THROUGH
            Default Value: FLOW_THROUGH
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an 
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results. 
                    If not specified, a unique table name is internally 
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output 
                    table into. If not specified, table is created into 
                    database specified by the user at the time of context 
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of Resample.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as Resample_obj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Load the example data.
        load_example_data("uaf", ["production_data"])
        # Create teradataml DataFrame object.
        production_data = DataFrame.from_table("production_data")
        # Example 1 : Execute function to transform irregular time series into
        #             regular time series when row index style is "SEQUENCE".
        # Create teradataml TDSeries object.
        data_series_df = TDSeries(data=production_data,
                                  id="product_id",
                                  row_index_style="SEQUENCE",
                                  row_index="TD_SEQNO",
                                  payload_field="beer_sales",
                                  payload_content="REAL")
        # Execute Resample for TDSeries.
        uaf_out1 = Resample(data=data_series_df,
                            interpolate='LINEAR',
                            sequence_start_value=0.0,
                            sequence_duration=1.0)
        # Print the result DataFrame.
        print(uaf_out1.result)
        # Example 2 : Execute function to transform irregular time series into
        #             regular time series when row index style is "TIMECODE".
        # Create teradataml TDSeries object.
        data_series_df = TDSeries(data=production_data,
                                  id="product_id",
                                  row_index_style="TIMECODE",
                                  row_index="TD_TIMECODE",
                                  payload_field="beer_sales",
                                  payload_content="REAL")
        # Execute Resample for TDSeries.
        uaf_out2 = Resample(data=data_series_df,
                            interpolate='LINEAR',
                            timecode_start_value="TIMESTAMP '2021-01-01 00:00:00'",
                            timecode_duration="MINUTES(30)")
        # Print the result DataFrame.
        print(uaf_out2.result)
    DIAGNOSTIC STATISTICAL TEST functions
    BreuschGodfrey
    BreuschGodfrey
    Functions
    BreuschGodfrey(data=None, data_ﬁlter_expr=None, residual_max_lags=None, explanatory_count=None, signiﬁcance_level=0.05, **generic_arguments)
    DESCRIPTION:
        The BreuschGodfrey() function checks for the presence of serial correlation
        among the residual and error terms after running a regression
        associated with a fitted model. With respect to regression models,
        it is expected that there is no serial correlation among the error terms.
        The following procedure is an example of how to use BreuschGodfrey:
            * Use LinearRegr() for regression testing.
            * Use BreuschGodfrey() on the result from LinearRegr() to
              compute the test statistics and determine if there is serial correlation.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the residual multivariate series
            or TDAnalyticResult object on the output generated
            by the UAF function.
            Types: TDSeries, TDAnalyticResult
        data_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data".
            Types: ColumnExpression
        residual_max_lags:
            Required Argument.
            Specifies the maximum lag number for the
            residuals used in the auxiliary regression. It also
            determines degrees of freedom associated with the test.
            Types: int
        explanatory_count:
            Required Argument.
            Specifies the number of explanatory variables that are present
            in the original regression plus '1' if a constant is
            present.
            Example: If the "formula" parameter used in LinearRegr() is 'Y = B0 + B1*X1',
            it has "explanatory_count" of '2', '1' for 'X1' and '1' for the
            constant 'B0'.
            Types: int
        significance_level:
            Optional Argument.
            Specifies the desired significance level for the test.
            Default Value: 0.05
            Types: float
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results.
                    If not specified, a unique table name is internally
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output
                    table into. If not specified, table is created into
                    database specified by the user at the time of context
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of BreuschGodfrey.
        Output teradataml DataFrames can be accessed using attribute
        references, such as BreuschGodfrey_obj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Load the example data.
        load_example_data("uaf", ["house_values"])
        # Create teradataml DataFrame object.
        data = DataFrame.from_table("house_values")
        # Execute LinearRegr() function to fit house_values data to
        # the curve mentioned in the "formula" for cityid 33. It returns
        # a result containing solved coefficients, model statistics,
        # and residuals statistics.
        # Create teradataml TDSeries object.
        data_series_df = TDSeries(data=data,
                                  id="cityid",
                                  row_index="TD_TIMECODE",
                                  row_index_style="TIMECODE",
                                  payload_field=["salary","house_val"],
                                  payload_content="MULTIVAR_REAL")
        filter_expr = data_series_df.cityid==33
        linear_regr_result = LinearRegr(data=data_series_df,
                                        data_filter_expr=filter_expr,
                                        formula = "Y = B0 + B1*X1",
                                        weights=False,
                                        algorithm='QR',
                                        coeff_stats=True,
                                        variables_count=2,
                                        model_stats=True,
                                        residuals=True)
        # Example 1: Compute the serial correlation among the residual
        #            and error terms. Input is teradataml TDAnalyticResult
        #            object over the output generated by running a linear
        #            regression associated with a fitted model.
        # Create teradataml TDAnalyticResult object.
        data_art_df =  TDAnalyticResult(data=linear_regr_result.result)
        uaf_out = BreuschGodfrey(data=data_art_df,
                                 explanatory_count=2,
                                 residual_max_lags=1,
                                 significance_level=.01)
        # Print the result DataFrame.
        print(uaf_out.result)
        # Example 2: Compute the serial correlation among the residual
        #            and error terms. Input is teradataml TDSeries
        #            object over the "fitresiduals" dataframe generated
        #            by running a linear regression associated
        #            with a fitted model.
        data_series_bg = TDSeries(data=linear_regr_result.fitresiduals,
                                  id="cityid",
                                  row_index="ROW_I",
                                  row_index_style= "SEQUENCE",
                                  payload_field=["RESIDUAL","ACTUAL_VALUE","CALC_VALUE"],
                                  payload_content="MULTIVAR_REAL")
        uaf_out = BreuschGodfrey(data=data_series_bg,
                                 explanatory_count=2,
                                 residual_max_lags=1,
                                 significance_level=.01)
        # Print the result DataFrame.
        print(uaf_out.result)
    BreuschPaganGodfrey
    BreuschPaganGodfrey
    Functions
    BreuschPaganGodfrey(data=None, data_ﬁlter_expr=None, variables_count=None, formula=None, studentize=False, signiﬁcance_level=0.05,
    **generic_arguments)
    DESCRIPTION:
        The BreuschPaganGodfrey() function checks for heteroscedasticity using 
        one or more variables among the residual terms after running a regression.
        The following procedure is an example of how to use BreuschPaganGodfrey() function:
            * Use MultivarRegr() to create regression model and generate residuals.
            * Use BreuschPaganGodfrey() on the output DataFrame from MultivarRegr().
            * Check the 'NULL_HYPOTHESIS' value to determine if there is heteroscedasticity. 
              ACCEPT means that variance is homoscedastic. 
              REJECT means that variance is heteroscedastic.       
    PARAMETERS:
        data:
            Required Argument.
            Specifies the residual multivariate series or 
            TDAnalyticResult object created over output 
            generated by the UAF regression function.
            Types: TDSeries, TDAnalyticResult
        data_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data".
            Types: ColumnExpression
        variables_count:
            Required Argument.
            Specifies the number of explanatory variables 
            that are used in the auxiliary regression.
            Types: int
        formula:
            Optional Argument.
            Specifies the regression formula to use for the 
            auxiliary regression. If a formula is not included, 
            then the default regression formula is used.
            Types: str
        studentize:
            Optional Argument.
            Specifies whether to use the Koenker studentized 
            version of the Breusch-Pagan-Godfrey (BPG) test.   
            When set to True, Koenker studentized version is 
            used, otherwise the standard BPG test is used.
            Default Value: False
            Types: bool
        significance_level:
            Optional Argument.
            Specifies the desired significance level for the test.
            Default Value: 0.05
            Types: float
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an 
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results. 
                    If not specified, a unique table name is internally 
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output 
                    table into. If not specified, table is created into 
                    database specified by the user at the time of context 
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of BreuschPaganGodfrey.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as BreuschPaganGodfrey_obj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Load the example data.
        load_example_data("uaf", ["house_values"])
        # Create teradataml DataFrame object.
        data = DataFrame.from_table("house_values")
        # Create teradataml TDSeries object.
        data_series_df = TDSeries(data=data,
                                  id="cityid",
                                  row_index="TD_TIMECODE",
                                  payload_field=["house_val","salary","mortgage"],
                                  payload_content="MULTIVAR_REAL")
        # Create Multivariate regression model.
        mvr_out = MultivarRegr(data=data_series_df,
                               variables_count=3,
                               weights=False,
                               formula="Y = B0 + B1*X1 + B2*X2",
                               algorithm='QR',
                               coeff_stats=True,
                               model_stats=True,
                               residuals=True)
        # Example 1: Perform Breusch-Pagan-Godfrey (BPG) test using input as teradataml 
        #            TDSeries object generated from model residuals.
        # Extract residuals from the model as TDSeries.
        data_series_bg = TDSeries(data=mvr_out.fitresiduals,
                                  id="cityid",
                                  row_index="ROW_I",
                                  row_index_style= "SEQUENCE",
                                  payload_field=["RESIDUAL","ACTUAL_VALUE","CALC_VALUE"],
                                  payload_content="MULTIVAR_REAL")
        uaf_out = BreuschPaganGodfrey(data=data_series_bg,
                                      variables_count=2,
                                      significance_level=0.01)
        # Print the result DataFrame.
        print(uaf_out.result)
        # Example 2: Perform Koenker studentized version of the Breusch-Pagan-Godfrey 
        #            (BPG) test using input as teradataml TDAnalyticResult object
        #            generated from model output.
        # Create teradataml TDAnalyticResult object from model output.
        data_art_df =  TDAnalyticResult(data=mvr_out.result)
        uaf_out = BreuschPaganGodfrey(data=data_art_df,
                                      variables_count=2,
                                      studentize=True,
                                      significance_level=0.01)
        # Print the result DataFrame.
        print(uaf_out.result)
    CumulPeriodogram
    CumulPeriodogram
    Functions
    CumulPeriodogram(data=None, data_ﬁlter_expr=None, signiﬁcance_level=None, **generic_arguments)
    DESCRIPTION:
        The CumulPeriodogram() function is used to perform a cumulative periodogram
        statistics test, also known as Bartlett's test, to determine
        the optimal data model. The series sample set is divided into two portions:
            * The first portion is used for the 'model fitting' exercise.
            * The second portion is used for the 'model validation'
              exercise. CumulPeriodogram() function is usually
              executed against the residuals produced during the second
              'model validation' exercise, meaning against the
              'in-sample' forecasted data points.
       CumulPeriodogram() is similar to the SignifPeriodicities() function,
       but the CumulPeriodogram() function tests periodicities at the same time
       instead of sequentially.
        The following procedure is an example of how to use CumulPeriodogram():
            1. Use ArimaEstimate() to identify spectral candidates.
            2. Use ArimaValidate() to validate spectral candidates.
            4. Execute CumulPeriodogram() using the residuals.
            5. See the null hypothesis result from CumulPeriodogram().
            6. Use DataFrame.plot() to plot the results.
    PARAMETERS:
        data:
            Required Argument.
            Specifies a single univariate series with calculated residuals
            from original regression or TDAnalyticResult object over the
            output containing residuals generated by the UAF functions.
            Types: TDSeries, TDAnalyticResult
        data_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data".
            Types: ColumnExpression
        significance_level:
            Required Argument.
            Specifies the significance level to be associated with the test.
            Permitted Values: 0.05, 0.01
            Types: float
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an 
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results. 
                    If not specified, a unique table name is internally 
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output 
                    table into. If not specified, table is created into 
                    database specified by the user at the time of context 
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of CumulPeriodogram.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as CumulPeriodogram_obj.<attribute_name>.
        Output teradataml DataFrame attribute names are:
            1. result
            2. cpdata
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Load the example data.
        load_example_data("uaf", "timeseriesdatasetsd4")
        # Create teradataml DataFrame object.
        df = DataFrame("timeseriesdatasetsd4")
        # Create teradataml TDSeries object over 'df' dataframe.
        td_series = TDSeries(data = df,
                             id="dataset_id",
                             row_index="seqno",
                             row_index_style="SEQUENCE",
                             payload_field = "magnitude",
                             payload_content="REAL")
        # Create a filter expression to filter the dataset using dataset_id 552.
        filter_expr = df.dataset_id==552
        # Perform parameter estimation using ArimaEstimate() function.
        arima_estimate = ArimaEstimate(data1=td_series,
                                       data_filter_expr=filter_expr,
                                       nonseasonal_model_order=[2, 0, 0],
                                       constant=True,
                                       algorithm="MLE",
                                       fit_percentage=70,
                                       fit_metrics=True,
                                       coeff_stats=True,
                                       residuals=True,
                                       max_iterations=100)
        # Create TDAnalyticResult object on the result attribute of arima_estimate
        # generated by ArimaEstimate() function.
        art_inp = TDAnalyticResult(arima_estimate.result)
        # Validate the 'goodness of fit' of the model using ArimaValidate() function.
        arima_validate = ArimaValidate(data=art_inp,
                                       fit_metrics=True,
                                       residuals=True)
        # Example 1: Perform statistical test using CumulPeriodogram()
        #            with input as TDSeries object created over the 'fitresiduals'
        #            attribute of arima_validate generated by running ArimaValidate() and
        #            with a significance level of 0.05.
        # Create teradataml TDSeries object on the 'fitresiduals' attribute of arima_validate
        # generated by ArimaValidate() function.
        data_series_df = TDSeries(data=arima_validate.fitresiduals,
                                  id="dataset_id",
                                  row_index="ROW_I",
                                  row_index_style="SEQUENCE",
                                  payload_field="RESIDUAL",
                                  payload_content="REAL")
        uaf_out = CumulPeriodogram(data=data_series_df, 
                                   significance_level=0.05)
        # Print the result DataFrames.
        print(uaf_out.result)
        print(uaf_out.cpdata)
        # Example 2: Perform statistical test using CumulPeriodogram()
        #            with input as TDAnalyticResult object created over
        #            the result attribute of arima_validate
        #            generated by ArimaValidate() function and
        #            with a significance level of 0.05.
        # Create teradataml TDAnalyticResult object on the result attribute of arima_validate
        # generated by ArimaValidate() function with layer as 'ARTFITRESIDUALS'.
        art_df = TDAnalyticResult(data=arima_validate.result, layer="ARTFITRESIDUALS")
        uaf_out = CumulPeriodogram(data=art_df, 
                                   significance_level=0.05)
        # Print the result DataFrames.
        print(uaf_out.result)
        print(uaf_out.cpdata)
    DickeyFuller
    DickeyFuller
    Functions
    DickeyFuller(data=None, data_ﬁlter_expr=None, algorithm=None, max_lags=0, **generic_arguments)
    DESCRIPTION:
        The DickeyFuller() function tests for the presence of one or more
        unit roots in a series to determine if the series is non-stationary.
        When a series contains unit roots, it is non-stationary. When a series
        contains no unit roots, whether the series is stationary is based on
        other factors.
        The following procedure is an example of how to use DickeyFuller() function:
            * Run DickeyFuller() on the time series being modeled.
            * Retrieve the results of the DickeyFuller() test to determine if the
              time series contains any unit roots.
            * If unit roots are present, use a technique such as differencing such as Diff()
              or seasonal normalization, such as SeasonalNormalize(), to create a new series,
              then rerun the DickeyFuller() test to verify that the differenced or
              seasonally-normalized series unit root are removed.
            * If the result shows unit roots, use Diff() and SeasonalNormalize()
              to remove unit roots.
    PARAMETERS:
        data:
            Required Argument.
            Specifies a single logical-runtime series as an input or TDAnalyticResult which
            contains ARTFITRESIDUALS layer.
            Types: TDSeries, TDAnalyticResult
        data_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data".
            Types: ColumnExpression
        algorithm:
            Required Argument.
            Specifies the type of regression that is run for the test.
            Permitted Values:
                * NONE: Random walk
                * DRIFT: Random walk with drift
                * DRIFTNTREND: Random walk with drift and trend
                * SQUARED: Random walk with drift, trend, and
                           quadratic trend.
            Types: str
        max_lags:
            Optional Argument.
            Specifies the maximum number of lags to use with the regression
            equation. Range is [0, 100]
            DefaultValue: 0
            Types: int
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an 
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results. 
                    If not specified, a unique table name is internally 
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output 
                    table into. If not specified, table is created into 
                    database specified by the user at the time of context 
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of DickeyFuller.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as DickeyFuller_obj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Load the example data.
        load_example_data("uaf","timeseriesdatasetsd4")
        # Create teradataml DataFrame object.
        df = DataFrame.from_table("timeseriesdatasetsd4")
        # Create teradataml TDSeries object.
        data_series_df = TDSeries(data=df,
                                  id="dataset_id",
                                  row_index="seqno",
                                  row_index_style= "SEQUENCE",
                                  payload_field="magnitude",
                                  payload_content="REAL")
        # Example 1 : Determine whether the series is non-stationary by testing
        #             for the presence of the unit roots using random walk with
        #             linear trend for regression.
        uaf_out = DickeyFuller(data=data_series_df,
                               algorithm='DRIFT')
        # Print the result DataFrame.
        print(uaf_out.result)
    DurbinWatson
    DurbinWatson
    Functions
    DurbinWatson(data=None, data_ﬁlter_expr=None, explanatory_count=None, include_constant=False, method=None, signiﬁcance_level=None,
    **generic_arguments)
    DESCRIPTION:
        The DurbinWatson() function determines serial correlation between
        residuals within an independent time series data, or in the residual result
        of TDAnalyticResult.
        The following procedure is an example of how to use DurbinWatson() function:
            * Use LinearRegr() on the residuals.
            * Use DurbinWatson() on the output from LinearRegr() to determine
              if there is serial correlation.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input series or a TDAnalyticResult object
            created over the output residual generated by the UAF function.
            Types: TDSeries, TDAnalyticResult
        data_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data".
            Types: ColumnExpression
        explanatory_count:
            Required Argument.
            Specifies the number of explanatory variables in the original regression.
            The number of explanatory variables along with the "include_constant"
            information is needed to perform the lookup in the Durbin-Watson data.
            Types: int
        include_constant:
            Optional Argument.
            Specifies whether the original regression equation contains a constant,
            also known as an intercept. When set to False, no constant is present,
            otherwise constant is present.
            Default Value: False
            Types: bool
        method:
            Required Argument.
            Specifies the enumerated value specifying the formula to calculate
            the Durbin-Watson test statistic value.
            Permitted Values:
                * DW_FORMULA: use the full summation formula to calculate
                              the value.
                * ACR_LAG1: perform the regression with an autocorrelation
                           of lag 1, and use as the role of the Durbin-Watson
                           statistic.
            Types: str
        significance_level:
            Required Argument.
            Specifies the significance level for the test. Value must be greater
            than 0 and less than 1. For example, 0.01, or 0.05.
            Types: float
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an 
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results. 
                    If not specified, a unique table name is internally 
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output 
                    table into. If not specified, table is created into 
                    database specified by the user at the time of context 
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of DurbinWatson.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as DurbinWatson_obj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Load the example data.
        load_example_data("uaf", ["house_values2"])
        # Create teradataml DataFrame object.
        data = DataFrame.from_table("house_values2")
        # Create teradataml TDSeries object.
        linearregr_series = TDSeries(data=data,
                                     id="cid",
                                     row_index_style="SEQUENCE",
                                     row_index="s_no",
                                     payload_field=["house_value", "salary"],
                                     payload_content="MULTIVAR_REAL"
                                     )
        # The LinearRegr() function fits TDSeries data to
        # the curve mentioned in the "formula." It returns
        # a result containing solved coefficients, model statistics,
        # and residuals statistics.
        linearregr_output = LinearRegr(data=linearregr_series,
                                       variables_count=2,
                                       weights=False,
                                       formula="Y=B0+B1*X1",
                                       algorithm='QR',
                                       model_stats=True,
                                       coeff_stats=False,
                                       residuals=True
                                       )
        # Example 1: Determine the serial correlation using TDAnalyticResult.
        # Create teradataml TDAnalyticResult object.
        artspec=TDAnalyticResult(data=linearregr_output.result)
        uaf_out1 = DurbinWatson(data=artspec,
                                explanatory_count=2,
                                method="DW_FORMULA",
                                significance_level=0.05)
        # Print the result DataFrame.
        print(uaf_out1.result)
        # Example 2: Determine the serial correlation using TDSeries.
        # Create teradataml TDSeries object.
        seriesspec = TDSeries(data=linearregr_output.fitresiduals,
                              id="cid",
                              row_index_style="SEQUENCE",
                              row_index="ROW_I",
                              payload_field="RESIDUAL",
                              payload_content="REAL"
                              )
        uaf_out2 = DurbinWatson(data=seriesspec,
                                explanatory_count=2,
                                method="DW_FORMULA",
                                significance_level=0.05)
        # Print the result DataFrame.
        print(uaf_out2.result)
    FitMetrics
    FitMetrics
    Functions
    FitMetrics(data=None, data_ﬁlter_expr=None, var_count=None, fstat=False, signiﬁcance_level=None, **generic_arguments)
    DESCRIPTION:
        The FitMetrics() takes the original series, the model-predicted series,
        the original series mean and the modeling residuals to generate the
        goodness-of-fit of the modeling exercise.
    PARAMETERS:
        data:
            Required Argument.
            Specifies a single multivariate series as an input or a
            TDAnalyticResult object over the residual results from
            a previously run regression operation.
            When multivariate series is the input, the three fields
            should be the original series, predicted series,
            and residuals from the original regression.
            Types: TDSeries, TDAnalyticResult
        data_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data".
            Types: ColumnExpression
        var_count:
            Required Argument.
            Specifies an integer indicating how many explanatory variables
            including the constant are used while calculating the fitness
            metrics.
            Types: int
        fstat:
            Optional Argument.
            Specifies whether to include F-test related
            statistics in the final result.
            When set to True, results are included otherwise,
            results are not included.
            Default Value: False
            Types: bool
        significance_level:
            Optional Argument.
            Specifies the significance level for the test.
            Value should be between 0 and 1.
            Note:
                Valid only when "fstat" is set to True.
            Types: float
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results.
                    If not specified, a unique table name is internally
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output
                    table into. If not specified, table is created into
                    database specified by the user at the time of context
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of FitMetrics.
        Output teradataml DataFrames can be accessed using attribute
        references, such as FitMetrics_obj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Load the example data.
        load_example_data("uaf", ["timeseriesdatasetsd4"])
        # Create teradataml DataFrame object.
        data = DataFrame.from_table("timeseriesdatasetsd4")
        # Execute ArimaEstimate() function to estimate the coefficients
        # and statistical ratings corresponding to an ARIMA model.
        # Create teradataml TDSeries object.
        data_series_df = TDSeries(data=data,
                                  id="dataset_id",
                                  row_index="seqno",
                                  row_index_style="SEQUENCE",
                                  payload_field="magnitude",
                                  payload_content="REAL")
        # Execute ArimaEstimate function.
        arima_estimate_op = ArimaEstimate(data1=data_series_df,
                                          nonseasonal_model_order=[2,0,0],
                                          constant=False,
                                          algorithm="MLE",
                                          coeff_stats=True,
                                          fit_metrics=True,
                                          residuals=True,
                                          fit_percentage=80)
        # Example 1: Generate goodness of fit metrics by using TDAnalyticResult
        #            object over the result attribute of 'arima_estimate_op'
        #            with 'ARTFITRESIDUALS' layer as input.
        # Create teradataml TDAnalyticResult object.
        data_art_df = TDAnalyticResult(data=arima_estimate_op.result,
                                       layer="ARTFITRESIDUALS")
        uaf_out = FitMetrics(data=data_art_df,
                             var_count=5,
                             fstat=True,
                             significance_level=0.05)
        # Print the result DataFrame.
        print(uaf_out.result)
        # Example 2: Generate goodness of fit metrics by using TDSeries over
        #            the 'fitresiduals' attribute of 'arima_estimate_op'
        #            as input.
        # Create teradataml TDSeries object.
        data_series_df = TDSeries(data=arima_estimate_op.fitresiduals,
                                  id="dataset_id",
                                  row_index="ROW_I",
                                  row_index_style="SEQUENCE",
                                  payload_field=["RESIDUAL", "ACTUAL_VALUE",
                                                 "CALC_VALUE"],
                                  payload_content="MULTIVAR_REAL")
        uaf_out = FitMetrics(data=data_series_df, var_count=5)
        # Print the result DataFrame.
        print(uaf_out.result)
    GoldfeldQuandt
    GoldfeldQuandt
    Functions
    GoldfeldQuandt(data=None, data_ﬁlter_expr=None, const_term=True, algorithm=None, start_idx=None, omit=None, signiﬁcance_level=None, test='GREATER',
    **generic_arguments)
    DESCRIPTION:
        The GoldfeldQuandt() function is a statistical test to determine
        a regression model with the Best Linear Unbiased Estimator (BLUE).
        The test checks for homoscedasticity (constant variance) in
        regression analyses.
    PARAMETERS:
        data:
            Required Argument.
            Specifies an input time series to be tested for
            heteroscedasticity. The "row_axis" determines
            the order of TDSeries data.
            Types: TDSeries
        data_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data".
            Types: ColumnExpression
        const_term:
            Optional Argument.
            Specifies the indicator of whether the regression performed should
            use a Y-intercept coefficient.
            When set to True, means the regression is performed on “Y=C+aX1+bX2+…”.
            When set to False, means the regression is performed on “Y=aX1+bX2+…”.
            Default Value: True
            Types: bool
        algorithm:
            Required Argument.
            Specifies the algorithm used for the regression.
            Permitted Values:
                1. QR: QR decomposition is used for the regression.
                2. PSI: pseudo-inverse based on singular value
                   decomposition (SVD) is used to solve the regression.
            Types: str
        start_idx:
            Optional Argument.
            Specifies the split-point index for the function. 
            When argument is:
                * less than 1.0, then the split-point index is calculated as:
                    split_point_index = start_idx * N
                    Where, 'N' is the total number of sample rows.
                * greater than 1.0, then "start_idx" is the split-point index.
                * not specified, then split-point index is calculate as:
                    start_idx = (N - omit) / 2
                    Where, 'N' is the total number of entries in the data series.
            Types: int OR float
        omit:
            Required Argument.
            Specifies the number of central sample values to omit when
            forming the two test groups. 
            When argument is:
                * less than 1.0, then the number of samples to be omitted is
                  calculated as:
                    omit_sample_count = omit * N
                    Where 'N' is the total number of entries in the data series
                * greater than 1.0, then "omit" is interpreted as number of 
                  central points to omit.
            Types: int OR float
        significance_level:
            Required Argument.
            Specifies the significance level for the test.
            Types: float
        test:
            Optional Argument.
            Specifies the test method for Goldfeld-Quandt test 
            statistic computation and hypothesis evaluation.
            Permitted Values:
                * GREATER: If the Goldfeld-Quandt test-statistic is less
                           than the higher critical value, the null hypothesis
                           is accepted, and there is no evidence of
                           heteroscedastic variance.
                           If the Goldfeld-Quandt test statistic is greater
                           than or equal to the critical value, then the null 
                           hypothesis is rejected, and there is evidence of 
                           heteroscedastic variance.
                * LESS: If the Goldfeld-Quandt test-statistic is greater than
                        the lower critical value, the null hypothesis is accepted,
                        and there is no evidence of heteroscedastic variance.
                        If the Goldfeld-Quandt test statistic is less than or
                        equal to than the critical value, then the null hypothesis
                        is rejected, and there is evidence of heteroscedastic
                        variance.
                * TWOSIDED: If the Goldfeld-Quandt test-statistic is greater than
                            the lower tail critical value and less than the higher
                            tail critical value, the null hypothesis is accepted,
                            and there is no evidence of heteroscedastic variance.
                            If the Goldfeld-Quandt test statistic is less than or
                            equal to the lower tail critical value or greater than
                            or equal to the high tail critical value, then the
                            null hypothesis is rejected, and there is evidence
                            of heteroscedastic variance.
            Default Value: GREATER
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an 
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results. 
                    If not specified, a unique table name is internally 
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output 
                    table into. If not specified, table is created into 
                    database specified by the user at the time of context 
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of GoldfeldQuandt.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as GoldfeldQuandt_obj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Load the example data.
        load_example_data("uaf", ["gq_t1"])
        # Create teradataml DataFrame object.
        data = DataFrame.from_table("gq_t1")
        # Example 1: Execute the GoldfeldQuandt() function on TDSeries input
        #            to check for homoscedasticity in regression analyses.
        # Create teradataml TDSeries object.
        data_series_df = TDSeries(data=data,
                                 id="series_id",
                                 row_index="row_i",
                                 row_index_style="SEQUENCE",
                                 payload_field=["y1", "x1"],
                                 payload_content="MULTIVAR_REAL")
        # Execute GoldfeldQuandt.
        uaf_out = GoldfeldQuandt(data=data_series_df,
                                 omit=2.0,
                                 significance_level=0.05,
                                 algorithm="QR")
        # Print the result DataFrame.
        print(uaf_out.result)
    Portman
    Portman
    Functions
    Portman(data=None, data_ﬁlter_expr=None, max_lags=None, test=None, degrees_freedom=None, pacf_method=None, signiﬁcance_level=None,
    **generic_arguments)
    DESCRIPTION:
        The Portman() function (Portmanteau test) uses a series of tests to
        determine whether the residuals can be classified as zero mean, no
        evidence of serial correlation, or the residuals exhibit homoscedastic
        variance. These residuals are also referred to as white noise. All the
        tests assume that the calculated statistic follows a chi-squared distribution.
        The following procedure is an example of how to use Portman() function:
            1. Use ArimaEstimate() function to get residuals from the data set.
            2. Use ArimaValidate() function to validate the output.
            3. Use Portman() to check the residuals for zero mean white noise using the 
            "fitresiduals" output attribute of ArimaValidate() function.
    PARAMETERS:
        data:
            Required Argument.
            Specifies a residual univariate series data.
            Types: TDSeries, TDAnalyticResult
        data_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data".
            Types: ColumnExpression
        max_lags:
            Required Argument.
            Specifies the number of explanatory variables referenced in the payload
            declaration in the original series specification. These
            explanatory variables are the variables that are used for the
            auxiliary regression.
            Types: int
        test:
            Required Argument.
            Specifies the Portmanteau test to use.
            Permitted Values:
                BP : Box-Pierce Q test. It calculates a test statistic value based
                     on the square of the auto-correlation coefficients associated
                     with the residual series. Result is expected to follow a Chi-squared
                     distribution.
                LB : Ljung-Box Q test. It calculates a test statistic value based on
                     the square of the auto-correlation coefficients adjusted by its
                     asymptotic variance.
                LM : Li-McLeod Q test. It calculates a test statistic value based
                     on the square of the auto-correlation coefficients, and does a
                     conservative adjustment to the value by its asymptotic variance.
                MQ : Monti Q test. It calculates a test statistic value based on
                     the square of the partial auto-correlation coefficients which
                     are then adjusted toward their asymptotic variance.
                ML : McLeod-Li Q test. It creates a new series from the residual
                     series by squaring each of the series entries, calculates the
                     auto-correlation coefficients associated with the new series,
                     and then calculates a test statistic value based on the square
                     of those auto-correlation coefficients, adjusted toward their
                     asymptotic variance.
            Types: str
        degrees_freedom:
            Required Argument.
            Specifies the degrees of freedom to be subtracted from "max_lags".
            Types: int
        pacf_method:
            Optional Argument.
            Specifies the underlying algorithm to calculate the partial auto-orrelation
            coefficients.
            Note:
                Applicable to Monti Q test only.
            Permitted Values: LEVINSON_DURBIN, OLS
            Types: str
        significance_level:
            Required Argument.
            Specifies the desired significance level for the test.
            Types: float
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an 
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results. 
                    If not specified, a unique table name is internally 
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output 
                    table into. If not specified, table is created into 
                    database specified by the user at the time of context 
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of Portman.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as Portman_obj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Load the example data.
        load_example_data("uaf","timeseriesdatasetsd4")
        # Create teradataml DataFrame object.
        df=DataFrame.from_table("timeseriesdatasetsd4")
        # Create teradataml TDSeries object for ArimaEstimate.
        series_arimaestimate = TDSeries(data=df,
                                        id="dataset_id",
                                        row_index="seqno",
                                        row_index_style="SEQUENCE",
                                        payload_field="Magnitude",
                                        payload_content="REAL")
        # Function outputs a result set that contains the estimated
        # coefficients with accompanying per-coefficient statistical ratings.
        arima_estimate = ArimaEstimate(data1=series_arimaestimate,
                                       nonseasonal_model_order=[1,0,0],
                                       constant=True,
                                       algorithm='MLE',
                                       fit_percentage=70,
                                       coeff_stats=True,
                                       fit_metrics=True,
                                       residuals=True
                                       )
        # Create teradataml TDAnalyticResult object for ArimaValidate.
        art_arimavalidate = TDAnalyticResult(data=arima_estimate.result)
        # Function produces a multiple outputs to return up to four result sets.
        arima_validate=ArimaValidate(data=art_arimavalidate,
                                     fit_metrics=True,
                                     residuals=True)
        # Create teradataml TDSeries object for Portman.
        series_portman = TDSeries(data=arima_validate.fitresiduals,
                                  id="dataset_id",
                                  row_index="ROW_I",
                                  row_index_style="SEQUENCE",
                                  payload_field="RESIDUAL",
                                  payload_content="REAL")
        # Example 1 : Calculates a test statistic value based
        #             on the square of the auto-correlation coefficients
        #             associated with the residual series using TDSeries.
        portman1 = Portman(data=series_portman,
                          max_lags=2,
                          test='BP',
                          degrees_freedom=1,
                          significance_level=0.05)
        # Print the result DataFrame.
        print(portman1.result)
        # Example 2 : Calculates a test statistic value based
        #             on the square of the auto-correlation coefficients
        #             associated with the residual series using TDAnalyticResult.
        # Create teradataml TDAnalyticResult object for Portman.
        art_portman = TDAnalyticResult(data=arima_validate.result,layer="ARTFITRESIDUALS")
        portman2 = Portman(data=art_portman,
                          max_lags=2,
                          test='BP',
                          degrees_freedom=1,
                          significance_level=0.05)
        # Print the result DataFrame.
        print(portman2.result)
    SelectionCriteria
    SelectionCriteria
    Functions
    SelectionCriteria(data=None, data_ﬁlter_expr=None, var_count=None, constant=False, use_likelihood=False, **generic_arguments)
    DESCRIPTION:
        The SelectionCriteria() function calculates metrics to help
        determine the users for an forecast modeling project.
    PARAMETERS:
        data:
            Required Argument.
            Specifies a single multivariate series or a TDAnalyticResult object
            created on the residual results from a previously run regression
            operation. A single multivariate series has the following fields:
                * First field is the original series value.
                * Second field is the calculated series value from the model.
                * Third field is the calculated residual, which is the original
                  value minus the calculated value.
            When TDAnalyticResult is used, make sure TDAnalyticResult is created without
            passing the "layer" argument.
            Types: TDSeries, TDAnalyticResult
        data_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data".
            Types: ColumnExpression
        var_count:
            Required Argument.
            Specifies the total number of arguments present in the model.
            Types: int
        constant:
            Optional Argument.
            Specifies whether model has a constant. When set to False,
            model has no constant, otherwise has constant.
            Default Value: False
            Types: bool
        use_likelihood:
            Optional Argument.
            Specifies whether the selection criteria use residual sum squares
            (RSS) or log-likelihood. When set to False, indicates RSS, otherwise
            indicates log-likelihood.
            Note:
                * Applicable only when input is from the ArimaEstimate() function.
            Default Value: False
            Types: bool
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an 
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results. 
                    If not specified, a unique table name is internally 
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output 
                    table into. If not specified, table is created into 
                    database specified by the user at the time of context 
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of SelectionCriteria.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as SelectionCriteria_obj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Load the example data.
        load_example_data("uaf", ["blood2ageandweight"])
        # Create teradataml DataFrame object.
        df = DataFrame.from_table("blood2ageandweight")
        # Create teradataml TDSeries object.
        series_arimaestimate = TDSeries(data=df,
                                        id="PatientID",
                                        row_index="SeqNo",
                                        row_index_style="SEQUENCE",
                                        payload_field="Age",
                                        payload_content="REAL")
        # Function outputs a result set that contains the estimated
        # coefficients with accompanying per-coefficient statistical ratings.
        arima_estimate = ArimaEstimate(data1=series_arimaestimate,
                                       nonseasonal_model_order=[1,0,2],
                                       constant=True,
                                       algorithm='MLE',
                                       fit_percentage=70,
                                       coeff_stats=True,
                                       fit_metrics=True,
                                       residuals=True
                                       )
        # Example 1 : Calculate the metrics on the series created on the output
        #             of ArimaEstimate() function.
        # Create teradataml TDSeries object.
        selectioncriteria_series = TDSeries(data=arima_estimate.fitresiduals,
                                            id="PatientID",
                                            row_index="ROW_I",
                                            row_index_style= "SEQUENCE",
                                            payload_field=["ACTUAL_VALUE",  "CALC_VALUE","RESIDUAL"],
                                            payload_content="MULTIVAR_REAL")
        uaf_out=SelectionCriteria(data=selectioncriteria_series,
                                  var_count=4,
                                  constant=True,
                                  use_likelihood=False)
        # Print the result DataFrame.
        print(uaf_out.result)
        # Example 2 : Calculate the metrics on the the output of ArimaEstimate()
        #             function. Note that output of ArimaEstimate() is encapsulated
        #             in TDAnalyticResult while passing as input.
        # Create teradataml TDAnalyticResult object.
        art_SelectionCriteria = TDAnalyticResult(data=arima_estimate.result)
        uaf_out=SelectionCriteria(data=art_SelectionCriteria,var_count=4,
                                  constant=True,
                                  use_likelihood=True)
        # Print the result DataFrame.
        print(uaf_out.result)
    SignifPeriodicities
    SignifPeriodicities
    Functions
    SignifPeriodicities(data=None, data_ﬁlter_expr=None, signiﬁcance_level=None, periodicities=None, **generic_arguments)
    DESCRIPTION:
        The SignifPeriodicities() function is a significance 
        of periodicities statistics test to determine the 
        optimum data model. It uses the residuals generated 
        during the model validation/selection phase. 
        Significance of periodicities statistics test is 
        usually performed against residuals produced during 
        the model validation phase, meaning against the 
        in-sample forecasted data points.
        The following procedure is an example of how to use 
        SignifPeriodicities():
            1. Use ArimaEstimate() to identify spectral candidates.
            2. Use ArimaValidate() to validate spectral candidates.
            3. Use DataFrame.plot() to plot the results.
            4. Compute the test statistic.
            5. Use SignifPeriodicities() on the periodicities of interest. 
               More than one periodicities can be entered using the 
               "periodicities" parameter.
    PARAMETERS:
        data:
            Required Argument.
            Specifies a single univariate series with calculated residuals
            from original regression or TDAnalyticResult object on the 
            residual output generated by the UAF functions.
            Types: TDSeries, TDAnalyticResult
        data_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data".
            Types: ColumnExpression
        significance_level:
            Required Argument.
            Specifies the significance level to be associated with
            the statistical F-test. Value must be greater than 0 and
            less than 1.
            Types: float
        periodicities:
            Required Argument.
            Specifies the significant periodicities to perform 
            tests for each period.
            Types: int, list of int, float, list of float
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an 
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results. 
                    If not specified, a unique table name is internally 
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output 
                    table into. If not specified, table is created into 
                    database specified by the user at the time of context 
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of SignifPeriodicities.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as SignifPeriodicities_obj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Load the example data.
        load_example_data("uaf", "timeseriesdatasetsd4")
        # Create teradataml DataFrame object.
        data = DataFrame.from_table("timeseriesdatasetsd4")
        # Create teradataml TDSeries object.
        from teradataml import TDSeries
        data_series_df = TDSeries(data = data, 
                                  id="dataset_id", 
                                  row_index="seqno", 
                                  row_index_style="SEQUENCE",
                                  payload_field = "Magnitude",
                                  payload_content="REAL")
        # Identify spectral candidates using ArimaEstimate().
        from teradataml import ArimaEstimate
        arima_estimate = ArimaEstimate(data1=data_series_df,
                                       nonseasonal_model_order=[2, 0, 0],
                                       constant=True,
                                       algorithm="MLE",
                                       fit_percentage=70,
                                       fit_metrics=True,
                                       coeff_stats=True,
                                       residuals=True,
                                       max_iterations=100)
        # Create teradataml TDAnalyticResult object
        # using the output of ArimaEstimate.
        from teradataml import TDAnalyticResult
        data_art_df = TDAnalyticResult(arima_estimate.result)
        # Validate spectral candidates using ArimaValidate().
        from teradataml import ArimaValidate
        arima_validate = ArimaValidate(data=data_art_df,
                                       fit_metrics=True,
                                       residuals=True)
        # Retrieve residuals from ArimaValidate() result.
        data_series_df_real = TDSeries(data=arima_validate.fitresiduals,
                                       id="dataset_id",
                                       row_index="ROW_I",
                                       row_index_style="SEQUENCE",
                                       payload_field="RESIDUAL",
                                       payload_content="REAL")
        # Example 1: Perform statistical tests using SignifPeriodicities() 
        #            function with TDSeries for multiple periodicities.
        uaf_out = SignifPeriodicities(data=data_series_df_real, 
                                      periodicities=[2.666666666, 5.333333333, 8.0], 
                                      significance_level=0.05)
        # Print the result DataFrame.
        print(uaf_out.result)
    SignifResidmean
    SignifResidmean
    Functions
    SignifResidmean(data=None, data_ﬁlter_expr=None, signiﬁcance_level=None, **generic_arguments)
    DESCRIPTION:
        The SignifResidmean() function is a significance of residual mean
        statistics test to determine the optimum data model.
        The function uses the residuals generated during model validation
        that used the forecasted data points.
        The following procedure is an example of how to use SignifResidmean() function:
            1. Divide the sample data into two sets. One set is used to fit the data to the
               model, and the other set is used to select between models.
            2. Use ArimaEstimate() to determine the coefficients for the test statistics
               to use with models.
            3. Use ArimaValidate() to validate the goodness of the coefficients.
            4. Use the residuals data from ArimaValidate() as an input for SignifResidmean().
            5. Retrieve the null hypothesis results from the SignifResidmean() output.
               A value of 1 means that the series has mean of zero. A value of 0 means
               that the series has non-zero mean.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input series or a TDAnalyticResult object
            created over the residuals data generated by the UAF function.
            Types: TDSeries, TDAnalyticResult
        data_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data".
            Types: ColumnExpression
        significance_level:
            Required Argument.
            Specifies the significance level for the test.
            Types: float
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an 
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results. 
                    If not specified, a unique table name is internally 
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output 
                    table into. If not specified, table is created into 
                    database specified by the user at the time of context 
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of SignifResidmean.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as SignifResidmean_obj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Load the example data.
        load_example_data("uaf","timeseriesdatasetsd4")
        # Create teradataml DataFrame object.
        df = DataFrame.from_table("timeseriesdatasetsd4")
        # Create teradataml TDSeries object for ArimaEstimate.
        series_arimaestimate = TDSeries(data=df,
                                        id="dataset_id",
                                        row_index="seqno",
                                        row_index_style="SEQUENCE",
                                        payload_field="Magnitude",
                                        payload_content="REAL")
        # Function outputs a result set that contains the estimated
        # coefficients with accompanying per-coefficient statistical ratings.
        arima_estimate = ArimaEstimate(data1=series_arimaestimate,
                                       nonseasonal_model_order=[1,0,0],
                                       constant=True,
                                       algorithm='MLE',
                                       fit_percentage=70,
                                       coeff_stats=True,
                                       fit_metrics=True,
                                       residuals=True)
        # Create teradataml TDAnalyticResult object for ArimaValidate.
        art_arimavalidate = TDAnalyticResult(data=arima_estimate.result)
        # Validate the goodness of the coefficients.
        arima_validate = ArimaValidate(data=art_arimavalidate,
                                     fit_metrics=True,
                                     residuals=True)
        # Example 1: Determine the optimum data model using TDSeries created over
        #            'fitresiduals' attribute of arima_validate as input.
        # Create teradataml TDSeries object.
        series_signifresidmean = TDSeries(data=arima_validate.fitresiduals,
                                          id="dataset_id",
                                          row_index="ROW_I",
                                          row_index_style="SEQUENCE",
                                          payload_field="RESIDUAL",
                                          payload_content="REAL")
        uaf_out1 = SignifResidmean(data=series_signifresidmean,
                                   significance_level=0.05)
        # Print the result DataFrame.
        print(uaf_out1.result)
        # Example 2: Determine the optimum data model using TDAnalyticResult created over
        #            'result' attribute of arima_validate with layer as input.
        # Create teradataml TDAnalyticResult object.
        art_signifresidmean = TDAnalyticResult(data=arima_validate.result,
                                               layer="ARTFITRESIDUALS")
        uaf_out2 = SignifResidmean(data=art_signifresidmean,
                                   significance_level=0.05)
        # Print the result DataFrame.
        print(uaf_out2.result)
    WhitesGeneral
    WhitesGeneral
    Functions
    WhitesGeneral(data=None, data_ﬁlter_expr=None, variables_count=None, signiﬁcance_level=None, **generic_arguments)
    DESCRIPTION:
        The WhitesGeneral() function checks for the presence of correlation
        among the residual terms after running a regression. The function
        determines if there exists any heteroscedastic variance in the
        residuals of regression tests.
        The output specifies the following:
            * 'ACCEPT' means the null hypothesis is accepted,
              and there is homoscedasticity variance evident.
            * 'REJECT' means the null hypothesis is rejected,
              and there is evidence of heteroscedasticity.
       The Whites-General test does not require reordering like
       the Goldfeld-Quandt test, and is not sensitive to the normal
       distribution assumption like the Breusch-Pagan-Godfrey test.
       The following procedure is an example of how to use WhitesGeneral() function:
            1. Use the function MultivarRegr() for regression testing.
            2. Use WhitesGeneral() on the residual output from MultivarRegr().
            3. Determine if the variance is homoscedastic or heteroscedastic
               from the WhitesGeneral() result.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the residual multivariate series or TDAnalyticResult
            object over the residual output of UAF regression functions.
            Input payload content is 'MULTIVAR_REAL'.
            Types: TDSeries, TDAnalyticResult
        data_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data".
            Types: ColumnExpression
        variables_count:
            Required Argument.
            Specifies the number of explanatory variables used
            in the auxiliary regression in the payload of the
            original series.
            Types: int
        significance_level:
            Required Argument.
            Specifies the significance level used for the test.
            Types: float
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results.
                    If not specified, a unique table name is internally
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output
                    table into. If not specified, table is created into
                    database specified by the user at the time of context
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of WhitesGeneral.
        Output teradataml DataFrames can be accessed using attribute
        references, such as WhitesGeneral_obj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Load the example data.
        load_example_data("uaf", ["house_values"])
        # Create teradataml DataFrame object.
        df = DataFrame.from_table("house_values")
        # Create teradataml TDSeries object.
        data_series_df = TDSeries(data=df,
                                  id="cityid",
                                  row_index="TD_TIMECODE",
                                  payload_field=["house_val","salary","mortgage"],
                                  payload_content="MULTIVAR_REAL")
        # Execute multivariate regression function to identify the degree of
        # linearity the explanatory variable and multiple response variables.
        # Generate the model statistics and residual data as well.
        multivar_out = MultivarRegr(data=data_series_df,
                                    variables_count=3,
                                    weights=False,
                                    formula="Y = B0 + B1*X1 + B2*X2",
                                    algorithm='QR',
                                    coeff_stats=True,
                                    model_stats=True,
                                    residuals=True)
        # Example 1: Perform Whites-General test on TDAnalyticResult object
        #            over the result attribute of the "multivar_out" as input.
        # Create teradataml TDAnalyticResult object over the 'result'
        # attribute of the 'multivar_out'.
        data_art_df = TDAnalyticResult(data=multivar_out.result)
        uaf_out = WhitesGeneral(data=data_art_df,
                                variables_count=3,
                                significance_level=0.05)
        # Print the result DataFrame.
        print(uaf_out.result)
        # Example 2: Perform Whites-General test on TDSeries
        #            object over the 'fitresiduals' attribute
        #            of the 'multivar_out'.
        # Create teradataml TDSeries object over the 'fitresiduals' attribute
        # of the 'multivar_out'.
        data_series_df = TDSeries(data=multivar_out.fitresiduals,
                                  id="cityid",
                                  row_index="ROW_I",
                                  row_index_style="SEQUENCE",
                                  payload_field=["ACTUAL_VALUE","CALC_VALUE","RESIDUAL"],
                                  payload_content="MULTIVAR_REAL")
        uaf_out1 = WhitesGeneral(data=data_series_df, 
                                 variables_count=3, 
                                 significance_level=0.05)
        # Print the result DataFrame.
        print(uaf_out1.result)
    TEMPORAL and SPATIAL functions
    Convolve
    Convolve
    Functions
    Convolve(data1=None, data1_ﬁlter_expr=None, data2=None, data2_ﬁlter_expr=None, algorithm=None, input_fmt_input_mode=None, **generic_arguments)
    DESCRIPTION:
        The Convolve() function applies a series representing a
        digital filter to a time series by convolving the two series. 
        The digital filter can be of any type such as low-pass, band-pass,
        band-reject, high-pass, and so on.
        User can use digital filters to separate time series that
        have been combined and to restore time series that have 
        become distorted.
    PARAMETERS:
        data1:
            Required Argument.
            Specifies the time series to be filtered.
            The time series have the following TDSeries characteristics.
                1. "payload_content" must have one of these values:
                    * REAL
                    * COMPLEX
                    * MULTIVAR_REAL
                    * MULTIVAR_COMPLEX
                2. The two time series must have the same "payload_content" value
                   and number of payload fields, with these exceptions:
                    * One can have "payload_content" value 'MULTIVAR_REAL' and
                      multiple payload fields and the other can have
                      "payload_content" value REAL and one payload field.
                    * When both have the "payload_content" value 'MULTIVAR_COMPLEX',
                      one can have multiple pairs of payload fields and
                      the other can have a single pair of payload fields.
            Types: TDSeries
        data1_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data1".
            Types: ColumnExpression
        data2:
            Required Argument.
            Specifies the actual filter kernel.
            The time series have the following TDSeries characteristics.
                1. "payload_content" must have one of these values:
                    * REAL
                    * COMPLEX
                    * MULTIVAR_REAL
                    * MULTIVAR_COMPLEX
                2. The two time series must have the same "payload_content" value
                   and number of payload fields, with these exceptions:
                    * One can have "payload_content" value 'MULTIVAR_REAL' and
                      multiple payload fields and the other can have
                      "payload_content" value REAL and one payload field.
                    * When both have the "payload_content" value 'MULTIVAR_COMPLEX',
                      one can have multiple pairs of payload fields and
                      the other can have a single pair of payload fields.
            Types: TDSeries
        data2_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data2".
            Types: ColumnExpression
        algorithm:
            Optional Argument.
            Specifies the options to use for convolving.
            By default, the function selects the best option based
            on the number of entries present in the two inputs,
            and their types ( REAL, COMPLEX, and so on.)
            CONV_SUMMATION only supports:
                * REAL, REAL
                * REAL, MULTIVAR_REAL
                * MULTIVAR_REAL, REAL
                * MULTIVAR_REAL, MULTIVAR_REAL
            Note:
                * This parameter is usually used for testing.
                  If this parameter is not included, the internal
                  planning logic selects the best option based on
                  the number of entries present in the two inputs,
                  and their types ( REAL, COMPLEX, and so on.)
            Permitted Values: CONV_SUMMATION, CONV_DFFT
            Types: str
        input_fmt_input_mode:
            Required Argument.
            Specifies the input mode supported by the function.
            Permitted Values: 
                1. ONE2ONE: Both the data1 and data2 series
                   specifications contain a series name which identifies
                   the two series in the function.
                2. MANY2ONE: The MANY specification is the data1 series
                   declaration. The data2 series specification contains
                   a series name that identifies the single data2 series.
                3. MATCH: Both series are defined by their respective
                   series specification instance name declarations.
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an 
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results. 
                    If not specified, a unique table name is internally 
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output 
                    table into. If not specified, table is created into 
                    database specified by the user at the time of context 
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of Convolve.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as Convolve_obj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Load the example data.
        load_example_data("uaf", ["xconvolve_complex_leftmulti", "hconvolve_complex_rightmulti",
                                  "xconvolve_complex_left", "hconvolve_complex_right"])
        # Create teradataml DataFrame objects.
        data1 = DataFrame.from_table("xconvolve_complex_leftmulti")
        data2 = DataFrame.from_table("hconvolve_complex_rightmulti")
        data3 = DataFrame.from_table("xconvolve_complex_left")
        data4 = DataFrame.from_table("hconvolve_complex_right")
        # Example 1: Execute Convolve() function to convolve two series into new time series using "CONV_DFFT" 
        #            algorithm. 
        #            Note:
        #               The two input time series "payload_content" are 'MULTIVAR_COMPLEX' and the resultant
        #               time series "payload_content" is 'MULTIVAR_COMPLEX'.
        # Create teradataml TDSeries objects for "payload_content" with the value 'MULTIVAR_COMPLEX'.
        data1_series_df = TDSeries(data=data1,
                                   id="id",
                                   row_index_style="sequence",
                                   row_index="seq",
                                   payload_field=["a_real", "a_imag", "b_real", "b_imag", "c_real", "c_imag"],
                                   payload_content="MULTIVAR_COMPLEX")
        data2_series_df = TDSeries(data=data2,
                                   id="id",
                                   row_index_style="sequence",
                                   row_index="seq",
                                   payload_field=["a_real", "a_imag", "b_real", "b_imag", "c_real", "c_imag"],
                                   payload_content="MULTIVAR_COMPLEX")
        # Convolve the "data1_series_df" and "data2_series_df" series using the Convolve() function.
        uaf_out1 = Convolve(data1=data1_series_df,
                            data2=data2_series_df,
                            algorithm="CONV_DFFT",
                            input_fmt_input_mode="MATCH")
        # Print the result DataFrame.
        print(uaf_out1.result)
        # Example 2: Execute Convolve() function to convolve two series into new time series using "CONV_SUMMATION" 
        #            algorithm.
        #            Note:
        #               The two input time series "payload_content" are 'MULTIVAR_REAL' and the resultant
        #               time series "payload_content" is 'MULTIVAR_COMPLEX'.
        # Create teradataml TDSeries objects for "payload_content" with the value 'MULTIVAR_REAL'.
        data3_series_df = TDSeries(data=data3,
                                   id="id",
                                   row_index_style="sequence",
                                   row_index="seq",
                                   payload_field=["a_real", "a_imag"],
                                   payload_content="MULTIVAR_REAL")
        data4_series_df = TDSeries(data=data4,
                                   id="id",
                                   row_index_style="sequence",
                                   row_index="seq",
                                   payload_field=["a_real", "a_imag"],
                                   payload_content="MULTIVAR_REAL")
        # Convolve the "data3_series_df" and "data4_series_df" series using the Convolve() function.
        uaf_out2 = Convolve(data1=data3_series_df,
                            data2=data4_series_df,
                            algorithm="CONV_SUMMATION",
                            input_fmt_input_mode="MATCH")
        # Print the result DataFrame.
        print(uaf_out2.result)
    Convolve2
    Convolve2
    Functions
    Convolve2(data1=None, data1_ﬁlter_expr=None, data2=None, data2_ﬁlter_expr=None, input_fmt_input_mode=None, **generic_arguments)
    DESCRIPTION:
        The Convolve2() function uses two matrices as input. One matrix is
        the image in pixels, and the other matrix is the filter.
        Smaller images with results sets smaller than 128 by 128
        use summation. Larger images use the Discrete Fast Fourier
        Transform (DFFT) method.
    PARAMETERS:
        data1:
            Required Argument.
            Specifies the matrix for the image to be filtered.
            The matrices have the following characteristics:
                1. The matrix can have any supported payload content as mentioned below:
                    * REAL
                    * COMPLEX
                    * MULTIVAR_REAL
                    * MULTIVAR_COMPLEX
                2. The following combinations are supported for
                   MULTIVAR varieties:
                    * For all MULTIVAR types: Both multivariate matrixes
                      are of the same content type, and both matrixes have 
                      the same number of payload fields.
                    * For MULTIVAR_REAL: One input is a MULTIVAR content matrix
                      having greater than one payload field, and the other matrix
                      is a REAL content series having just one payload field.
                    * For the MULTIVAR_COMPLEX: One input is a MULTIVAR content
                      matrix having greater than one pair of fields, and
                      the other matrix is a MULTIVAR content of the same 
                      type having just one pair of payload fields.
            Types: TDMatrix
        data1_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data1".
            Types: ColumnExpression
        data2:
            Required Argument.
            Specifies the actual filter kernel matrix for filtering image.
            The matrices have the following characteristics:
                1. The matrix can have any supported payload content as mentioned below:
                    * REAL
                    * COMPLEX
                    * MULTIVAR_REAL
                    * MULTIVAR_COMPLEX
                2. The following combinations are supported for
                   MULTIVAR varieties:
                    * For all MULTIVAR types: Both multivariate matrixes
                      are of the same content type, and both matrixes have 
                      the same number of payload fields.
                    * For MULTIVAR_REAL: One input is a MULTIVAR content matrix
                      having greater than one payload field, and the other matrix
                      is a REAL content series having just one payload field.
                    * For the MULTIVAR_COMPLEX: One input is a MULTIVAR content
                      matrix having greater than one pair of fields, and
                      the other matrix is a MULTIVAR content of the same 
                      type having just one pair of payload fields.
            Types: TDMatrix
        data2_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data2".
            Types: ColumnExpression
        input_fmt_input_mode:
            Required Argument.
            Specifies the input mode supported by the function.
            Permitted Values:
                1. ONE2ONE: Both the primary and secondary matrix
                   specifications contain a matrix name which identifies
                   the two matrixes in the function.
                2. MANY2ONE: The MANY specification is the primary matrix
                   declaration. The secondary matrix specification contains
                   a matrix name that identifies the single secondary matrix.
                3. MATCH: Both matrixes are defined by their respective
                   matrix specification MATRIX_ID declarations.
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an 
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results. 
                    If not specified, a unique table name is internally 
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output 
                    table into. If not specified, table is created into 
                    database specified by the user at the time of context 
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of Convolve2.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as Convolve2_obj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Load the example data.
        load_example_data("uaf", ["Convolve2ValidLeft", "Convolve2RealsLeft"])
        # Create teradataml DataFrame objects.
        data1 = DataFrame.from_table("Convolve2ValidLeft")
        data2 = DataFrame.from_table("Convolve2ValidLeft")
        data3 = DataFrame.from_table("Convolve2RealsLeft")
        data4 = DataFrame.from_table("Convolve2RealsLeft")
        # Example 1: Apply the Convolve2() function when payload fields of two matrices
        #            are the different to convolve two matrices into a new source 
        #            image matrix.
        # Create teradataml TDMatrix objects with different payload fields.
        data1_matrix_df = TDMatrix(data=data1,
                                   id='id',
                                   row_index_style="sequence",
                                   row_index='row_i',
                                   column_index_style="sequence",
                                   column_index='column_i',
                                   payload_field=["B"],
                                   payload_content="REAL")
        data2_matrix_df = TDMatrix(data=data2,
                                   id='id',
                                   row_index_style="sequence",
                                   row_index='row_i',
                                   column_index_style="sequence",
                                   column_index='column_i',
                                   payload_field=["A"],
                                   payload_content="REAL")
        # Convolve the "data1_matrix_df" and "data2_matrix_df" matrices using the Convolve2() function.
        uaf_out1 = Convolve2(data1=data1_matrix_df,
                             data2=data2_matrix_df,
                             input_fmt_input_mode="MATCH")
        # Print the result DataFrame.
        print(uaf_out1.result)
        # Example 2: Apply the Convolve2() function when payload fields of two matrices
        #            are the same to convolve two matrices into a new source image matrix 
        # Create teradataml TDMatrix objects with same payload fields.
        data3_matrix_df = TDMatrix(data=data3,
                                   id='id',
                                   row_index_style="sequence",
                                   row_index='row_seq',
                                   column_index_style="sequence",
                                   column_index='col_seq',
                                   payload_field=["A"],
                                   payload_content="REAL")
        data4_matrix_df = TDMatrix(data=data4,
                                   id='id',
                                   row_index_style="sequence",
                                   row_index='row_seq',
                                   column_index_style="sequence",
                                   column_index='col_seq',
                                   payload_field=["A"],
                                   payload_content="REAL")
        # Convolve the "data3_matrix_df" and "data4_matrix_df" matrices using the Convolve2() function.
        uaf_out2 = Convolve2(data1=data3_matrix_df,
                             data2=data4_matrix_df,
                             input_fmt_input_mode="MATCH")
        # Print the result DataFrame.
        print(uaf_out2.result)
    DFFT
    DFFT
    Functions
    DFFT(data=None, data_ﬁlter_expr=None, zero_padding_ok=True, freq_style='K_INTEGRAL', hertz_sample_rate=None, algorithm=None, human_readable=True,
    output_fmt_content=None, **generic_arguments)
    DESCRIPTION:
        The DFFT() function takes a time or space series as an input, and produces
        a result series containing the computed Fourier Transform Coefficients.
        The computed coefficients can be output in either
        rectangular (real, imaginary) or polar (amplitude, phase) forms.
        The following procedure is an example of how to use DFFT() when convolving
        two series with digital signal processing:
            1. Use DFFT() on series 1 and series 2 to get dataframes
               named 'dfftRes1' and 'dfftRes2', respectively.
            2. Use BinarySeriesOp() to do point-wise multiplication
               using 'dfftRes1' and 'dfftRes2'.
            3. Use IDFFT() on the output of BinarySeriesOp() to get the
               convolved result of the two series.
    PARAMETERS:
        data:
            Required Argument.
            Specifies a logical runtime series or the output of IDFFT in ART Spec.
            Types: TDSeries, TDAnalyticResult
        data_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data".
            Types: ColumnExpression
        zero_padding_ok:
            Optional Argument.
            Specifies whether to pad zeros to the end of a given Series to
            achieve a more efficient computation of the FFT coefficients.
            When set to True, series is padded, otherwise not.
            Default Value: True
            Types: bool
        freq_style:
            Optional Argument.
            Specifies format/values associated with the x-axis of the output.
            Permitted Values:
                * K_INTEGRAL: Integer representation.
                * K_SAMPLE_RATE: Integer normalized to number entries,
                                 with ranges from -0.5 to +0.5.
                * K_RADIANS: Radian ranges from -π to +π.
                * K_HERTZ: Frequency in hertz. Must be used in conjunction
                           with HERTZ_SAMPLE_RATE.
            Default Value: K_INTEGRAL
            Types: str
        hertz_sample_rate:
            Optional Argument.
            Specifies a floating point constant representing the sample rate
            in hertz. A value of 10000.0 means that the sample points were
            obtained by sampling at a rate of 10,000 hertz.
            Note:
                Applicable only when "freq_style" is set to 'K_HERTZ'.
            Types: int OR float
        algorithm:
            Optional Argument.
            Specifies the algorithm to be used for the primary DFFT
            calculation. If the argument is not provided, the
            internal DFFT planner selects the most efficient
            algorithm for the operation.
            Note:
                For best performance, do not include this parameter.
                Instead, let the internal DFFT planner select the best
                algorithm.
            Permitted Values: COOLEY_TUKEY, SINGLETON
            Types: str
        human_readable:
            Optional Argument.
            Specifies whether the input rows are in human-readable / plottable form,
            or if they are output in the raw-form. Human-readable
            output is symmetric around 0, such as -3, -2, -1, 0, 1, 2, 3.
            Raw output is sequential, starting at zero, such as 0, 1, 2, 3.
            When set to True the output is in human-readable form,
            otherwise the output is in raw form.
            Default Value: True
            Types: bool
        output_fmt_content:
            Optional Argument.
            Specifies the Fourier coefficient's output form.
            The default value is dependent on the datatype
            of the input series:
              * A single var input generates COMPLEX output content by default.
              * A multi var input generates MULTIVAR_COMPLEX output content by default.
            Note:
                1. Users can use COMPLEX or MULTIVAR_COMPLEX to
                   request the Fourier coefficients in rectangular form.
                2. AMPL_PHASE_RADIANS, AMPL_PHASE_DEGREES, AMPL_PHASE,
                   MULTIVAR_AMPL_PHASE_RADIANS, MULTIVAR_AMPL_PHASE or
                   MULTIVAR_AMPL_PHASE_DEGREES can be used to output
                   the Fourier coefficients in the polar form and to further
                   request the phase to be output in
                   radians or degrees.
                3. AMPL_PHASE is one of the permitted
                   values, it is synonymous with AMPL_PHASE_RADIANS.
                4. MULTIVAR_AMPL_PHASE is equivalent to
                   MULTIVAR_AMPL_PHASE_RADIANS.
            Permitted Values: COMPLEX,
                              AMPL_PHASE_RADIANS,
                              AMPL_PHASE_DEGREES,
                              AMPL_PHASE,
                              MULTIVAR_COMPLEX,
                              MULTIVAR_AMPL_PHASE_RADIANS,
                              MULTIVAR_AMPL_PHASE_DEGREES,
                              MULTIVAR_AMPL_PHASE
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results.
                    If not specified, a unique table name is internally
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output
                    table into. If not specified, table is created into
                    database specified by the user at the time of context
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of DFFT.
        Output teradataml DataFrames can be accessed using attribute
        references, such as DFFT_obj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Load the example data.
        load_example_data("uaf", "mvdfft8")
        # Create teradataml DataFrame object.
        data = DataFrame.from_table("mvdfft8")
        # Create teradataml TDSeries object which is an input to DFFT function.
        data_series_df = TDSeries(data=data,
                                  id="sid",
                                  row_index="n_seqno",
                                  row_index_style= "SEQUENCE",
                                  payload_field="magnitude1",
                                  payload_content="REAL")
        # Example 1: Compute the complex Fourier Transform Coefficients
        #            using a sequential series.
        uaf_out = DFFT(data=data_series_df,
                       human_readable=True,
                       output_fmt_content='COMPLEX')
        # Print the result DataFrame.
        print(uaf_out.result)
    DFFT2
    DFFT2
    Functions
    DFFT2(data=None, data_ﬁlter_expr=None, zero_padding_ok=True, freq_style='K_INTEGRAL', hertz_sample_rate=None, algorithm=None, human_readable=True,
    output_fmt_content=None, output_fmt_row_major=True, **generic_arguments)
    DESCRIPTION:
        The DFFT2() function takes a matrix (two-dimensional array) as an input,
        and returns a result matrix whose elements are the computed two-dimension
        Fourier Coefficients for the input matrix.
        The coefficients can be output as complex numbers in either
        rectangular (real, imaginary) or polar (amplitude, phase) form.
    PARAMETERS:
        data:
            Required Argument.
            Specifies a logical matrix or the output of IDFFT2 in ART Spec.
            Elements of the matrix can be either real or complex numbers.
            Types: TDMatrix, TDAnalyticResult
        data_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data".
            Types: ColumnExpression
        zero_padding_ok:
            Optional Argument.
            Specifies whether to pad zeros to the end of both the 'row-series'
            and 'column-series' to achieve a more efficient computation of
            the FFT coefficients.
            When set to True, row-series and column-series are padded,
            otherwise they are not.
            Default Value: True
            Types: bool
        freq_style:
            Optional Argument.
            Specifies format/values associated with the x-axis and y-axis
            of the output.
            Permitted Values:
                * K_INTEGRAL: Integer representation.
                * K_SAMPLE_RATE: Integer normalized to number entries,
                                 with ranges from -0.5 to +0.5.
                * K_RADIANS: Radian ranges from -π to +π.
                * K_HERTZ: Frequency in hertz. Must be used in conjunction
                           with HERTZ_SAMPLE_RATE.
            Default Value: K_INTEGRAL
            Types: str
        hertz_sample_rate:
            Optional Argument.
            Specifies a floating point constant representing the sample rate
            in hertz. A value of 10000.0 means that the sample points were
            obtained by sampling at a rate of 10,000 hertz. The hertz
            interpretation will be applied to both the row index and
            column index.
            Note:
                Applicable only when "freq_style" is set to 'K_HERTZ'.
            Types: int OR float
        algorithm:
            Optional Argument.
            Specifies the algorithm to be used for the primary DFFT
            calculation. If the argument is not provided, the
            internal DFFT planner selects the most efficient
            algorithm for the operation.
            Note:
                For best performance, do not include this parameter.
                Instead, let the internal DFFT planner select the best
                algorithm.
            Permitted Values: COOLEY_TUKEY, SINGLETON
            Types: str
        human_readable:
            Optional Argument.
            Specifies whether the input rows are in human-readable / plottable form,
            or if they are output in the raw-form. Human-readable
            output is symmetric around 0, such as -3, -2, -1, 0, 1, 2, 3.
            Raw output is sequential, starting at zero, such as 0, 1, 2, 3.
            When set to True the output is in human-readable form,
            otherwise the output is in raw form.
            Default Value: True
            Types: bool
        output_fmt_content:
            Optional Argument.
            Specifies the Fourier coefficient's output form.
                The default value is dependent on the datatype
                of the input series:
                  * A single var input generates COMPLEX output content by default.
                  * A multi var input generates MULTIVAR_COMPLEX output content by default.
                Note:
                    1. Users can use COMPLEX or MULTIVAR_COMPLEX to
                       request the Fourier coefficients in rectangular form.
                    2. AMPL_PHASE_RADIANS, AMPL_PHASE_DEGREES, AMPL_PHASE,
                       MULTIVAR_AMPL_PHASE_RADIANS, MULTIVAR_AMPL_PHASE or
                       MULTIVAR_AMPL_PHASE_DEGREES can be used to output
                       the Fourier coefficients in the polar form and to further
                       request the phase to be output in
                       radians or degrees.
                    3. AMPL_PHASE is one of the permitted
                       values, it is synonymous with AMPL_PHASE_RADIANS.
                    4. MULTIVAR_AMPL_PHASE is equivalent to
                       MULTIVAR_AMPL_PHASE_RADIANS.
                Permitted Values: COMPLEX,
                                  AMPL_PHASE_RADIANS,
                                  AMPL_PHASE_DEGREES,
                                  AMPL_PHASE,
                                  MULTIVAR_COMPLEX,
                                  MULTIVAR_AMPL_PHASE_RADIANS,
                                  MULTIVAR_AMPL_PHASE_DEGREES,
                                  MULTIVAR_AMPL_PHASE
                Types: str
        output_fmt_row_major:
            Optional Argument.
            Species whether the matrix output should be in a row-major-centric
            or column-major-centric manner.
            When set to True, the output is in row-major-centric manner,
            otherwise the output is in column-major-centric manner.
            Default Value: True
            Types: bool
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an 
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results. 
                    If not specified, a unique table name is internally 
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output 
                    table into. If not specified, table is created into 
                    database specified by the user at the time of context 
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of DFFT2.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as DFFT2_obj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Load the example data.
        load_example_data("uaf", ["dfft2_size4_real"])
        # Create teradataml DataFrame object.
        data = DataFrame.from_table("dfft2_size4_real")
        # Create teradataml TDMatrix object.
        td_matrix = TDMatrix(data=data,
                             id="buoy_id",
                             row_index="row_i",
                             row_index_style="SEQUENCE",
                             column_index="column_i",
                             column_index_style="SEQUENCE",
                             payload_field="magnitude",
                             payload_content="REAL")
        # Example 1 : Compute the two-dimension fourier transform using the
        #             input matrix with real numbers only for the matrix id 33.
        filter_expr = td_matrix.buoy_id==3
        uaf_out = DFFT2(data=td_matrix,
                        data_filter_expr=filter_expr,
                        freq_style="K_INTEGRAL",
                        human_readable=False,
                        output_fmt_content="COMPLEX")
        # Print the result DataFrame.
        print(uaf_out.result)
    DFFT2Conv
    DFFT2Conv
    Functions
    DFFT2Conv(data=None, data_ﬁlter_expr=None, conv=None, freq_style='K_INTEGRAL', hertz_sample_rate=None, output_fmt_content=None,
    **generic_arguments)
    DESCRIPTION:
        The DFFT2Conv() function is a Fast Fourier Transform converter that
        converts output generated by DFFT2(). DFFT2() results are in
        human-readable and raw form. DFFT2Conv() can also output different payload types
        such as AMPL_PHASE_DEGREES, MULTIVAR_AMPL_PHASE_RADIANS, COMPLEX. However,
        user may want a different result form in order to pass the results to
        another UAF function, plot the results in human readable form, compare the
        results to Python, and so on. Instead of rerunning DFFT2() to produce a
        different result set, user can use DFFT2Conv() to accomplish the task faster and
        with less memory consumption.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input matrix or a TDAnalyticResult object
            created over the output generated by the UAF function.
            Types: TDMatrix, TDAnalyticResult
        data_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data".
            Types: ColumnExpression
        conv:
            Required Argument.
            Specifies the type of conversion to be performed by the
            function.
            Permitted Values:
                * HR_TO_RAW: Convert from human readable to raw form.
                * RAW_TO_HR: Convert from raw form to human readable form.
            Types: str
        freq_style:
            Optional Argument.
            Specifies the format or values associated with the x-axis of
            the output.
            Permitted Values:
                * K_INTEGRAL: Integer representation.
                * K_SAMPLE_RATE: Integer normalized to number entries, ranging from -0.5 to +0.5.
                * K_RADIANS: Radian ranges from -π to +π.
                * K_HERTZ: Frequency in hertz. Must be used in conjunction with "hertz_sample_rate".
            Default Value: K_INTEGRAL
            Types: str
        hertz_sample_rate:
            Optional Argument.
            Specifies the sample rate as a floating point constant,
            in hertz. A value of 10000.0 indicates that the sample points
            were obtained by sampling at a rate of 10,000 hertz.
            Note:
                * Applicable only when "freq_style" is set to 'K_HERTZ'.
            Types: int OR float
        output_fmt_content:
            Optional Argument.
            Specifies a complex number that can be in rectangular (real, imaginary)
            or polar (amplitude, phase) form. The default is rectangular (real,
            imaginary). The output options are as follows:
                * COMPLEX
                * AMPL_PHASE_DEGREES
                * AMPL_PHASE_RADIANS
                * AMPL_PHASE
            The MULTIVAR options are below. The default is MULTIVAR_COMPLEX:
                * MULTIVAR_COMPLEX
                * MULTIVAR_AMPL_PHASE_DEGREES
                * MULTIVAR_AMPL_PHASE_RADIANS
                * MULTIVAR_AMPL_PHASE
            Valid Conversion:
                COMPLEX -> COMPLEX
                COMPLEX -> AMPL_PHASE
                AMPL_PHASE -> AMPL_PHASE
                AMPL_PHASE -> COMPLEX
                MULTIVAR_COMPLEX -> MULTIVAR_COMPLEX
                MULTIVAR_COMPLEX -> MULTVAR_AMPL_PHASE
                MULTIVAR_AMPL_PHASE -> MULTIVAR_AMPL_PHASE
                MULTIVAR_AMPL_PHASE -> MULTIVAR_COMPLEX
            Rest of conversions are invalid.
            Permitted Values: COMPLEX,
                              AMPL_PHASE_RADIANS,
                              AMPL_PHASE_DEGREES,
                              AMPL_PHASE,
                              MULTIVAR_COMPLEX, 
                              MULTIVAR_AMPL_PHASE_RADIANS, 
                              MULTIVAR_AMPL_PHASE_DEGREES, 
                              MULTIVAR_AMPL_PHASE
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an 
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results. 
                    If not specified, a unique table name is internally 
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output 
                    table into. If not specified, table is created into 
                    database specified by the user at the time of context 
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of DFFT2Conv.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as DFFT2Conv_obj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Load the example data.
        load_example_data("uaf", ["dfft2conv_real_4_4"])
        # Create teradataml DataFrame object.
        data = DataFrame.from_table("dfft2conv_real_4_4")
        # Create teradataml TDMatrix object.
        td_matrix = TDMatrix(data=data,
                             id="id",
                             row_index="row_i",
                             row_index_style="SEQUENCE",
                             column_index="column_i",
                             column_index_style="SEQUENCE",
                             payload_field="magnitude",
                             payload_content="REAL")
        # Compute the two-dimension fourier transform using the
        # input matrix with real numbers only for the matrix id 33.
        filter_expr = td_matrix.id==33
        dfft2_out = DFFT2(data=td_matrix,
                          data_filter_expr=filter_expr,
                          freq_style="K_INTEGRAL",
                          human_readable=False,
                          output_fmt_content="COMPLEX")
        # Example 1: Convert the complex(REAL,IMAGINARY) output of DFFT2() to
        #            polar(AMPLITUDE,PHASE) in RADIAN format using TDMatrix
        #            as input data.
        # Create teradataml TDMatrix object.
        matrix_spec = TDMatrix(data=dfft2_out.result,
                               id="id",
                               row_index="ROW_I",
                               column_index="COLUMN_I",
                               row_index_style="SEQUENCE",
                               column_index_style="SEQUENCE",
                               payload_field=["REAL_magnitude", "IMAG_magnitude"],
                               payload_content="COMPLEX")
        uaf_out1 = DFFT2Conv(data=matrix_spec,
                             conv="HR_TO_RAW",
                             output_fmt_content="AMPL_PHASE_RADIANS")
        # Print the result DataFrame.
        print(uaf_out1.result)
         # Example 2: Convert the complex(REAL,IMAGINARY) output of DFFT2() to
         #            polar(AMPLITUDE,PHASE) in RADIAN format using TDAnalyticResult
         #            as input data.
        # Create teradataml TDAnalyticResult object.
        art_spec = TDAnalyticResult(data=dfft2_out.result)
        uaf_out2 = DFFT2Conv(data=art_spec,
                             conv="HR_TO_RAW",
                             output_fmt_content="AMPL_PHASE_RADIANS")
        # Print the result DataFrame.
        print(uaf_out2.result)
    DFFTConv
    DFFTConv
    Functions
    DFFTConv(data=None, data_ﬁlter_expr=None, conv=None, freq_style='K_INTEGRAL', hertz_sample_rate=None, output_fmt_content=None,
    **generic_arguments)
    DESCRIPTION:
        The DFFTConv() function is a Fast Fourier Transform converter
        that converts output generated by DFFT(). DFFT() results are
        in human-readable and raw form. DFFTConv() can also output different
        payload types such as AMPL_PHASE_DEGREES, MULTIVAR_AMPL_PHASE_RADIANS,
        COMPLEX. However, user may want a different result form in order
        to pass the results to another UAF function, plot the results in human
        readable form, compare the results to Python, and so on. Instead of
        rerunning DFFT() to produce a different result set, user can use DFFTConv()
        to accomplish the task faster and with less memory consumption.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input series or a TDAnalyticResult object created over the
            output generated by the UAF function.
            Types: TDSeries, TDAnalyticResult
        data_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data".
            Types: ColumnExpression
        conv:
            Required Argument.
            Specifies the type of conversion to be performed by the
            function.
            Permitted Values:
                * HR_TO_RAW: Convert from human readable to raw form.
                * RAW_TO_HR: Convert from raw form to human readable form.
            Types: str
        freq_style:
            Optional Argument.
            Specifies the format or values associated with the x-axis of
            the output.
            Permitted Values:
                * K_INTEGRAL: Integer representation.
                * K_SAMPLE_RATE: Integer normalized to number entries, ranging from -0.5 to +0.5.
                * K_RADIANS: Radian ranges from -π to +π.
                * K_HERTZ: Frequency in hertz. Must be used in conjunction with "hertz_sample_rate".
            Default Value: K_INTEGRAL
            Types: str
        hertz_sample_rate:
            Optional Argument.
            Specifies the sample rate as a floating point constant,
            in hertz. A value of 10000.0 indicates that the sample points
            were obtained by sampling at a rate of 10,000 hertz.
            Value should be greater than 0.
            Notes:
                * Applicable only when "freq_style" is set to 'K_HERTZ'.
            Types: int OR float
        output_fmt_content:
            Optional Argument.
            Specifies a complex number that can be in rectangular (real, imaginary)
            or polar (amplitude, phase) form. The default is rectangular (real,
            imaginary). The output options are as follows:
                * COMPLEX
                * AMPL_PHASE_DEGREES
                * AMPL_PHASE_RADIANS
                * AMPL_PHASE
            The MULTIVAR options are below. The default is MULTIVAR_COMPLEX:
                * MULTIVAR_COMPLEX
                * MULTIVAR_AMPL_PHASE_DEGREES
                * MULTIVAR_AMPL_PHASE_RADIANS
                * MULTIVAR_AMPL_PHASE
            Valid Conversion:
                COMPLEX -> COMPLEX
                COMPLEX -> AMPL_PHASE
                AMPL_PHASE -> AMPL_PHASE
                AMPL_PHASE -> COMPLEX
                MULTIVAR_COMPLEX -> MULTIVAR_COMPLEX
                MULTIVAR_COMPLEX -> MULTVAR_AMPL_PHASE
                MULTIVAR_AMPL_PHASE -> MULTIVAR_AMPL_PHASE
                MULTIVAR_AMPL_PHASE -> MULTIVAR_COMPLEX
            Rest of conversions are invalid.
            Permitted Values: COMPLEX,
                              AMPL_PHASE_RADIANS,
                              AMPL_PHASE_DEGREES,
                              AMPL_PHASE,
                              MULTIVAR_COMPLEX,
                              MULTIVAR_AMPL_PHASE_RADIANS,
                              MULTIVAR_AMPL_PHASE_DEGREES,
                              MULTIVAR_AMPL_PHASE
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an 
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results. 
                    If not specified, a unique table name is internally 
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output 
                    table into. If not specified, table is created into 
                    database specified by the user at the time of context 
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of DFFTConv.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as DFFTConv_obj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Load the example data.
        load_example_data("uaf", ["DFFTConv_Real_8_8"])
        # Create teradataml DataFrame object.
        df = DataFrame("DFFTConv_Real_8_8")
        # Create teradataml TDSeries object.
        td_series = TDSeries(data=df,
                             id="ID",
                             row_index="ROW_I",
                             row_index_style="SEQUENCE",
                             payload_field="MAGNITUDE",
                             payload_content="REAL")
        # Compute the complex Fourier Transform Coefficients
        # using a sequential series.
        dfft = DFFT(data=td_series,
                    freq_style="K_INTEGRAL",
                    human_readable=False,
                    output_fmt_content="COMPLEX")
        # Create teradataml TDAnalyticResult object.
        art_spec = TDAnalyticResult(data=dfft.result)
        # Example 1 : Convert the complex(REAL,IMAGINARY) output of DFFT() to
        #             polar(AMPLITUDE,PHASE) in RADIAN format.
        dfft_conv = DFFTConv(data=art_spec,
                             conv="RAW_TO_HR",
                             output_fmt_content="AMPL_PHASE_RADIANS")
        # Print the result DataFrame.
        print(dfft_conv.result)
    GenseriesSinusoids
    GenseriesSinusoids
    Functions
    GenseriesSinusoids(data=None, data_ﬁlter_expr=None, periodicities=None, output_fmt_index_style='NUMERICAL_SEQUENCE', **generic_arguments)
    DESCRIPTION:        
        The GenseriesSinusoids() function generates a result series
        containing a subset of the sinusoidal elements periodicities (sinusoids).
        User can subtract the new time series from the original time series
        by removing the periodicities. The following procedure is an
        example of how to use GenseriesSinusoids() function:
            * Use the LineSpec() or PowerSpec() function with "freq_style" argument
              set to 'K_PERIODICITY' to determine the periodicities in
              the series.
            * Use the result dataframe from the GenseriesSinusoids() function
              to view the periodicities.
            * Use GenseriesSinusoids() function with the "periodicities"
              argument and a comma-separated list of periodicities to
              exclude from the data set.
            * Use the BinarySeriesOp() function to subtract the generated series
              from the original series using "mathop" argument value as 'SUB'.
            * Use the PowerSpec() function to verify that target periodicities
              have been removed from the original series.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input time series, whose payload content
            value is 'REAL'.
            Types: TDSeries
        data_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data".
            Types: ColumnExpression
        periodicities:
            Required Argument.
            Specifies the periodicity as a comma-separated list, which
            contains one or more floating point values representing 
            periodicities.
            Types: int, list of int, float, list of float
        output_fmt_index_style:
            Optional Argument.
            Specifies the index style of the output format.
            Permitted Values: NUMERICAL_SEQUENCE, FLOW_THROUGH
            Default Value: NUMERICAL_SEQUENCE
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an 
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results. 
                    If not specified, a unique table name is internally 
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output 
                    table into. If not specified, table is created into 
                    database specified by the user at the time of context 
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of GenseriesSinusoids.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as GenseriesSinusoids_obj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Load the example data.
        load_example_data("uaf", ["production_data"])
        # Create teradataml DataFrame object.
        data = DataFrame.from_table("production_data")
        # Example 1: Execute the GenseriesSinusoids() function 
        #            on TDSeries input to generate a time series 
        #            containing a subset of the sinusoidal 
        #            elements periodicities, whose payload content
        #            value is REAL.
        # Create teradataml TDSeries object.
        data_series_df = TDSeries(data=data,
                                  id="product_id",
                                  row_index="TD_TIMECODE",
                                  payload_field="beer_sales",
                                  payload_content="REAL")
        # Execute GenseriesSinusoids() fucntion.
        uaf_out = GenseriesSinusoids(data=data_series_df,
                                     periodicities=[0.523, 1.4367])
        # Print the result DataFrame.
        print(uaf_out.result)
    IDFFT
    IDFFT
    Functions
    IDFFT(data=None, data_ﬁlter_expr=None, human_readable=True, **generic_arguments)
    DESCRIPTION:
        The IDFFT() function reverses the effects of the forward
        transform (DFFT() function). It takes a series containing Fourier
        Coefficients as an input, and returns the original series that
        was input into the DFFT() function to generate the coefficients.
        The Fourier Coefficients can be in either the rectangular
        (real, imaginary) or polar (amplitude, phase) formats.
        The following procedure is an example of how to use IDFFT() when
        convolving two series with digital signal processing:
            1. Use DFFT() on series 1 and series 2 to get dataframes named
               'dfftRes1' and 'dfftRes2', respectively.
            2. Use BinarySeriesOp() to do point-wise multiplication using
               'dfftRes1' and 'dfftRes2'.
            3. Use IDFFT() on the output of BinarySeriesOp() function to get
               the convolved result of the two series.
    PARAMETERS:
        data:
            Required Argument.
            Specifies a logical-runtime series, that is,
            'a 1D array' as its input, that has been populated
            previously with Fourier Transform coefficients.
            The calculated coefficients may exist
            in either of the following forms:
                1. complex numbers - real and imaginary pairs
                2. amplitude-phase pairs
            Types: TDSeries, TDAnalyticResult
        data_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data".
            Types: ColumnExpression
        human_readable:
            Optional Argument.
            Specifies whether the input rows are in human-readable / plottable form,
            or if they are output in the raw-form. Human-readable
            output is symmetric around 0, such as -3, -2, -1, 0, 1, 2, 3.
            Raw output is sequential, starting at zero, such as 0, 1, 2, 3.
            When set to True the output is in human-readable form,
            otherwise the output is in raw form.
            Default Value: True
            Types: bool
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an 
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results. 
                    If not specified, a unique table name is internally 
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output 
                    table into. If not specified, table is created into 
                    database specified by the user at the time of context 
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of IDFFT.
        Output teradataml DataFrames can be accessed using attribute
        references, such as IDFFT_obj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Load the example data.
        load_example_data("uaf", "mvdfft8")
        # Compute the complex Fourier Transform Coefficients using a
        # sequential series.
        # Create teradataml DataFrame object.
        data = DataFrame.from_table("mvdfft8")
        # Create teradataml TDSeries object which is an input to DFFT function.
        data_series_df = TDSeries(data=data,
                                  id="sid",
                                  row_index="n_seqno",
                                  row_index_style="SEQUENCE",
                                  payload_field="magnitude1",
                                  payload_content="REAL")
        # Execute DFFT function.
        DFFT_result = DFFT(data=data_series_df,
                           human_readable=True,
                           output_fmt_content='COMPLEX')
        # Example 1: Compute the inverse fourier transform using TDAnalyticResult as input.
        # Create teradataml TDAnalyticResult object to be passed as an
        # input to IDFFT.
        idfft_art_spec = TDAnalyticResult(data=DFFT_result.result,
                                          payload_content="COMPLEX",
                                          payload_field=["REAL_MAGNITUDE1",
                                                         "IMAG_MAGNITUDE1"])
        # Execute IDFFT function.
        uaf_out = IDFFT(data=idfft_art_spec, human_readable=True)
        # Print the result DataFrame.
        print(uaf_out.result)
        # Example 2: Compute the inverse fourier transform using TDSeries as input.
        # Create a teradataml TDSeries object.
        idfft_series_spec = TDSeries(data=DFFT_result.result,
                                     id="sid",
                                     row_index="sid",
                                     row_index_style="SEQUENCE",
                                     payload_content="COMPLEX",
                                     payload_field=["REAL_MAGNITUDE1",
                                                    "IMAG_MAGNITUDE1"])
        # Execute IDFFT function.
        uaf_out = IDFFT(data=idfft_series_spec, human_readable=True)
        # Print the result DataFrame.
        print(uaf_out.result)
    IDFFT2
    IDFFT2
    Functions
    IDFFT2(data=None, data_ﬁlter_expr=None, human_readable=True, output_fmt_content=None, **generic_arguments)
    DESCRIPTION:
        The IDFFT2() function simply reverses the 2D Fourier Transform. It
        takes either a logical matrix containing
        Fourier coefficients in the form of complex number
        elements, or, alternatively, in the form of amplitude-phase
        pair (polar form) elements as inputs. The function then runs them
        through a reverse-transform summation formula, and outputs
        the original logical matrix (original 2D array) that was
        input into the DFFT2() to generate the Fourier
        coefficients.
    PARAMETERS:
        data:
            Required Argument.
            Specifies a logical matrix or TDAnalyticResult object on the data
            that has been populated previously with 2D Fourier Transform
            coefficients. The calculated coefficients may exist
            in either of the following forms:
                1. complex numbers - real and imaginary pairs.
                2. amplitude-phase pairs.
            Types: TDMatrix, TDAnalyticResult
        data_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data".
            Types: ColumnExpression
        human_readable:
            Optional Argument.
            Specifies whether the input rows are in human-readable / plottable form,
            or if they are output in the raw-form. Human-readable
            output is symmetric around 0, such as -3, -2, -1, 0, 1, 2, 3.
            Raw output is sequential, starting at zero, such as 0, 1, 2, 3.
            When set to True the output is in human-readable form,
            otherwise the output is in raw form.
            Default Value: True
            Types: bool
        output_fmt_content:
            Optional Argument.
            Specifies the Fourier coefficient's output form.
            The default value is dependent on the datatype
            of the input series, a single var input
            generates COMPLEX output content by default,
            a multi var input generates
            MULTIVAR_COMPLEX output content by default.
            Note:
                1. Users can use COMPLEX or MULTIVAR_COMPLEX to
                   request the Fourier coefficients in rectangular form.
                2. AMPL_PHASE_RADIANS, AMPL_PHASE_DEGREES, AMPL_PHASE,
                   MULTIVAR_AMPL_PHASE_RADIANS, MULTIVAR_AMPL_PHASE or
                   MULTIVAR_AMPL_PHASE_DEGREES can be used to output
                   the Fourier coefficients in the polar form and to further
                   request the phase to be output in
                   radians or degrees.
                3. AMPL_PHASE is one of the permitted
                   values, it is synonymous with AMPL_PHASE_RADIANS.
                4. MULTIVAR_AMPL_PHASE is equivalent to
                   MULTIVAR_AMPL_PHASE_RADIANS.
            Permitted Values: COMPLEX,
                              AMPL_PHASE_RADIANS,
                              AMPL_PHASE_DEGREES,
                              AMPL_PHASE,
                              MULTIVAR_COMPLEX,
                              MULTIVAR_AMPL_PHASE_RADIANS,
                              MULTIVAR_AMPL_PHASE_DEGREES,
                              MULTIVAR_AMPL_PHASE
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results.
                    If not specified, a unique table name is internally
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output
                    table into. If not specified, table is created into
                    database specified by the user at the time of context
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of IDFFT2.
        Output teradataml DataFrames can be accessed using attribute
        references, such as IDFFT2_obj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Load the example data.
        load_example_data("uaf", ["dfft2conv_real_4_4"])
        # Compute the two-dimension fourier transform using the
        # input matrix with real numbers only for the matrix id 33.
        # Create teradataml DataFrame object.
        data = DataFrame.from_table("dfft2conv_real_4_4")
        # Create teradataml TDMatrix object.
        td_matrix = TDMatrix(data=data,
                             id="id",
                             row_index="row_i",
                             row_index_style="SEQUENCE",
                             column_index="column_i",
                             column_index_style="SEQUENCE",
                             payload_field="magnitude",
                             payload_content="REAL")
        filter_expr = td_matrix.id==33
        dfft2 = DFFT2(data=td_matrix,
                      data_filter_expr=filter_expr,
                      freq_style="K_INTEGRAL",
                      human_readable=False,
                      output_fmt_content="COMPLEX")
        # Example 1: Compute the inverse of two-dimension fourier transform using matrix
        #            with complex numbers.
        # Create teradataml TDMatrix object.
        data_matrix_df = TDMatrix(data=dfft2.result,
                                  id="id",
                                  row_index="ROW_I",
                                  row_index_style="SEQUENCE",
                                  column_index="COLUMN_I",
                                  column_index_style="SEQUENCE",
                                  payload_field=["REAL_magnitude", "IMAG_magnitude"],
                                  payload_content="COMPLEX")
        uaf_out = IDFFT2(data=data_matrix_df, human_readable=False)
        # Print the result DataFrame.
        print(uaf_out.result)
        # Example 2: Compute the inverse of two-dimension fourier transform using
        #            TDAnalyticResult instead of matrix with complex numbers.
        # Create teradataml TDAnalyticResult object.
        art_df = TDAnalyticResult(data=dfft2.result)
        uaf_out = IDFFT2(data=art_df, human_readable=False)
        # Print the result DataFrame.
        print(uaf_out.result)
    LineSpec
    LineSpec
    Functions
    LineSpec(data=None, data_ﬁlter_expr=None, freq_style='K_INTEGRAL', include_coeff=False, zero_padding_ok=True, hertz_sample_rate=None,
    **generic_arguments)
    DESCRIPTION:
        The LineSpec() function identifies periodicity that may be inherent in
        an input series.
        The following procedure is an example of how to use LineSpec:
            1. Use ArimaEstimate() to identify spectral candidates.
            2. Use ArimaValidate() to validate spectral candidates.
            3. Use LineSpec() with "freq_style" parameter set to K_PERIODICITY
               to perform spectral analysis.
            4. Use DataFrame.plot() to plot the results.
            5. Compute the test statistic.
            6. Use SignifPeriodicities() on the periodicities of interest.
               More than one periodicity can be entered using the "periodicities"
               parameter.
    PARAMETERS:
        data:
            Required Argument.
            Specifies an input time series whose payload content has one of the
            following values:
                * REAL
                * COMPLEX
                * MULTIVAR_REAL
                * MULTIVAR_COMPLEX
            Types: TDSeries
        data_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data".
            Types: ColumnExpression
        freq_style:
            Optional Argument.
            Specifies the format or values associated with the x-axis of the
            output.
            Permitted Values:
                * K_INTEGRAL: Integer representation.
                * K_SAMPLE_RATE: Integer normalized to number entries, with
                                  ranges from -0.5 to +0.5.
                * K_RADIANS: Radian ranges from -π to +π.
                * K_PERIODICITY: Periodicity.
            Default Value: K_INTEGRAL
            Types: str
        include_coeff:
            Optional Argument.
            Specifies whether to include the calculated αk and βk values
            in the output. The formula for the magnitude at time k in the
            line spectrum is:
                (n/2) * ((αk)**2 + (βk)**2)
            Default Value: False
            Types: bool
        zero_padding_ok:
            Optional Argument.
            Specifies whether to add zeros to the end of time series
            to make computation more efficient. When set to False, does
            not add zeros, otherwise zeros are added.
            Default Value: True
            Types: bool
        hertz_sample_rate:
            Optional Argument.
            Specifies the sample rate as a floating point constant, in hertz.
            A value of 10000.0 indicates that the sample points were obtained
            by sampling at a rate of 10,000 hertz. This hertz interpretation
            applies to both the ROW_I and COLUMN_I indices.
            Types: int OR float
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions. 
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an 
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results. 
                    If not specified, a unique table name is internally 
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output 
                    table into. If not specified, table is created into 
                    database specified by the user at the time of context 
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of LineSpec.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as LineSpec_obj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Load the example data.
        load_example_data("uaf", ["TestRiver"])
        # Create teradataml DataFrame object.
        df = DataFrame.from_table("TestRiver")
        # Create teradataml TDSeries object.
        result = TDSeries(data=df, id="BuoyID", row_index="N_SeqNo",
                          row_index_style="SEQUENCE", payload_field="MAGNITUDE",
                          payload_content="REAL")
        # Example 1 : Execute function to identify the periodicity of the input
        #             series.
        uaf_out = LineSpec(data=result)
        # Print the result DataFrame.
        print(uaf_out.result)
    PowerSpec
    PowerSpec
    Functions
    PowerSpec(data=None, data_ﬁlter_expr=None, freq_style=None, hertz_sample_rate=None, zero_padding_ok=True, algorithm=None,
    incrfourier_interval_length=None, incrfourier_spacing_interval=None, window_name=None, window_param=None, **generic_arguments)
    DESCRIPTION:
        The PowerSpec() function converts a series from the time or spatial
        domain to the frequency domain in order to facilitate frequency
        domain analysis. Its calculations serve to estimate the correct
        power spectrum associated with the series.
        The following procedure is an example of how to use POWERSPEC:
            * Use ArimaEstimate() to identify spectral candidates.
            * Use ArimaValidate() to validate spectral candidates.
            * Use PowerSpec() with "freq_style" argument set to 'K_PERIODICITY'
              to perform spectral analysis.
            * Use DataFrame.plot() to plot the results.
            * Compute the test statistic.
            * Use SignifPeriodicities() on the periodicities of interest.
              More than one periodicities can be entered using the Periodicities
              argument.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the time or spatial domain series.
            Types: TDSeries
        data_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data".
            Types: ColumnExpression
        freq_style:
            Optional Argument.
            Specifies the axis scale. Following are the description of styles:
                * FREQ_STYLE (FK) : It is 1/P
                * FREQ_STYLE (WK) : It is 2π/P
                * FREQ_STYLE (PERIODICITY) : It is N/k.
            Permitted Values: K_INTEGRAL, K_SAMPLE_RATE, K_RADIANS, 
                              K_HERTZ, K_PERIODICITY
            Types: str
        hertz_sample_rate:
            Optional Argument.
            Specifies floating point constant representing the sample
            rate, in hertz. A value of 10000.0 indicates that the sample
            points were obtained by sampling at a rate of 10,000 hertz.
            Note:
                * Only used with "freq_style" set to 'K_HERTZ'.
            Types: int OR float
        zero_padding_ok:
            Optional Argument.
            Specifies whether to add zeros to the end of a given
            series to achieve a more efficient computation of
            the FFT coefficients. When set to True, zeros are added, 
            otherwise not. 
            Default Value: True
            Types: bool
        algorithm:
            Optional Argument.
            Specifies algorithm to calculate the power spectrum.
            Permitted Values:
                * AUTOCOV : Use the Fourier Cosine of the Auto covariance
                            approach.
                * AUTOCORR : Use the Fourier Cosine of the Auto correlation
                             approach.
                * FOURIER : Use the Fourier Transform approach.
                * INCRFOURIER : Use the Incremental Fourier Transform
                                approach.
            Types: str
        incrfourier_interval_length:
            Optional Argument.
            Specifies interval sampling lengths (L).
            Note:
                * Only valid when "algorithm" is set to 'INCRFOURIER'.
            Types: int
        incrfourier_spacing_interval:
            Optional Argument.
            Specifies the spacing interval (K).
            Note:
                * Only valid when "algorithm" is set to 'INCRFOURIER'.
            Types: int
        window_name:
            Optional Argument.
            Specifies a smoothing window.
            Permitted Values:
                * NONE : Do not apply a smoothing window.
                         This translates into the application of
                         a square wave window, which has a magnitude of '1.0'
                         for the whole duration of the window.
                * TUKEY : Apply a Tukey smoothing window with the supplied
                          alpha value. Must be used with "window_param". 
                * BARTLETT : Apply a Bartlett smoothing window.
                * PARZEN : Apply a Parzen smoothing window.
                * WELCH : Apply a Welch smoothing window.
            Types: str
        window_param:
            Optional Argument.
            Specifies the sample rate, in hertz. Value of 10000.0 indicates
            the sample points were obtained by sampling at a rate of 10,000
            hertz.
            Note:
                * Use when "window_name" is set to 'TUKEY'.
            Types: int OR float
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an 
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results. 
                    If not specified, a unique table name is internally 
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output 
                    table into. If not specified, table is created into 
                    database specified by the user at the time of context 
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of PowerSpec.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as PowerSpec_obj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Load the example data.
        load_example_data("uaf", ["test_river2"])
        # Create teradataml DataFrame object.
        data = DataFrame.from_table("test_river2")
        # Create teradataml TDSeries object.
        data_series_df = TDSeries(data=data, id="buoy_id",
                                  row_index="n_seq_no",
                                  row_index_style="SEQUENCE",
                                  payload_field="magnitude",
                                  payload_content="REAL")
        # Example 1 : Converting a series from the time domain to the frequency
        #             domain using AUTOCORR algorithm and window name as Tukey.
        uaf_out = PowerSpec(data=data_series_df,
                            freq_style="K_SAMPLE_RATE",
                            algorithm="AUTOCORR",
                            window_name="TUKEY",
                            window_param=0.5)
        # Print the result DataFrame.
        print(uaf_out.result)
    DIGITAL SIGNAL PROCESSING functions
    DWT
    DWT
    Functions
    DWT(data1=None, data1_ﬁlter_expr=None, data2=None, data2_ﬁlter_expr=None, wavelet=None, mode='symmetric', level=1, part=None,
    input_fmt_input_mode=None, output_fmt_index_style='NUMERICAL_SEQUENCE', **generic_arguments)
    DESCRIPTION:
        DWT() is a function that performs discrete wavelet
        transform (DWT).
    PARAMETERS:
        data1:
            Required Argument.
            Specifies the series to be used as an input.
            Multiple payloads are supported, and each payload column is
            transformed independently. Only REAL or MULTIVAR_REAL
            payload content types are supported.
            Types: TDSeries
        data1_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data1".
            Types: ColumnExpression
        data2:
            Optional Argument.
            Specifies the series to be used as an input. The
            series specifies the filter. It should have two payload
            columns corresponding to low and high pass 
            filters. Only MULTIVAR_REAL payload content type is
            supported.
            Types: TDSeries
        data2_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data2".
            Types: ColumnExpression
        wavelet:
            Optional Argument.
            Specifies the name of the wavelet.
            Option families and names are:
                * Daubechies: 'db1' or 'haar', 'db2', 'db3', .... ,'db38'
                * Coiflets: 'coif1', 'coif2', ... , 'coif17'
                * Symlets: 'sym2', 'sym3', ... ,' sym20'
                * Discrete Meyer: 'dmey'
                * Biorthogonal: 'bior1.1', 'bior1.3', 'bior1.5',
                                'bior2.2', 'bior2.4', 'bior2.6',
                                'bior2.8', 'bior3.1', 'bior3.3',
                                'bior3.5', 'bior3.7', 'bior3.9',
                                'bior4.4', 'bior5.5', 'bior6.8'
                * Reverse Biorthogonal: 'rbio1.1', 'rbio1.3',
                                        'rbio1.5' 'rbio2.2',
                                        'rbio2.4', 'rbio2.6',
                                        'rbio2.8', 'rbio3.1',
                                        'rbio3.3', 'rbio3.5',
                                        'rbio3.7','rbio3.9',
                                        'rbio4.4', 'rbio5.5',
                                        'rbio6.8'
            Note:
                * If 'wavelet' is specified, do not include a second
                  input series for the function. Otherwise, include
                  a second input series to provide the filter.
                * Data type is case-sensitive.
            Types: str
        mode:
            Optional Argument.
            Specifies the signal extension mode. Data type is
            case-insensitive.
            Permitted Values:
                * symmetric, sym, symh
                * reflect, symw
                * smooth, spd, sp1
                * constant, sp0
                * zero, zpd
                * periodic, ppd
                * periodization, per
                * antisymmetric, asym, asymh
                * antireflect, asymw
            Default Value: symmetric
            Types: str
        level:
            Optional Argument.
            Specifies the level of decomposition.
            Valid values are [1,15].
            Default Value: 1
            Types: int
        part:
            Optional Argument.
            Specifies the indicator that the input is partial decomposition
            result.
            Note:
                Data type is case-insensitive.
            Permitted Values:
                * a - the approximation
                * d - the detail of decomposition of result.
            Types: str
        input_fmt_input_mode:
            Optional Argument.
            Specifies the input mode supported by the function.
            When there are two input series, then the input_fmt_input_mode
            specification is mandatory.
            Permitted Values:
            The input_fmt_input_mode parameter has the following options:
                * ONE2ONE: Both the primary and secondary series
                           specifications contain a series name which
                           identifies the two series in the function.
                * MANY2ONE: The MANY specification is the primary series
                            declaration. The secondary series specification
                            contains a series name that identifies the single
                            secondary series.
                * MATCH: Both series are defined by their respective series
                         specification instance name declarations.
            Types: str
        output_fmt_index_style:
            Optional Argument.
            Specifies the index style of the output format.
            Permitted Values: NUMERICAL_SEQUENCE
            Default Value: NUMERICAL_SEQUENCE
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an 
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results. 
                    If not specified, a unique table name is internally 
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output 
                    table into. If not specified, table is created into 
                    database specified by the user at the time of context 
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of DWT.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as DWT_obj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage, before importing the
        #        function in user space.
        #     2. User can import the function, if it is available on
        #        Vantage user is connected to.
        #     3. To check the list of UAF analytic functions available
        #        on Vantage user connected to, use
        #        "display_analytic_functions()".
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Import function DWT.
        from teradataml import DWT
        # Load the example data.
        load_example_data("uaf", ["dwt_dataTable", "dwt_filterTable"])
        # Create teradataml DataFrame objects.
        data1 = DataFrame.from_table("dwt_dataTable")
        data2 = DataFrame.from_table("dwt_filterTable")
        # Create teradataml TDSeries objects.
        data1_series_df = TDSeries(data=data1,
                                   id="id",
                                   row_index="rowi",
                                   row_index_style="SEQUENCE",
                                   payload_field="v",
                                   payload_content="REAL")
        data2_series_df = TDSeries(data=data2,
                                   id="id",
                                   row_index="seq",
                                   row_index_style="SEQUENCE",
                                   payload_field=["lo", "hi"],
                                   payload_content="MULTIVAR_REAL")
        # Example 1: Perform discrete wavelet transform using two series as input.
        uaf_out = DWT(data1=data1_series_df,
                      data2=data2_series_df,
                      data2_filter_expr=data2_series_df.id==1,
                      input_fmt_input_mode='MANY2ONE')
        # Print the result DataFrame.
        print(uaf_out.result)
       # Example 2: Perform discrete wavelet transform using single series as input and the wavelet parameter.
        uaf_out = DWT(data1=data1_series_df,
                      wavelet='haar')
        # Print the result DataFrame.
        print(uaf_out.result)
    DWT2D
    DWT2D
    Functions
    DWT2D(data1=None, data1_ﬁlter_expr=None, data2=None, data2_ﬁlter_expr=None, wavelet=None, mode='symmetric', level=1, input_fmt_input_mode=None,
    output_fmt_index_style='NUMERICAL_SEQUENCE', **generic_arguments)
    DESCRIPTION:
        DWT2D() function performs discrete wavelet transform (DWT) for
        two-dimensional data. The algorithm is applied first
        vertically by column axis, then horizontally by row axis.
    PARAMETERS:
        data1:
            Required Argument.
            Specifies the input matrix. Multiple payloads are supported,
            and each payload column is transformed independently.
            Only REAL or MULTIVAR_REAL payload content types are supported.
            Types: TDMatrix
        data1_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data1".
            Types: ColumnExpression
        data2:
            Optional Argument.
            Specifies the input series. The series specifies the filter.
            It should have two payload columns corresponding to low and high
            pass filters. Only MULTIVAR_REAL payload content type is supported.
            Types: TDSeries
        data2_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data2".
            Types: ColumnExpression
        wavelet:
            Optional Argument.
            Specifies the name of the wavelet.
            Permitted families and names are:
                * Daubechies: 'db1' or 'haar', 'db2', 'db3', .... ,'db38'
                * Coiflets: 'coif1', 'coif2', ... , 'coif17'
                * Symlets: 'sym2', 'sym3', ... ,' sym20'
                * Discrete Meyer: 'dmey'
                * Biorthogonal: 'bior1.1', 'bior1.3', 'bior1.5', 'bior2.2',
                                'bior2.4', 'bior2.6', 'bior2.8', 'bior3.1',
                                'bior3.3', 'bior3.5', 'bior3.7', 'bior3.9',
                                'bior4.4', 'bior5.5', 'bior6.8'
                * Reverse Biorthogonal: 'rbio1.1', 'rbio1.3', 'rbio1.5'
                                        'rbio2.2', 'rbio2.4', 'rbio2.6',
                                        'rbio2.8', 'rbio3.1', 'rbio3.3',
                                        'rbio3.5', 'rbio3.7','rbio3.9',
                                        'rbio4.4', 'rbio5.5', 'rbio6.8'
            Note:
                * If 'wavelet' is specified, do not include a second
                  input series for the function. Otherwise, include
                  a second input series to provide the filter.
                * Data type is case-sensitive.
            Types: str
        mode:
            Optional Argument.
            Specifies the signal extension mode. Data type is case-insensitive.
            Permitted Values:
                * symmetric, sym, symh
                * reflect, symw
                * smooth, spd, sp1
                * constant, sp0
                * zero, zpd
                * periodic, ppd
                * periodization, per
                * antisymmetric, asym, asymh
                * antireflect, asymw
            Default Value: symmetric
            Types: str
        level:
            Optional Argument.
            Specifies the level of decomposition. Valid values are [1,15].
            Default Value: 1
            Types: int
        input_fmt_input_mode:
            Optional Argument.
            Specifies the input mode supported by the function.
            When there are two input series, then the "input_fmt_input_mode"
            specification is mandatory.
            Permitted Values:
                * ONE2ONE: Both the primary and secondary series specifications
                           contain a series name which identifies the two series
                           in the function.
                * MANY2ONE: The MANY specification is the primary series
                            declaration. The secondary series specification
                            contains a series name that identifies the single
                            secondary series.
                * MATCH: Both series are defined by their respective series
                         specification instance name declarations.
            Types: str
        output_fmt_index_style:
            Optional Argument.
            Specifies the index style of the output format.
            Permitted Values: NUMERICAL_SEQUENCE
            Default Value: NUMERICAL_SEQUENCE
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an 
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results. 
                    If not specified, a unique table name is internally 
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output 
                    table into. If not specified, table is created into 
                    database specified by the user at the time of context 
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of DWT2D.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as DWT2D_obj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage, before importing the
        #        function in user space.
        #     2. User can import the function, if it is available on
        #        Vantage user is connected to.
        #     3. To check the list of UAF analytic functions available
        #        on Vantage user connected to, use
        #        "display_analytic_functions()".
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Load the example data.
        load_example_data("uaf", ["dwt2d_dataTable", "dwt_filterTable"])
        # Create teradataml DataFrame objects.
        data1 = DataFrame.from_table("dwt2d_dataTable")
        data2 = DataFrame.from_table("dwt_filterTable")
        # Create teradataml TDSeries object.
        data2_series_df = TDSeries(data=data2,
                                   id="id",
                                   row_index="seq",
                                   row_index_style="SEQUENCE",
                                   payload_field=["lo", "hi"],
                                   payload_content="MULTIVAR_REAL")
        # Create teradataml TDMatrix object.
        data1_matrix_df = TDMatrix(data=data1,
                                   id="id",
                                   row_index="y",
                                   row_index_style="SEQUENCE",
                                   column_index="x",
                                   column_index_style="SEQUENCE",
                                   payload_field="v",
                                   payload_content="REAL")
        # Example 1: Perform discrete wavelet transform (DWT) for two-dimensional data using both inputs.
        uaf_out = DWT2D(data1=data1_matrix_df,
                        data2=data2_series_df,
                        data2_filter_expr=data2.id==1,
                        input_fmt_input_mode="MANY2ONE")
        # Print the result DataFrame.
        print(uaf_out.result)
        # Example 2: Perform discrete wavelet transform (DWT) for two-dimensional data
        #            using only one matrix as input and wavelet as 'haar'.
        uaf_out = DWT2D(data1=data1_matrix_df,
                        wavelet='haar')
        # Print the result DataFrame.
        print(uaf_out.result)
    IDWT
    IDWT
    Functions
    IDWT(data1=None, data1_ﬁlter_expr=None, data2=None, data2_ﬁlter_expr=None, wavelet=None, mode='symmetric', part=None, input_fmt_input_mode=None,
    output_fmt_index_style='NUMERICAL_SEQUENCE', **generic_arguments)
    DESCRIPTION:
        IDWT() is a function that performs inverse discrete wavelet transform (IDWT).
    PARAMETERS:
        data1:
            Required Argument.
            Specifies the input series. Multiple
            payloads are supported, and each payload column is 
            transformed independently. Only REAL or MULTIVAR_REAL
            payload content types are supported.
            Types: TDSeries
        data1_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data1".
            Types: ColumnExpression
        data2:
            Optional Argument.
            Specifies the input series. The series specifies the filter.
            It should have two payload columns corresponding to low
            and high pass filters.
            Only MULTIVAR_REAL payload content type is supported.
            Types: TDSeries
        data2_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data2".
            Types: ColumnExpression
        wavelet:
            Optional Argument.
            Specifies the name of the wavelet.
            Permitted Values:
                * Daubechies: 'db1' or 'haar', 'db2', 'db3', .... ,'db38'
                * Coiflets: 'coif1', 'coif2', ... , 'coif17'
                * Symlets: 'sym2', 'sym3', ... ,' sym20'
                * Discrete Meyer: 'dmey'
                * Biorthogonal: 'bior1.1', 'bior1.3', 'bior1.5',
                                'bior2.2', 'bior2.4', 'bior2.6',
                                'bior2.8', 'bior3.1', 'bior3.3',
                                'bior3.5', 'bior3.7', 'bior3.9',
                                'bior4.4', 'bior5.5', 'bior6.8'
                * Reverse Biorthogonal: 'rbio1.1', 'rbio1.3',
                                        'rbio1.5' 'rbio2.2',
                                        'rbio2.4', 'rbio2.6',
                                        'rbio2.8', 'rbio3.1',
                                        'rbio3.3', 'rbio3.5',
                                        'rbio3.7','rbio3.9',
                                        'rbio4.4', 'rbio5.5',
                                        'rbio6.8'
            Note:
                * If 'wavelet' is specified, do not include a second
                  input series for the function. Otherwise, include
                  a second input series to provide the filter.
                * Data type is case-sensitive.
            Types: str
        mode:
            Optional Argument.
            Specifies the signal extension mode.
            Data type is case-insensitive.
            Permitted Values:
                * symmetric, sym, symh
                * reflect, symw
                * smooth, spd, sp1
                * constant, sp0
                * zero, zpd
                * periodic, ppd
                * periodization, per
                * antisymmetric, asym, asymh
                * antireflect, asymw
            Default Value: symmetric
            Types: str
        part:
            Optional Argument.
            Specifies the indicator that the input is partial decomposition
            result.
            Note:
                Data type is case-insensitive.
            Permitted Values:
                * a - the approximation
                * d - the detail of decomposition of result.
            Types: str
        input_fmt_input_mode:
            Optional Argument.
            Specifies the input mode supported by the function.
            When there are two input series, then the input_fmt_input_mode
            specification is mandatory.
            Permitted Values:
                * ONE2ONE: Both the primary and secondary series
                           specifications contain a series name which
                           identifies the two series in the function.
                * MANY2ONE: The MANY specification is the primary series
                            declaration. The secondary series specification
                            contains a series name that identifies the single
                            secondary series.
                * MATCH: Both series are defined by their respective series
                         specification instance name declarations.
            Types: str
        output_fmt_index_style:
            Optional Argument.
            Specifies the index style of the output format.
            Permitted Values: NUMERICAL_SEQUENCE
            Default Value: NUMERICAL_SEQUENCE
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an 
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results. 
                    If not specified, a unique table name is internally 
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output 
                    table into. If not specified, table is created into 
                    database specified by the user at the time of context 
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of IDWT.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as IDWT_obj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage, before importing the
        #        function in user space.
        #     2. User can import the function, if it is available on
        #        Vantage user is connected to.
        #     3. To check the list of UAF analytic functions available
        #        on Vantage user connected to, use
        #        "display_analytic_functions()".
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Import function IDWT.
        from teradataml import IDWT
        # Load the example data.
        load_example_data("uaf", ["idwt_dataTable", "idwt_filterTable"])
        # Create teradataml DataFrame objects.
        data1 = DataFrame.from_table("idwt_dataTable")
        data2 = DataFrame.from_table("idwt_filterTable")
        # Create teradataml TDSeries objects.
        data1_series_df = TDSeries(data=data1,
                                   id="id",
                                   row_index="rowi",
                                   row_index_style="SEQUENCE",
                                   payload_field=["approx"],
                                   payload_content="REAL")
        data2_series_df = TDSeries(data=data2,
                                   id="id",
                                   row_index="seq",
                                   row_index_style="SEQUENCE",
                                   payload_field=["lo", "hi"],
                                   payload_content="MULTIVAR_REAL")
        # Example 1: Perform inverse discrete wavelet transform using 2 series as input.
        uaf_out = IDWT(data1=data1_series_df,
                       data2=data2_series_df,
                       data2_filter_expr=data2.id==1,
                       input_fmt_input_mode="MANY2ONE",
                       part='a')
        # Print the result DataFrame.
        print(uaf_out.result)
        # Example 2: Perform inverse discrete wavelet transform using 1 series as input and wavelet parameter.
        # Create teradataml TDSeries objects.
        data_series_df = TDSeries(data=data1,
                                  id="id",
                                  row_index="rowi",
                                  row_index_style="SEQUENCE",
                                  payload_field=["approx", "detail"],
                                  payload_content="MULTIVAR_REAL")
        uaf_out = IDWT(data1=data_series_df,
                       wavelet='haar')
        # Print the result DataFrame.
        print(uaf_out.result)
    IDWT2D
    IDWT2D
    Functions
    IDWT2D(data1=None, data1_ﬁlter_expr=None, data2=None, data2_ﬁlter_expr=None, wavelet=None, mode='symmetric', input_fmt_input_mode=None,
    output_fmt_index_style='NUMERICAL_SEQUENCE', **generic_arguments)
    DESCRIPTION:
        IDWT2D() function performs inverse discrete wavelet transform
        (IDWT) for two-dimensional data. The algorithm is applied 
        first horizontally by row axis, then vertically by column 
        axis.
    PARAMETERS:
        data1:
            Required Argument.
            Specifies the input matrix. Multiple
            payloads are supported, and each payload column is 
            transformed independently. Only MULTIVAR_REAL payload
            content type is supported.
            Types: TDMatrix
        data1_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data1".
            Types: ColumnExpression
        data2:
            Optional Argument.
            Specifies the input series. The series specifies the filter.
            It should have two payload columns corresponding to low and high
            pass filters. Only MULTIVAR_REAL payload content type is supported.
            Types: TDSeries
        data2_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data2".
            Types: ColumnExpression
        wavelet:
            Optional Argument.
            Specifies the name of the wavelet.
            Option families and names are:
                * Daubechies: 'db1' or 'haar', 'db2', 'db3', .... ,'db38'
                * Coiflets: 'coif1', 'coif2', ... , 'coif17'
                * Symlets: 'sym2', 'sym3', ... ,' sym20'
                * Discrete Meyer: 'dmey'
                * Biorthogonal: 'bior1.1', 'bior1.3', 'bior1.5', 'bior2.2',
                                'bior2.4', 'bior2.6', 'bior2.8', 'bior3.1',
                                'bior3.3', 'bior3.5', 'bior3.7', 'bior3.9',
                                'bior4.4', 'bior5.5', 'bior6.8'
                * Reverse Biorthogonal: 'rbio1.1', 'rbio1.3', 'rbio1.5'
                                        'rbio2.2', 'rbio2.4', 'rbio2.6',
                                        'rbio2.8', 'rbio3.1', 'rbio3.3',
                                        'rbio3.5', 'rbio3.7','rbio3.9',
                                        'rbio4.4', 'rbio5.5', 'rbio6.8'
            Note:
                * If 'wavelet' is specified, do not include a second
                  input series for the function. Otherwise, include
                  a second input series to provide the filter.
                * Data type is case-sensitive.
            Types: str
        mode:
            Optional Argument.
            Specifies the signal extension mode.
            Data type is case-insensitive.
            Permitted Values:
                * symmetric, sym, symh
                * reflect, symw
                * smooth, spd, sp1
                * constant, sp0
                * zero, zpd
                * periodic, ppd
                * periodization, per
                * antisymmetric, asym, asymh
                * antireflect, asymw
            Default Value: symmetric
            Types: str
        input_fmt_input_mode:
            Optional Argument.
            Specifies the input mode supported by the function.
            When there are two input series, then the "input_fmt_input_mode" .
            specification is mandatory.
            Permitted Values:
                * ONE2ONE: Both the primary and secondary series specifications
                           contain a series name which identifies the two series
                           in the function.
                * MANY2ONE: The MANY specification is the primary series
                            declaration. The secondary series specification
                            contains a series name that identifies the single
                            secondary series.
                * MATCH: Both series are defined by their respective series
                         specification instance name declarations.
            Types: str
        output_fmt_index_style:
            Optional Argument.
            Specifies the index style of the output format.
            Permitted Values: NUMERICAL_SEQUENCE
            Default Value: NUMERICAL_SEQUENCE
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an 
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results. 
                    If not specified, a unique table name is internally 
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output 
                    table into. If not specified, table is created into 
                    database specified by the user at the time of context 
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of IDWT2D.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as IDWT2D_obj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage, before importing the
        #        function in user space.
        #     2. User can import the function, if it is available on
        #        Vantage user is connected to.
        #     3. To check the list of UAF analytic functions available
        #        on Vantage user connected to, use
        #        "display_analytic_functions()".
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Import function IDWT2D.
        from teradataml import IDWT2D
        # Load the example data.
        load_example_data("uaf", ["idwt2d_dataTable", "idwt_filterTable"])
        # Create teradataml DataFrame objects.
        data1 = DataFrame.from_table("idwt2d_dataTable")
        data2 = DataFrame.from_table("idwt_filterTable")
        # Create teradataml TDMatrix object.
        data1_matrix_df = TDMatrix(data=data1,
                                   id="id",
                                   row_index="y",
                                   row_index_style="SEQUENCE",
                                   column_index="x",
                                   column_index_style="SEQUENCE",
                                   payload_field="v",
                                   payload_content="REAL")
        # Execute DWT2D
        uaf_out = DWT2D(data1=data1_matrix_df,
                        wavelet='haar')
        # Example 1: Perform inverse discrete wavelet transform using TDAnalyticResult
        #            from DWT2D() as input and wavelet as 'haar'
        # Create teradataml TDAnalyticResult object.
        art_df = TDAnalyticResult(data=uaf_out.result)
        uaf_out = IDWT2D(data1=art_df,
                         wavelet='haar')
        # Print the result DataFrame.
        print(uaf_out.result)
        # Example 1: Perform inverse discrete wavelet transform using TDAnalyticResult from DWT2D()
        #            and TDSeries as input.
        # Create teradataml TDSeries object.
        data2_series_df = TDSeries(data=data2,
                                   id="id",
                                   row_index="seq",
                                   row_index_style="SEQUENCE",
                                   payload_field=["lo", "hi"],
                                   payload_content="MULTIVAR_REAL")
        uaf_out = IDWT2D(data1=art_df,
                         data2=data2_series_df,
                         data2_filter_expr=data2.id==1,
                         input_fmt_input_mode='MANY2ONE')
        # Print the result DataFrame.
        print(uaf_out.result)
    SAX
    SAX
    Functions
    SAX(data=None, data_ﬁlter_expr=None, window_type='GLOBAL', output_type='STRING', mean=None, std_dev=None, window_size=None, output_frequency=1,
    points_per_symbol=1, symbols_per_window=1, alphabet_size=4, bitmap_level=2, code_stats=0, breakpoints=None,
    output_fmt_index_style='NUMERICAL_SEQUENCE', **generic_arguments)
    DESCRIPTION:
        SAX() function uses Piecewise Aggregate Approximation (PAA) and 
        transform a timeseries into sequence of symbols.
        The symbols can be characters, string, and bitmap.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the time series whose value can be REAL or MULTIVAR_REAL.
            Types: TDSeries
        data_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data".
            Types: ColumnExpression
        window_type:
            Optional Argument.
            Specifies the window type used in the SAX transformation.
            Default Value: GLOBAL
            Permitted Values: GLOBAL, SLIDING
            Types: str
        output_type:
            Optional Argument.
            Specifies the output format of the result.
            Default Value: STRING
            Permitted Values: STRING, BITMAP, O_CHARS
            Types: str
        mean:
            Optional Argument.
            Specifies the global mean values that used to
            calculate the SAX code for every partition.
            Note:
                * If "mean" not specified, the function calculates the mean values
                  for every partition.
                * If "mean" specifies a single value but there are multiple payloads,
                  the specified value will apply to all payloads.
                * If "mean" specifies multiple values, each value will be 
                  applied to its corresponding payload.
            Types: int, list of int, float, list of float
        std_dev:
            Optional Argument.
            Specifies the global standard deviation values that used to
            calculate the SAX code for every partition.
            Note:
                * If "std_dev" not specified, the function calculates the standard
                  deviation values for every partition.
                * If "std_dev" specifies a single value but there are multiple payloads,
                  the specified value will apply to all payloads.
                * If "std_dev" specifies multiple values, each value will be
                  applied to its corresponding payload.
            Types: int, list of int, float, list of float
        window_size:
            Optional Argument, Required if "window_type" is SLIDING.
            Specifies the size of the window used in the SAX
            transformation. Maximum value is 64000.
            Types: int
        output_frequency:
            Optional Argument.
            Specifies the number of data points that the window slides
            between successive outputs.
            Note:
                * "output_frequency" is valid only for SLIDING "window_type".
            Default Value: 1
            Types: int
        points_per_symbol:
            Optional Argument.
            Specifies the number of data points to be converted to one SAX
            symbol.
            Note:
                * "points_per_symbol" is valid for GLOBAL "window_type".
            Default Value: 1
            Types: int
        symbols_per_window:
            Optional Argument.
            Specifies the number of SAX symbols to be generated for each
            window.
            Note:
                * "symbols_per_window" is valid for SLIDING "window_type".
            Default Value: 1
            Types: int
        alphabet_size:
            Optional Argument.
            Specifies the number of symbols in the SAX alphabet. 
            The alphabet consists of letters from 'a' to 't'.
            The size of the alphabet must be less than or equal to 20 
            and greater than or equal to 2.
            Default Value: 4
            Types: int
        bitmap_level:
            Optional Argument.
            Specifies the level of the bitmap. The bitmap level
            determines the number of consecutive symbols to be
            converted to one symbol on a bitmap. 
            "bitmap_level" must be greater than or equal to 1 and less than or equal to 4.
            Default Value: 2
            Types: int
        code_stats:
            Optional Argument.
            Specifies whether to print the mean and standard deviation
            Default Value: 0
            Types: int
        breakpoints:
            Optional Argument.
            Specifies the breakpoints to form the SAX code based on "data".
            Types: int, list of int, float, list of float
        output_fmt_index_style:
            Optional Argument.
            Specifies the index style of the output format.
            Permitted Values: NUMERICAL_SEQUENCE
            Default Value: NUMERICAL_SEQUENCE
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an 
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results. 
                    If not specified, a unique table name is internally 
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output 
                    table into. If not specified, table is created into 
                    database specified by the user at the time of context 
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of SAX.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as SAX_obj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage, before importing the
        #        function in user space.
        #     2. User can import the function, if it is available on
        #        Vantage user is connected to.
        #     3. To check the list of UAF analytic functions available
        #        on Vantage user connected to, use
        #        "display_analytic_functions()".
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Import function SAX.
        from teradataml import SAX
        # Load the example data.
        load_example_data("sax", ["finance_data4"])
        # Create teradataml DataFrame object.
        data = DataFrame.from_table("finance_data4")
        # Create teradataml TDSeries object.
        data_series_df = TDSeries(data=data,
                                  id="id",
                                  row_index="period",
                                  row_index_style="SEQUENCE",
                                  payload_field=["expenditure", "income", "investment"],
                                  payload_content="MULTIVAR_REAL")
        # Example 1: Execute SAX() function on the TDSeries to transform the
        #            time series into sequence of symbols using GLOBAL window.
        uaf_out = SAX(data=data_series_df, 
                      window_type='GLOBAL', 
                      output_type='STRING', 
                      mean=[2045.16666, 2387.41666,759.083333], 
                      std_dev=[256.612489,317.496587,113.352594], 
                      output_frequency=1, 
                      points_per_symbol=2,
                      alphabet_size=10,
                      code_stats=True)
        # Print the result DataFrame.
        print(uaf_out.result)
        # Example 2: Execute SAX() function on the TDSeries to transform the
        #            time series into sequence of symbols using SLIDING window.
        uaf_out1 = SAX(data=data_series_df, 
                       window_type='SLIDING', 
                       window_size=4,
                       symbols_per_window=5, 
                       code_stats=True)
        # Print the result DataFrame.
        print(uaf_out1.result)
    WindowDFFT
    WindowDFFT
    Functions
    WindowDFFT(data=None, data_ﬁlter_expr=None, dfft_algorithm=None, dfft_zero_padding_ok=True, dfft_freq_style='K_INTEGRAL', dfft_hertz_sample_rate=None,
    dfft_human_readable=True, window_size_num=None, window_size_perc=None, window_overlap=0, window_is_symmetric=True, window_scale=None,
    window_type=None, window_exponential_center=None, window_exponential_tau=None, window_gaussian_std=None, window_general_cosine_coeff=None,
    window_general_gaussian_shape=None, window_general_gaussian_sigma=None, window_general_hamming_alpha=None, window_kaiser_beta=None,
    window_taylor_num_sidelobes=4, window_taylor_sidelobe_suppression=30, window_taylor_norm=True, window_tukey_alpha=None, output_fmt_content=None,
    **generic_arguments)
    DESCRIPTION:
        WindowDFFT() function applies a user-selected window to data before 
        processing it with DFFT(). Windows are used to remove 
        noise or spectral leakage. The window type is determined 
        for the specific use case based on signal frequency, 
        amplitude, strength and so on.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the time series or spatial series.
            Types: TDSeries, TDAnalyticResult
        data_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data".
            Types: ColumnExpression
        dfft_algorithm:
            Optional Argument.
            Specifies the user-defined algorithm that bypasses the
            internal DFFT planner, and influences the core DFFT
            algorithm associated with the primary DFFT calculation.
            Note:
            * When "dfft_algorithm" is not passed, then the internal DFFT planner selects 
              the most efficient algorithm for the operation.
            Permitted Values: COOLEY_TUKEY, SINGLETON
            Types: str
        dfft_zero_padding_ok:
            Optional Argument.
            Specifies whether to add zeros to the end of a given
            series to achieve a more efficient computation for the
            Fast Fourier Transform coefficients.
            Default Value: True
            Types: bool
        dfft_freq_style:
            Optional Argument.
            Specifies the format or values associated with the x-axis of the
            output.
            Permitted Values: 
                * K_INTEGRAL: Integer representation.
                * K_SAMPLE_RATE: Integer normalized to number entries, with ranges from -0.5 to +0.5.
                * K_RADIANS: Radian ranges from -π to +π.
                * K_HERTZ: Frequency in hertz. Must be used in conjunction with "dfft_hertz_sample_rate".
            Default Value: K_INTEGRAL
            Types: str
        dfft_hertz_sample_rate:
            Optional Argument.
            Specifies the sample rate as a floating point constant, in
            hertz. A value of 10000.0 indicates that the sample 
            points were obtained by sampling at a rate of 10,000 
            hertz.
            Note:
                * "dfft_hertz_sample_rate" is only valid with "dfft_freq_style".
            Types: float
        dfft_human_readable:
            Optional Argument.
            Specifies whether the produced output rows are in human-readable or raw form.
            Human-readable output is symmetric around 0, such as -3, -2, -1, 0, 1, 2, 3 whereas
            raw output is sequential, starting at zero, such as 0, 1, 2, 3.
            Permitted Values:
                * True: Human-readable output.
                * False: Raw output.
            Default Value: True
            Types: bool
        window_size_num:
            Optional Argument.
            Specifies the size of the window.
            Note:
               * "window_size_num" must be greater than zero.
               * "window_size_num" and "window_size_perc" are mutually exclusive.
            Types: int
        window_size_perc:
            Optional Argument.
            Specifies the size of the window within a series as a percentage.
            Note:
               * "window_size_perc" must be greater than zero.
               * "window_size_num" and "window_size_perc" are mutually exclusive.
            Types: float
        window_overlap:
            Optional Argument.
            Specifies the number of values by which the window slides down for 
            each DFFT calculation within a series.
            Note:
                * The value must be less than the window size.
                * To use fraction form, use "window_size_perc".
            Default Value: 0
            Types: int
        window_is_symmetric:
            Optional Argument.
            Specifies whether to use a symmetric or periodic window.
            Permitted Values:
                * False: Periodic
                * True: Symmetric
            Default Value: True
            Types: bool
        window_scale:
            Optional Argument.
            Specifies the spectral density type applied to the result values.
            Permitted Values: DENSITY, SPECTRUM
            Types: str
        window_type:
            Optional Argument.
            Specifies the type of window to use.
            Note: 
                * Some windows have additional options such as Taylor, in which case refer 
                  to the Taylor parameter for specific Taylor window options.
            Permitted Values: 
                * BARTHANN
                * BARTLETT, 
                * BLACKMAN,               
                * BLACKMANHARRIS, 
                * BOHMAN, 
                * BOXCAR, 
                * COSINE, 
                * EXPONENTIAL,
                * FLATTOP,
                * GAUSSIAN, 
                * GENERAL_COSINE, 
                * GENERAL_GAUSSIAN, 
                * GENERAL_HAMMING, 
                * HAMMING, HANN, 
                * KAISER, 
                * NUTTALL, 
                * PARZEN, 
                * TAYLOR, 
                * TRIANG, 
                * TUKEY
            Types: str
        window_exponential_center:
            Optional Argument.
            Specifies the center of the window. 
            It is a parameter for "window_type" as EXPONENTIAL.
            The default value is (windowSize - 1 ) / 2.
            Types: float
        window_exponential_tau:
            Optional Argument.
            Specifies the amount of window decay.
            It is a parameter for window_type as EXPONENTIAL.
            If the "window_exponential_center" is zero, then use ( windowSize - 1 ) / ln( x ) if x is the 
            fractional part of the window remaining at the end of 
            the window.
            Types: float
        window_gaussian_std:
            Optional Argument. Required if "window_type" is GAUSSIAN.
            Specifies the standard deviation of the Gaussian window.
            Types: float
        window_general_cosine_coeff:
            Optional Argument. Required if "window_type" is GENERAL_GUASSIAN.
            Specifies the list of weighing coefficients.
            Types: float, list of float
        window_general_gaussian_shape:
            Optional Argument. Required if "window_type" is GENERAL_GUASSIAN.
            specifies the gaussian shape. 
            Types: float
        window_general_gaussian_sigma:
            Optional Argument. Required if "window_type" is GENERAL_GUASSIAN.
            Specifies the standard deviation value.
            Types: float
        window_general_hamming_alpha:
            Optional Argument. Required if "window_type" is GENERAL_HAMMING.
            Specifies the value of the window coefficient.
            Types: float
        window_kaiser_beta:
            Optional Argument. Required if "window_type" is KAISER.
            Specifies the shape between the main lobe width and side
            lobe level.
            Types: float
        window_taylor_num_sidelobes:
            Optional Argument.
            Specifies the number of nearly constant level sidelobes
            adjacent to the main lobe.
            Default Value: 4
            Types: int
        window_taylor_sidelobe_suppression:
            Optional Argument.
            Specifies the suppression level of the side lobe in decibels
            relative to the DC gain of the main lobe.
            Default Value: 30
            Types: float
        window_taylor_norm:
            Optional Argument.
            Specifies the normalization factor for the Taylor window.
            Permitted Values:
                * False: For an even sized window, divides the window by the 
                         value that would occur between the two middle values.
                * True : For an odd sized window, divides the window by the 
                         largest (middle) value.
            Default Value: True
            Types: bool
        window_tukey_alpha:
            Optional Argument. Required if "window_type" is TUKEY.
            Specifies the shape of the window inside the cosine-tapered region.
            A value of 0 is a rectangular window and value of 1 is the same as a Hann window. 
            Types: float
        output_fmt_content:
            Optional Argument.
            Specifies how the Fourier coefficients should be output.
            Permitted Values: 
                * COMPLEX
                * AMPL_PHASE_RADIANS
                * AMPL_PHASE_DEGREES
                * AMPL_PHASE
                * MULTIVAR_COMPLEX
                * MULTIVAR_AMPL_PHASE_RADIANS
                * MULTIVAR_AMPL_PHASE_DEGREES
                * MULTIVAR_AMPL_PHASE
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an 
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results. 
                    If not specified, a unique table name is internally 
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output 
                    table into. If not specified, table is created into 
                    database specified by the user at the time of context 
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of WindowDFFT.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as WindowDFFT_obj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage, before importing the
        #        function in user space.
        #     2. User can import the function, if it is available on
        #        Vantage user is connected to.
        #     3. To check the list of UAF analytic functions available
        #        on Vantage user connected to, use
        #        "display_analytic_functions()".
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Import function WindowDFFT.
        from teradataml import WindowDFFT
        # Load the example data.
        load_example_data("uaf", ["windowdfft"])
        # Create teradataml DataFrame object.
        data = DataFrame.from_table("windowdfft")
        # Create teradataml TDSeries object.
        data_series_df = TDSeries(data=data,
                                   id="id",
                                   row_index="row_i",
                                   row_index_style="SEQUENCE",
                                   payload_field=["v2"],
                                   payload_content="REAL")
        # Example 1: Execute WindowDFFT() function to apply a window to the data before processing it with DFFT()
        #            with window_type as BOHMAN and dfft_human_readable is True.
        uaf_out = WindowDFFT(data=data_series_df,
                             window_size_num=15,
                             window_overlap=2,
                             window_type="BOHMAN",
                             window_is_symmetric=True,
                             window_scale="SPECTRUM",
                             dfft_algorithm="SINGLETON",
                             dfft_zero_padding_ok=True,
                             dfft_freq_style="K_INTEGRAL",
                             dfft_human_readable=True)
        # Print the result DataFrame.
        print(uaf_out.result)
        # Example 2: Execute WindowDFFT() function to apply a window to the data before processing it with DFFT()
        #            with additional window parameters for window_type as GENERAL_COSINE and dfft_human_readable is False.
        uaf_out1 = WindowDFFT(data=data_series_df,
                              window_size_num=15,
                              window_overlap=2,
                              window_type="GENERAL_COSINE",
                              window_general_cosine_coeff=[2.3,3.7],
                              window_is_symmetric=True,
                              window_scale="SPECTRUM",
                              dfft_algorithm="SINGLETON",
                              dfft_zero_padding_ok=True,
                              dfft_freq_style="K_INTEGRAL",
                              dfft_human_readable=False)
        # Print the result DataFrame.
        print(uaf_out1.result)
    GENERAL UTILITY functions
    CopyArt
    CopyArt
    Functions
    CopyArt(data=None, database_name=None, table_name=None, map_name=None, **generic_arguments)
    DESCRIPTION:
        CopyArt() function creates a copy of an existing analytics result table (ART).
    PARAMETERS:
        data:
            Required Argument.
            Specifies the ART data to be copied.
            Types: DataFrame
        database_name:
            Required Argument.
            Specifies the name of the destination database for copied ART.
            Types: str
        table_name:
            Required Argument.
            Specifies the name of the destination table for copied ART.
            Types: str
        map_name:
            Optional Argument.
            Specifies the name of the map for the destination ART.
            By default, it refers to the map of the 'data'.
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results.
                    If not specified, a unique table name is internally
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output
                    table into. If not specified, table is created into
                    database specified by the user at the time of context
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of CopyArt.
        Output teradataml DataFrames can be accessed using attribute
        references, such as obj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage, before importing the
        #        function in user space.
        #     2. User can import the function, if it is available on
        #        Vantage user is connected to.
        #     3. To check the list of UAF analytic functions available
        #        on Vantage user connected to, use
        #        "display_analytic_functions()".
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Import function CopyArt.
        from teradataml import CopyArt, AutoArima
        # Load the example data.
        load_example_data("uaf", ["blood2ageandweight"])
        # Create teradataml DataFrame object.
        data = DataFrame.from_table("blood2ageandweight")
        # Create teradataml TDSeries object.
        data_series_df = TDSeries(data=data,
                                  id="PatientID",
                                  row_index="SeqNo",
                                  row_index_style="SEQUENCE",
                                  payload_field="BloodFat",
                                  payload_content="REAL")
        # Execute AutoArima function to create ART.
        uaf_out = AutoArima(data=data_series_df,
                            start_pq_nonseasonal=[1, 1],
                            seasonal=False,
                            constant=True,
                            algorithm="MLE",
                            fit_percentage=80,
                            stepwise=True,
                            nmodels=7,
                            fit_metrics=True,
                            residuals=True)
        # Example 1: Execute CopyArt function to copy ART to a destination table name
        #            with persist option.
        res = CopyArt(data=uaf_out.result,
                      database_name="alice",
                      table_name="copied_table",
                      persist=True)
        print(res.result)
        # Example 2: Execute CopyArt function to copy ART to a destination table name.
        res = CopyArt(data=uaf_out.result,
                      database_name="alice",
                      table_name="copied_table2")
        # Print the result DataFrame.
        print(res.result)
        # Example 3: Copy ART to a destination table name using uaf object.
        res = uaf_out.copy(database_name="alice",
                           table_name="copied_table3")
        # Print the result DataFrame.
        print(res.result)
    ExtractResults
    ExtractResults
    Functions
    ExtractResults(data=None, data_ﬁlter_expr=None, **generic_arguments)
    DESCRIPTION:
        The ExtractResults() function retrieves auxiliary result sets
        stored in an ART. The auxillary layers are as follows:
            * ARTFITRESIDUALS contains the residual series.
            * ARTFITMETADATA contains the goodness-of-fit metrics.
            * ARTMODEL shows the validation model context.
            * ARTVALDATA is used for the internal validation process
        The functions that have multiple layers are shown in the table.
        Layers of each function can be extracted from the function output,
        i.e., "result" attribute, using the layer name specified below:
        ------------------------------------------------------------------
        | Function                    | Layers                           |
        ------------------------------------------------------------------
        | LinearRegr                  | 1. ARTPRIMARY                    |
        |                             | 2. ARTFITMETADATA                |
        |                             | 3. ARTFITRESIDUALS               |
        |                             |                                  |
        | MultivarRegr                | 1. ARTPRIMARY                    |
        |                             | 2. ARTFITMETADATA                |
        |                             | 3. ARTFITRESIDUALS               |
        |                             |                                  |
        | ArimaEstimate               | 1. ARTPRIMARY                    |
        |                             | 2. ARTFITMETADATA                |
        |                             | 3. ARTFITRESIDUALS               |
        |                             | 4. ARTMODEL                      |
        |                             | 5. ARTVALDATA                    |
        |                             |                                  |
        | ArimaValidate               | 1. ARTPRIMARY                    |
        |                             | 2. ARTFITMETADATA                |
        |                             | 3. ARTFITRESIDUALS               |
        |                             | 4. ARTMODEL                      |
        |                             |                                  |
        | SeasonalNormalize           | 1. ARTPRIMARY                    |
        |                             | 2. ARTMETADATA                   |
        |                             |                                  |
        | CumulPeriodogram            | 1. ARTPRIMARY                    |
        |                             | 2. ARTCPDATA                     |
        |                             |                                  |
        | MAMean                      | 1. ARTPRIMARY                    |
        |                             | 2. ARTFITMETADATA                |
        |                             | 3. ARTFITRESIDUALS               |
        |                             |                                  |
        | SimpleExp                   | 1. ARTPRIMARY                    |
        |                             | 2. ARTFITMETADATA                |
        |                             | 3. ARTFITRESIDUALS               |
        |                             |                                  |
        | HoltWintersForecast         | 1. ARTPRIMARY                    |
        |                             | 2. ARTFITMETADATA                |
        |                             | 3. ARTSELMETRICS                 |
        |                             | 4. ARTFITRESIDUALS               |
        ------------------------------------------------------------------
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input data as TDAnalyticResult object
            with "layer" argument.
            Types: TDAnalyticResult
        data_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data".
            Types: ColumnExpression
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an 
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results. 
                    If not specified, a unique table name is internally 
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output 
                    table into. If not specified, table is created into 
                    database specified by the user at the time of context 
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of ExtractResults.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as ExtractResults_obj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Load the example data.
        load_example_data("uaf","timeseriesdatasetsd4")
        # Create teradataml DataFrame object.
        df = DataFrame.from_table("timeseriesdatasetsd4")
        # Create teradataml TDSeries object for ArimaEstimate.
        series_arimaestimate = TDSeries(data=df,
                                        id="dataset_id",
                                        row_index="seqno",
                                        row_index_style="SEQUENCE",
                                        payload_field="magnitude",
                                        payload_content="REAL")
        # Function outputs a result set that contains the estimated
        # coefficients with accompanying per-coefficient statistical ratings.
        arima_estimate = ArimaEstimate(data1=series_arimaestimate,
                                       nonseasonal_model_order=[1,0,0],
                                       constant=True,
                                       algorithm='MLE',
                                       fit_percentage=70,
                                       coeff_stats=True,
                                       fit_metrics=True,
                                       residuals=True
                                       )
        # Example 1 : Extract the residuals from the ArimaEstimate function
        #             output, by specifying the layer name 'ARTFITRESIDUALS'.
        # Create teradataml TDAnalyticResult object.
        art_extractresult = TDAnalyticResult(data=arima_estimate.result,
                                             layer="ARTFITRESIDUALS")
        # Execute the function ExtractResults() to extract the layer.
        uaf_out1 = ExtractResults(data=art_extractresult)
        # Print the result DataFrame.
        print(uaf_out1.result)
        # Example 2 : Extract the residuals from the ArimaEstimate function
        #             output, by specifying the layer name 'ARTFITMETADATA'.
        # Create teradataml TDAnalyticResult object.
        art_extractresult = TDAnalyticResult(data=arima_estimate.result,
                                             layer="ARTFITMETADATA")
        # Execute the function ExtractResults() to extract the layer.
        uaf_out2 = ExtractResults(data=art_extractresult)
        # Print the result DataFrame.
        print(uaf_out2.result)
    FilterFactory1d
    FilterFactory1d
    Functions
    FilterFactory1d(ﬁlter_id=None, ﬁlter_type=None, window_type=None, ﬁlter_length=None, transition_bandwidth=None, low_cutoff=None, high_cutoff=None,
    sampling_frequency=None, ﬁlter_description=None, **generic_arguments)
    DESCRIPTION:
        FilterFactory1d() function creates finite impulse response (FIR)
        filter coefficients. The filters are based on certain parameters
        and stored into a common table for reuse.
        Note:
            User needs EXECUTE PROCEDURE privelge on SYSLIB
    PARAMETERS:
        filter_id:
            Required Argument.
            Specifies the filter identifier, based on filter coefficients
            stored in the table.
            Types: int
        filter_type:
            Required Argument.
            Specifies the type of filter to generate.
            Permitted Values:
                * LOWPASS - To remove frequencies above low_cutoff.
                * HIGHPASS - To remove frequencies below high_cutoff.
                * BANDPASS - To remove frequencies below low_cutoff and
                             above high_cutoff.
                * BANDSTOP - To remove frequencies between low_cutoff
                  and high_cutoff.
            Types: str
        window_type:
            Optional Argument.
            Specifies the window function to the filter that maintains a
            smooth drop-off to zero, and avoids extra artifacts in the
            frequency domain. The default is to leave the filter
            coefficients as they are, and not apply any windowing function.
            Permitted Values: BLACKMAN, HAMMING, HANNING, BARTLETT
            Types: str
        filter_length:
            Optional Argument.
            Specifies the length of the filter to generate.
            Overrides "transition_bandwidth" argument if both are supplied,
            and renders the other an optional argument.
            Default is approximately 4/("transition_bandwidth"/
            "sampling_frequency").
            Types: int
        transition_bandwidth:
            Optional Argument.
            Specifies the maximum allowed size for the range of
            frequencies for filter transitions between a passband and stopband.
            This also determines the number of coefficients to be generated.
            Value must be greater than 0.
            A smaller value produces faster drop off at the cost of more coefficients.
            Not used when "filter_length" is supplied.
            Default is bandwidth from "filter_length".
            Types: int OR float
        low_cutoff:
            Optional Argument.
            Specifies  the lower frequency that change between a passband
            and stopband occurs. It must be greater
            than 0. It is not used by default with 'HIGHPASS' filter.
            Types: int OR float
        high_cutoff:
            Optional Argument.
            Specifies the higher frequency that change
            between a passband and stopband occurs. It must be greater
            than 0 and not used by default with 'LOWPASS' filter.
            Types: int OR float
        sampling_frequency:
            Required Argument.
            Specifies the frequency that the data to be filtered was
            sampled. It must be greater than 0.
            Types: int OR float
        filter_description:
            Optional Argument.
            Specifies the description for the filter coefficients
            that contain the same filter ID. Description is only
            written to one row for each filter generated, and 
            ROW_I is 0. Default is a string describing parameters.
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results.
                    If not specified, a unique table name is internally
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output
                    table into. If not specified, table is created into
                    database specified by the user at the time of context
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage, before importing the
        #        function in user space.
        #     2. User can import the function, if it is available on
        #        Vantage user is connected to.
        #     3. To check the list of UAF analytic functions available
        #        on Vantage user connected to, use
        #        "display_analytic_functions()".
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Import function FilterFactory1d.
        from teradataml import FilterFactory1d
        # Example 1: Create finite impulse response (FIR) filter coefficients.
        res = FilterFactory1d(filter_id = 33,
                              filter_type = 'lowpass',
                              window_type = 'blackman',
                              transition_bandwidth = 20.0,
                              low_cutoff = 40.0,
                              sampling_frequency = 200)
        print(res.result)
    InputValidator
    InputValidator
    Functions
    InputValidator(data=None, data_ﬁlter_expr=None, failure_mode=None, **generic_arguments)
    DESCRIPTION:
        The InputValidator() function validates the data and identifies
        series and matrixes that have indiscrete data.
        Discrete data is classified as follows:
            * Series data:
                * Interval is the same for row_index field.
                * No duplicate row_index field in same series.
            * Matrix data:
                * Interval is the same for row_index field.
                * Interval is the same for column_index field.
                * No duplicate row_index or no duplicate column_index in same matrix.
                * Number of rows in each series (wavelet) is the same.
                * For each series (wavelet), column_index starts from same value under row major.
    PARAMETERS:
        data:
            Required Argument.
            Specifies a logical series or a matrix to be validated.
            Types: TDSeries, TDMatrix
        data_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data".
            Types: ColumnExpression
        failure_mode:
            Required Argument.
            Specifies how many rows to display when the
            input instance is indiscrete.
            Permitted Values:
                * FUNC_FIRST - Lists the first row that makes the instance indiscrete.
                * FUNC_ALL - Lists all indiscrete rows.
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results.
                    If not specified, a unique table name is internally
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output
                    table into. If not specified, table is created into
                    database specified by the user at the time of context
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of InputValidator.
        Output teradataml DataFrames can be accessed using attribute
        references, such as InputValidator_obj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Load the example data.
        load_example_data("uaf", ["buoydata_mix"])
        # Create teradataml DataFrame object.
        data = DataFrame.from_table("buoydata_mix")
        # Create teradataml TDSeries object.
        data_series_df = TDSeries(data=data,
                                  row_index="TD_TIMECODE",
                                  row_index_style="TIMECODE",
                                  id=["oceanname", "buoyid"],
                                  payload_field="salinity",
                                  payload_content="REAL")
        # Example 1: Validate the input series to check if it has indiscrete data or not.
        uaf_out = InputValidator(data=data_series_df, failure_mode="FUNC_FIRST")
        # Print the result DataFrame.
        print(uaf_out.result)
    Matrix2Image
    Matrix2Image
    Functions
    Matrix2Image(data=None, data_ﬁlter_expr=None, image='PNG', type=None, colormap='viridis', range=None, red=None, green=None, blue=None, ﬂip_x=False, ﬂip_
    output_fmt_index_style='NUMERICAL_SEQUENCE', **generic_arguments)
    DESCRIPTION:
        Matrix2Image() function converts a matrix to an image.
        The conversion produces an image using color maps.
        The color image produced by Matrix2Image() is limited to
        8-bit color depth.
        In previous versions, Plot() with MESH option was used to
        convert a matrix to an image. Plot() is limited to a
        single payload.
        Matrix2Image() can combine three payloads to create RGB
        color images.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input matrix.
            Multiple payloads are supported, and each
            payload column is transformed independently.
            Only REAL or MULTIVAR_REAL payload content types are supported.
            Types: TDMatrix
        data_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data".
            Types: ColumnExpression
        image:
            Optional Argument.
            Specifies the image output format.
            It can be PNG or JPG.
            Permitted Values: PNG, JPG
            Default Value: PNG
            Types: str
        type:
            Optional Argument.
            Specifies the type of the image. It can be GRAY, RGB
            or COLORMAP.
                * GRAY has a single payload, the output
                  image is a gray scale image.
                * RGB has three payloads corresponding to RED, GREEN and BLUE channels,
                  the output image is a RGB color image.
                * COLORMAP has a single payload. The output image is a RGB color image.
            Note:
                If there is a single payload, then the default
                type is GRAY. If there are three payloads, then the
                default type is RGB.
            Permitted Values: GRAY, RGB, COLORMAP
            Types: str
        colormap:
            Optional Argument.
            Specifies the colormap to use when the "type" is
            COLORMAP. The values correspond to the colormap of
            Plot(). If not specified, then the default colormap is
            "viridis". The value is case-sensitive.
            Default Value: viridis
            Types: str
        range:
            Optional Argument.
            Specifies the range of the single payload value to be
            scaled. By default, the MIN and MAX values of the
            payload are used as the range. Used when "type" is 'GRAY'
            or 'COLORMAP'.
            Types: int, list of int, float, list of float
        red:
            Optional Argument.
            Specifies the range of the first payload value. By
            default, the MIN and MAX values of the payload are 
            used as the range. It is only used when "type" is 'RGB'.
            Types: int, list of int, float, list of float
        green:
            Optional Argument.
            Specifies the range of the second payload value.By
            default, the MIN and MAX values of the payload are 
            used as the range. It is only used when "type" is 'RGB'.
            Types: int, list of int, float, list of float
        blue:
            Optional Argument.
            Specifies the range of the third payload value. By
            default, the MIN and MAX values of the payload are 
            used as the range. It is only used when "type" is 'RGB'.
            Types: int, list of int, float, list of float
        flip_x:
            Optional Argument.
            Specifies the indicator to flip the image horizontally.
            When set to True, flip the image otherwise, do not
            flip the image.
            Default Value: False
            Types: bool
        flip_y:
            Optional Argument.
            Specifies the indicator to flip the image vertically.
            When set to True, flip the image otherwise,
            do not flip the image.
            Default Value: False
            Types: bool
        input_fmt_input_mode:
            Optional Argument.
            Specifies the input mode supported by the function.
            When there are two input series, then the "input_fmt_input_mode" .
            specification is mandatory.
            Permitted Values:
                * ONE2ONE: Both the primary and secondary series specifications
                           contain a series name which identifies the two series
                            in the function.
                * MANY2ONE: The MANY specification is the primary series
                            declaration. The secondary series specification
                            contains a series name that identifies the single
                            secondary series.
                * MATCH: Both series are defined by their respective series
                         specification instance name declarations.
            Types: str
        output_fmt_index_style:
            Optional Argument.
            Specifies the index style of the output format.
            Permitted Values: NUMERICAL_SEQUENCE
            Default Value: NUMERICAL_SEQUENCE
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an 
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results. 
                    If not specified, a unique table name is internally 
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output 
                    table into. If not specified, table is created into 
                    database specified by the user at the time of context 
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of Matrix2Image.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as Matrix2Image_obj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage, before importing the
        #        function in user space.
        #     2. User can import the function, if it is available on
        #        Vantage user is connected to.
        #     3. To check the list of UAF analytic functions available
        #        on Vantage user connected to, use
        #        "display_analytic_functions()".
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Import function Matrix2Image.
        from teradataml import Matrix2Image
        # Convert the image to matrix using 'TD_IMAGE2MATRIX' using output as gray.
        import teradataml
        # Drop the image table, matrixTable, matrixTable_rgb if it is present.
        try:
            db_drop_table('imageTable')
            db_drop_table('matrixTable')
            db_drop_table('matrixTable_rgb')
        except:
            pass
        execute_sql('CREATE TABLE imageTable(id INTEGER, image BLOB);')
        file_dir = os.path.join(os.path.dirname(teradataml.__file__), "data")
        with open(os.path.join(file_dir,'peppers.png'), mode='rb') as file:
            fileContent = file.read()
        sql = 'INSERT INTO imageTable VALUES(?, ?);'
        parameters = (1, fileContent)
        execute_sql(sql, parameters)
        execute_sql("CREATE TABLE matrixTable AS (SELECT * FROM TD_IMAGE2MATRIX ( ON (SELECT id, image FROM imageTable) US
        data = DataFrame('matrixTable')
        # Create teradataml TDMatrix object.
        data_matrix_df = TDMatrix(data=data,
                                  id="id",
                                  row_index="Y",
                                  column_index="X",
                                  row_index_style="SEQUENCE",
                                  column_index_style="SEQUENCE",
                                  payload_field="GRAY",
                                  payload_content="REAL"
                                  )
        # Example 1: Generate Gray Scale Image Output with Fixed Range.
        uaf_out = Matrix2Image(data=data_matrix_df,
                               range=[0,255])
        # Print the result DataFrame.
        print(uaf_out.result)
        # Example 2: Generate Gray Scale Image Output with Automatic Range.
        uaf_out = Matrix2Image(data=data_matrix_df)
        # Print the result DataFrame.
        print(uaf_out.result)
        # Example 3: Generate Colormap Image Output.
        uaf_out = Matrix2Image(data=data_matrix_df,
                               type='colormap',
                               colormap='viridis',
                               range=[0,255])
        # Print the result DataFrame.
        print(uaf_out.result)
        # Convert the image to matrix using 'TD_IMAGE2MATRIX' using output as 'rgb'.
        execute_sql("CREATE TABLE matrixTable_rgb AS (SELECT * FROM TD_IMAGE2MATRIX ( ON (SELECT id, image FROM imageTable
        # Create teradataml DataFrame object.
        data = DataFrame.from_table("matrixTable_rgb")
        # Create teradataml TDMatrix object.
        data_matrix_df = TDMatrix(data=data,
                                  id="id",
                                  row_index="Y",
                                  column_index="X",
                                  row_index_style="SEQUENCE",
                                  column_index_style="SEQUENCE",
                                  payload_field=["RED", "BLUE", "GREEN"],
                                  payload_content="MULTIVAR_REAL"
                                  )
        # Example 4: Generate RGB Image Output with All Channels Range Fixed.
        uaf_out = Matrix2Image(data=data_matrix_df,
                               red=[0,255],
                               green=[0,255],
                               blue=[0,255])
        # Print the result DataFrame.
        print(uaf_out.result)
        # Example 5: Generate RGB Image Output with Automatic Range for All Channels.
        uaf_out = Matrix2Image(data=data_matrix_df)
        # Print the result DataFrame.
        print(uaf_out.result)
    MInfo
    MInfo
    Functions
    MInfo(data=None, data_ﬁlter_expr=None, **generic_arguments)
    DESCRIPTION:
        The MInfo() function returns one row for each matrix instance found
        in the input data. Each returned row provides the following information
        about a matrix:
            * Row index data type
            * Starting row index value
            * Ending row index value
            * Number of row index entries
            * Indicator that the matrix is regular (discrete) or irregular along
              row index
            * Row index sample interval for regular series or average sample
              interval for irregular series
            * Column index data type
            * Starting column index value
            * Ending column index value
            * Number of column index entries
            * Indicator that the matrix is regular (discrete) or irregular along
              column index
            * Column index sample interval for regular series or average sample
              interval for irregular series
            * Content type
            * Minimum sample magnitude
            * Maximum sample magnitude
            * Average sample magnitude
            * Number of NULL values
            * Indicator matrix is well formed or malformed
    PARAMETERS:
        data:
            Required Argument.
            Specifies a collection of logical matrixes.
            These matrixes can be regular or irregular. Their indexing
            mechanisms can be time or space.
            Types: TDMatrix
        data_filter_expr:
            Optional Argument.
            Specifies filter expression for "data".
            Types: ColumnExpression
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an 
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results. 
                    If not specified, a unique table name is internally 
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output 
                    table into. If not specified, table is created into 
                    database specified by the user at the time of context 
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of MInfo.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as MInfo_obj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Load the example data.
        load_example_data("uaf", ["ocean_buoys2"])
        # Create teradataml DataFrame object.
        df = DataFrame.from_table("ocean_buoys2")
        # Create teradataml TDMatrix object.
        res = TDMatrix(data=df,
                       id=['ocean_name', 'buoyid'],
                       row_index='TD_TIMECODE',
                       column_index = 'space',
                       row_index_style="TIMECODE",
                       column_index_style="SEQUENCE",
                       payload_field='jsoncol.Measure.salinity',
                       payload_content='REAL')
        # Example 1 : Displays a result set containing metadata about each matrix.
        #             The function returns one row per matrix processed. In
        #             addition, each input payload produces the output variables
        #             for MIN_MAG, MAX_MAG and AVG_MAG. The output varies depending
        #             on the number of input payloads.
        uaf_out = MInfo(data=res)
        # Print the result DataFrame.
        print(uaf_out.result)
    SInfo
    SInfo
    Functions
    SInfo(data=None, data_ﬁlter_expr=None, **generic_arguments)
    DESCRIPTION:
        The SInfo() function returns one row for each series instance found in
        DataFrame. Each returned row provides the following information about
        the series:
            * Index data type
            * Starting index value
            * Ending index value
            * Number of series entries
            * Indicator that the series is regular (discrete) or irregular
            * Sample interval for regular series or average sample interval for
              irregular series
            * Content type
            * Minimum sample magnitude
            * Maximum sample magnitude
            * Average of magnitudes in the series
            * Root-mean-square for each magnitude
    PARAMETERS:
        data:
            Required Argument.
            Specifies a logical series of either the regular or irregular type based on
            time or space.
            Types: TDSeries
        data_filter_expr:
            Optional Argument.
            Specifies filter expression for "data".
            Types: ColumnExpression
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an 
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results. 
                    If not specified, a unique table name is internally 
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output 
                    table into. If not specified, table is created into 
                    database specified by the user at the time of context 
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of SInfo.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as SInfo_obj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Load the example data.
        load_example_data("uaf", ["ocean_buoys2"])
        # Create teradataml DataFrame object.
        data = DataFrame.from_table("ocean_buoys2")
        # Create teradataml TDSeries object.
        data_series_df = TDSeries(data=data,
                                  id=["ocean_name","buoyid"],
                                  row_index="TD_TIMECODE",
                                  row_index_style="TIMECODE",
                                  payload_field="jsoncol.Measure.salinity",
                                  payload_content="REAL")
        # Example 1 : Display a result set with metadata about the series.
        #             There is one output row for each series. In addition,
        #             each input payload produces the output variables for
        #             MIN, MAX, AVG and RMS. The output varies depending on
        #             the number of input payloads.
        uaf_out = SInfo(data=data_series_df)
        # Print the result DataFrame.
        print(uaf_out.result)
    TrackingOp
    TrackingOp
    Functions
    TrackingOp(data=None, data_ﬁlter_expr=None, distance=None, speed=None, time_spent=None, metric=None, **generic_arguments)
    DESCRIPTION:
        The TrackingOp() function is a multi-dimensional function for geospatial data.
        It calculates the trip distance, speed, time, and so on for a trip.
    PARAMETERS:
        data:
            Required Argument.
            Specifies a multivariate time series as an input.
            The first three fields of the payload fields must be as follows:
                * Field 1: A column or field which is a timestamp or timestamp with time zone data type.
                           The field represents the arrival time associated with the object being tracked.
                * Field 2: A column or field which is a timestamp or timestamp with time zone data type.
                           The field represents the departure time associated with the object being tracked.
                * Field 3: A column or field which is a geospatial data type that represents the location of
                           the object being tracked.
                Any number of fields may follow the first three fields, and can be any non-LOB data type.
            Types: TDSeries
        data_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data".
            Types: ColumnExpression
        distance:
            Optional Argument.
            Specifies whether to calculate the track distance.
            When set to True, calculates the distance, otherwise not.
            Default Value: False
            Types: bool
        speed:
            Optional Argument.
            Specifies whether to calculate the average speed
            with the maximum and minimum values.
            When set to True, calculates the trip's average
            speed along with max and min speeds in that trip, otherwise
            no action is taken.
            Default Value: False
            Types: bool
        time_spent:
            Optional Argument.
            Specifies whether to calculate the total time for the trip.
            When set to True, calculates the total time
            spent of the trip, otherwise no action is taken.
            Default Value: False
            Types: bool
        metric:
            Optional Argument.
            Specifies the metric to be used for distance and time.
            When set to True, distance and speed should be expressed
            in kilometer and Km/Hr, otherwise distance and speed should
            be expressed in miles and miles/Hr.
            Default Value: False
            Types: bool
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an 
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results. 
                    If not specified, a unique table name is internally 
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output 
                    table into. If not specified, table is created into 
                    database specified by the user at the time of context 
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of TrackingOp.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as TrackingOp_obj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            1. result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Load the example data.
        load_example_data("uaf", "train_tracking")
        # Create teradataml DataFrame object.
        data = DataFrame.from_table("train_tracking")
        # Create teradataml TDSeries object.
        data_series_df = TDSeries(data=data,
                                  id=["train_id", "schedule_date"],
                                  row_index="arrival_time",
                                  row_index_style="TIMECODE",
                                  payload_field=["arrival_time", "departure_time", "geo_tag"],
                                  payload_content="MULTIVAR_ANYTYPE")
        # Example 1 : Calculate total distance, minimum, maximum and average speed,
        #             trip_time and run_time for the train.
        uaf_out = TrackingOp(data=data_series_df,
                             distance=True,
                             speed=True,
                             time_spent=True,
                             metric=True)
        # Print the result DataFrame.
        print(uaf_out.result)
    ANOMALY DETECTION functions
    IQR
    IQR
    Functions
    IQR(data=None, data_ﬁlter_expr=None, stat_metrics=False, output_fmt_index_style='NUMERICAL_SEQUENCE', **generic_arguments)
    DESCRIPTION:
        Anomaly detection identifies data points, events and observations that
        deviate from the normal behavior of the data set.
        Anomalous data can indicate critical incidents, such as a change in
        consumer behavior or observations that are suspicious.
        Anomalies in data are also called standard deviations, outliers, noise,
        novelties, and exceptions.
        IQR() uses interquartile range for anomaly detection. Any data point
        that falls outside of 1.5 times of an interquartile range below
        the first quartile and above the third quartile is considered an outlier.
        The IQR() function creates a two-layered ART table.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the time series whose value can be REAL or MULTIVAR_REAL.
            Types: TDSeries
        data_filter_expr:
            Optional Argument.
            Specifies the filter expression for "data".
            Types: ColumnExpression
        stat_metrics:
            Optional Argument.
            Specifies the indicator for the secondary layer
            to indicate the number of outliers.
            Default Value: False
            Types: bool
        output_fmt_index_style:
            Optional Argument.
            Specifies the INDEX_STYLE of the output format.
            Permitted Values: NUMERICAL_SEQUENCE
            Default Value: NUMERICAL_SEQUENCE
            Types: str
        **generic_arguments:
            Specifies the generic keyword arguments of UAF functions.
            Below are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Note that, when UAF function is executed, an 
                    analytic result table (ART) is created.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile ART or not. When set to
                    True, results are stored in a volatile ART,
                    otherwise not.
                    Default Value: False
                    Types: bool
                output_table_name:
                    Optional Argument.
                    Specifies the name of the table to store results. 
                    If not specified, a unique table name is internally 
                    generated.
                    Types: str
                output_db_name:
                    Optional Argument.
                    Specifies the name of the database to create output 
                    table into. If not specified, table is created into 
                    database specified by the user at the time of context 
                    creation or configuration parameter. Argument is ignored,
                    if "output_table_name" is not specified.
                    Types: str
    RETURNS:
        Instance of IQR.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as IQR_obj.<attribute_name>.
        Output teradataml DataFrame attribute names are:
            1. result
            2. statsdata
            3. fitmetadata
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage, before importing the
        #        function in user space.
        #     2. User can import the function, if it is available on
        #        Vantage user is connected to.
        #     3. To check the list of UAF analytic functions available
        #        on Vantage user connected to, use
        #        "display_analytic_functions()".
        # Check the list of available UAF analytic functions.
        display_analytic_functions(type="UAF")
        # Load the example data.
        load_example_data("uaf", ["real_values"])
        # Create teradataml DataFrame object.
        data = DataFrame.from_table("real_values")
        # Create teradataml TDSeries object.
        data_series_df = TDSeries(data=data,
                                   id="id",
                                   row_index="TD_TIMECODE",
                                   payload_field="val",
                                   payload_content="REAL")
        # Example 1: Detect which and how many values are considered outliers.
        uaf_out = IQR(data=data_series_df,
                      stat_metrics=True)
        # Print the result DataFrames.
        print(uaf_out.result)
        print(uaf_out.statsdata)