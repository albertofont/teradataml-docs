# teradataml Plotting Functions

Axis
Methods of Axis
__init__
teradataml.plot.Axis.__init__ = __init__(self, **kwargs)
    Constructor for Axis.
    PARAMETERS:
        cmap:
            Optional Argument.
            Specifies the name of the colormap to be used for plotting.
            Notes:
                 * Significant only when corresponding type of plots is mesh or geometry.
                 * Ignored for other type of plots.
            Permitted Values:
                * All the colormaps mentioned in below URL's are supported.
                    * https://matplotlib.org/stable/tutorials/colors/colormaps.html
                    * https://matplotlib.org/cmocean/
            Types: str
        color:
            Optional Argument.
            Specifies the color for the plot.
            Note:
                Hexadecimal color codes are not supported.
            Permitted Values:
                * blue
                * orange
                * green
                * red
                * purple
                * brown
                * pink
                * gray
                * olive
                * cyan
                * Apart from above mentioned colors, the colors mentioned in
                  https://xkcd.com/color/rgb are also supported.
            Default Value: blue
            Types: str OR list of str
        grid_color:
            Optional Argument.
            Specifies the color of the grid. By default, grid is generated with
            Gray69(#b0b0b0) color.
            Note:
                Hexadecimal color codes are not supported.
            Permitted Values:
                * 'blue'
                * 'orange'
                * 'green'
                * 'red'
                * 'purple'
                * 'brown'
                * 'pink'
                * 'gray'
                * 'olive'
                * 'cyan'
                * Apart from above mentioned colors, the colors mentioned in
                  https://xkcd.com/color/rgb are also supported.
            Default Value: 'gray'
            Types: str
        grid_format:
            Optional Argument.
            Specifies the format for the grid.
            Types: str
        grid_linestyle:
            Optional Argument.
            Specifies the line style of the grid.
            Default Value: -
            Permitted Values:
                * -
                * --
                * -.
            Types: str
        grid_linewidth:
            Optional Argument.
            Specifies the line width of the grid.
            Note:
                Valid range for "grid_linewidth" is: 0.5 <= grid_linewidth <= 10.
            Default Value: 0.8
            Types: int OR float
        legend:
            Optional Argument.
            Specifies the legend(s) for the Plot.
            Types: str OR list of str
        legend_style:
            Optional Argument.
            Specifies the location for legend to display on Plot image. By default,
            legend is displayed at upper right corner.
                * 'upper right'
                * 'upper left'
                * 'lower right'
                * 'lower left'
                * 'right'
                * 'center left'
                * 'center right'
                * 'lower center'
                * 'upper center'
                * 'center'
            Default Value: 'upper right'
            Types: str
        linestyle:
            Optional Argument.
            Specifies the line style for the plot.
            Permitted Values:
                * -
                * --
                * -.
                * :
            Default Value: -
            Types: str OR list of str
        linewidth:
            Optional Argument.
            Specifies the line width for the plot.
            Note:
                Valid range for "linewidth" is: 0.5 <= linewidth <= 10.
            Default Value: 0.8
            Types: int OR float OR list of int OR list of float
        marker:
            Optional Argument.
            Specifies the type of the marker to be used.
            Permitted Values:
                All the markers mentioned in https://matplotlib.org/stable/api/markers_api.html
                are supported.
            Types: str OR list of str
        markersize:
            Optional Argument.
            Specifies the size of the marker.
            Note:
                Valid range for "markersize" is: 1 <= markersize <= 20.
            Default Value: 6
            Types: int OR float OR list of int OR list of float
        position:
            Optional Argument.
            Specifies the position of the axis in the Figure. 1st element
            represents the row and second element represents column.
            Default Value: (1, 1)
            Types: tuple
        reverse_xaxis:
            Optional Argument.
            Specifies whether to reverse tick values on x-axis or not.
            Default Value: False
            Types: bool
        reverse_yaxis:
            Optional Argument.
            Specifies whether to reverse tick values on y-axis or not.
            Default Value: False
            Types: bool
        span:
            Optional Argument.
            Specifies the span of the axis in the Figure. 1st element
            represents the row and second element represents column.
            For Example,
                Span of (2, 1) specifies the Axis occupies 2 rows and 1 column
                in Figure.
            Default Value: (1, 1)
            Types: tuple
        series_identifier:
            Optional Argument.
            Specifies the teradataml GeoDataFrame Column which represents the
            identifier for the data. As many plots as distinct "series_identifier"
            are generated in a single Axis.
            For example:
                consider the below data in teradataml GeoDataFrame.
                       ID   x   y
                    0  1    1   1
                    1  1    2   2
                    2  2   10  10
                    3  2   20  20
                If "series_identifier" is not specified, simple plot is
                generated where every 'y' is plotted against 'x' in a
                single plot. However, specifying "series_identifier" as 'ID'
                generates two plots in a single axis. One plot is for ID 1
                and another plot is for ID 2.
            Types: teradataml GeoDataFrame Column.
        title:
            Optional Argument.
            Specifies the title for the Axis.
            Types: str
        xlabel:
            Optional Argument.
            Specifies the label for x-axis.
            Notes:
                 * When set to empty string, label is not displayed for x-axis.
                 * When set to None, name of the x-axis column is displayed as
                   label.
            Types: str
        xlim:
            Optional Argument.
            Specifies the range for xtick values.
            Types: tuple
        xtick_format:
            Optional Argument.
            Specifies how to format tick values for x-axis.
            Types: str
        ylabel:
            Optional Argument.
            Specifies the label for y-axis.
            Notes:
                 * When set to empty string, label is not displayed for y-axis.
                 * When set to None, name of the y-axis column(s) is displayed as
                   label.
            Types: str
        ylim:
            Optional Argument.
            Specifies the range for ytick values.
            Types: tuple
        ytick_format:
            Optional Argument.
            Specifies how to format tick values for y-axis.
            Types: str
        vmin:
            Optional Argument.
            Specifies the lower range of the color map. By default, the range
            is derived from data and color codes are assigned accordingly.
            Note:
                "vmin" significant only for Geometry Plot.
            Types: int OR float
        vmax:
            Optional Argument.
            Specifies the upper range of the color map. By default, the range is
            derived from data and color codes are assigned accordingly.
            Note:
                "vmax" significant only for Geometry Plot.
            For example:
                Assuming user wants to use colormap 'matter' and derive the colors for
                values which are in between 1 and 100.
                Note:
                    Colormap 'matter' starts with Pale Yellow and ends with Violet.
                * If "colormap_range" is not specified, then range is derived from
                  existing values. Thus, colors are represented as below in the whole range:
                  * 1 as Pale Yellow.
                  * 100 as Violet.
                * If "colormap_range" is specified as -100 and 100, the value 1 is at middle of
                  the specified range. Thus, colors are represented as below in the whole range:
                  * -100 as Pale Yellow.
                  * 1 as Orange.
                  * 100 as Violet.
            Types: int OR float
    EXAMPLES:
        # Example 1: Create an Axis with marker as 'Pentagon'.
        >>> from teradataml import Axis
        >>> ax = Axis(marker="p")
        # Example 2: Create an Axis which does not have x-tick values
        #            and y-tick values but it should have grid.
        #            Note that the grid lines should be in the format of '-.'
        >>> from teradataml import Axis
        >>> ax = Axis(xtick_format="", ytick_format="", grid_linestyle="-.")
        # Example 3: Create an Axis which should plot only for the values
        #            between -10 to 100 on x-axis.
        >>> from teradataml import Axis
        >>> ax = Axis(xlim=(-10, 100))
        # Example 4: Create an Axis which should display legend at upper left
        #            corner and it should disable both x and y axis labels.
        >>> from teradataml import Axis
        >>> ax = Axis(legend_style="upper left", xlabel="", ylabel="")
        # Example 5: Create an Axis to format the y-axis tick values to
        #            display up to two decimal points. Also, use the color
        #            'dark green' for plotting.
        #            Note: Consider y-axis data has 5 digit floating numbers.
        >>> from teradataml import Axis
        >>> ax = Axis(ytick_format="99999.99", color='dark green')
    RAISES:
        TeradataMlException
    Properties of Axis
    cmap
    teradataml.plot.Axis.cmap
    Getter for argument "cmap".
    color
    teradataml.plot.Axis.color
    Getter for argument "color".
    grid_color
    teradataml.plot.Axis.grid_color
    Getter for argument "grid_color".
    grid_format
    teradataml.plot.Axis.grid_format
    Getter for argument "grid_format".
    grid_linestyle
    teradataml.plot.Axis.grid_linestyle
    Getter for argument "grid_linestyle".
    grid_linewidth
    teradataml.plot.Axis.grid_linewidth
    Getter for argument "grid_linewidth".
    kind
    teradataml.plot.Axis.kind
    Getter for argument "kind".
    legend
    teradataml.plot.Axis.legend
    Getter for argument "legend".
    legend_style
    teradataml.plot.Axis.legend_style
    Getter for argument "legend_style".
    linestyle
    teradataml.plot.Axis.linestyle
    Getter for argument "linestyle".
    linewidth
    teradataml.plot.Axis.linewidth
    Getter for argument "linewidth".
    marker
    teradataml.plot.Axis.marker
    Getter for argument "marker".
    markersize
    teradataml.plot.Axis.markersize
    Getter for argument "markersize".
    position
    teradataml.plot.Axis.position
    Getter for argument "position".
    reverse_xaxis
    teradataml.plot.Axis.reverse_xaxis
    Getter for argument "reverse_xaxis".
    reverse_yaxis
    teradataml.plot.Axis.reverse_yaxis
    Getter for argument "reverse_yaxis".
    span
    teradataml.plot.Axis.span
    Getter for argument "span".
    title
    teradataml.plot.Axis.title
    Getter for argument "title".
    vmax
    teradataml.plot.Axis.vmax
    Getter for argument "vmax".
    vmin
    teradataml.plot.Axis.vmin
    Getter for argument "vmin".
    xlabel
    teradataml.plot.Axis.xlabel
    Getter for argument "xlabel".
    xlim
    teradataml.plot.Axis.xlim
    Getter for argument "xlim".
    xtick_format
    teradataml.plot.Axis.xtick_format
    Getter for argument "xtick_format".
    ylabel
    teradataml.plot.Axis.ylabel
    Getter for argument "ylabel".
    ylim
    teradataml.plot.Axis.ylim
    Getter for argument "ylim".
    ytick_format
    teradataml.plot.Axis.ytick_format
    Getter for argument "ytick_format".
    Figure
    Methods of Figure
    __init__
    teradataml.plot.Figure.__init__ = __init__(self, width=640, height=480, dpi=100, image_type='png', heading=None, layout=(1, 1))
    Create a new figure for the plot.
    PARAMETERS:
        width:
            Optional Argument.
            Specifies the width of the figure in pixels.
            Default Value: 640
            Notes:
                 * Valid range for "width" is: 400 <= width <= 4096.
                 * Total number of pixels in output image, i.e., the product of "width"
                   and "height" should not exceed 4000000.
            Types: int
        height:
            Optional Argument.
            Specifies the height of the figure in pixels.
            Default Value: 480
            Notes:
                 * Valid range for "height" is: 400 <= height <= 4096.
                 * Total number of pixels in output image, i.e., the product of "width"
                   and "height" should not exceed 4000000.
            Types: int
        dpi:
            Optional Argument.
            Specifies the number of dots per inch for the output image.
            Note:
                * Valid range for "dpi" is: 72 <= width <= 300.
            Default Value: 100 for PNG and JPG Type image.
            Types: int
        image_type:
            Optional Argument.
            Specifies the type of output image.
            Default Value: PNG
            Permitted Values:
                * png
                * jpeg
                * svg
            Types: str
        heading:
            Optional Argument.
            Specifies the heading for the plot.
            Types: str
        layout:
            Optional Argument.
            Specifies the layout for the plot. Element 1 represents rows
            and element 2 represents columns.
            Default Value: (1, 1)
            Types: tuple
    RAISES:
        TeradataMlException
    EXAMPLES:
        # Example 1: Create a Figure object with height and width as 500
        #            pixels and 600 pixels respectively.
        >>> from teradataml import Figure
        >>> figure = Figure(height=500, width=600)
        >>>
        # Example 2: Create a Figure object with default height and width along
        #            with heading as 'Plot Heading'.
        >>> from teradataml import Figure
        >>> figure = Figure(heading="Plot Heading")
        >>>
    Properties of Figure
    dpi
    teradataml.plot.Figure.dpi
    DESCRIPTION:
        Returns the dots per inch for the corresponding figure.
    EXAMPLES:
        # Access the dpi for the Figure object.
        >>> from teradataml import Figure
        >>> figure = Figure()
        >>> figure.dpi
        100
        >>>
    get_axes
    teradataml.plot.Figure.get_axes = get_axes(self)
    DESCRIPTION:
        Function to get the all the axes which are associated with the
        corresponding Figure.
    RETURNS:
        An iterator. Every element of iterator is an Axis or SubAxis object.
    RAISES:
        None.
    EXAMPLES:
        >>> from teradataml import Figure
        >>> figure = Figure()
        >>> axis = list(figure.get_axes())
    heading
    teradataml.plot.Figure.heading
    DESCRIPTION:
        Returns the heading for the corresponding figure.
    EXAMPLES:
        # Access the heading for the Figure object.
        >>> from teradataml import Figure
        >>> figure = Figure(heading="Plot Heading")
        >>> figure.heading
        'Plot Heading'
        >>>
    height
    teradataml.plot.Figure.height
    DESCRIPTION:
        Returns the height of the figure.
    EXAMPLES:
        # Access the height of the figure.
        >>> from teradataml import Figure
        >>> figure = Figure(600)
        >>> figure.height
        600
        >>>
    image_type
    teradataml.plot.Figure.image_type
    DESCRIPTION:
        Returns the type of image for the corresponding figure.
    EXAMPLES:
        # Access the type of image from the Figure object.
        >>> from teradataml import Figure
        >>> figure = Figure()
        >>> figure.image_type
        'png'
        >>>
    layout
    teradataml.plot.Figure.layout
    DESCRIPTION:
        Returns the layout for the corresponding figure.
    EXAMPLES:
        # Access the layout for the Figure object.
        >>> from teradataml import Figure
        >>> figure = Figure()
        >>> figure.layout
        (1, 1)
        >>>
    width
    teradataml.plot.Figure.width
    DESCRIPTION:
        Returns the width of the figure.
    EXAMPLES:
        # Access the width of the figure.
        >>> from teradataml import Figure
        >>> figure = Figure(width=600)
        >>> figure.width
        600
        >>>
    Subplots
    __init__
    teradataml.subplots = subplots(nrows=None, ncols=None, grid=None)
    DESCRIPTION:
        Function to create a figure and a set of subplots. The function
        makes it convenient to create common layouts of subplots, including
        the enclosing figure object.
    PARAMETERS:
        nrows:
            Required when "grid" is not used, optional otherwise.
            Specifies the number of rows of the subplot grid.
            Notes:
                 * Provide either "grid" argument or "nrows" and "ncols" arguments.
                 * "nrows" and "ncols" are mutually inclusive.
            Types: int
        ncols:
            Optional Argument.
            Specifies the number of columns of the subplot grid.
            Notes:
                 * Provide either "grid" argument or "nrows" and "ncols" arguments.
                 * "nrows" and "ncols" are mutually inclusive.
            Types: int
        grid:
            Required when "nrows" and "ncols" are not used, optional otherwise.
            Specifies grid for subplotting. The argument is useful when one or more
            subplot occupies more than one unit of space in figure.
            For example:
                "grid" {(1,1): (1, 1), (1,2): (1,1), (2, 1): (1, 2)} makes 3 subplots
                in a figure.
                * The first subplot which is positioned at first row and first column
                  occupies one row and one column in the figure.
                * The second subplot which is positioned at first row and second column
                  occupies one row and one column in the figure.
                * The third subplot which is positioned at second row and first column
                  occupies one row and two columns in the figure. Thus, the third subplot
                  occupies the entire second row of subplot.
            Notes:
                 * Provide either "grid" argument or "nrows" and "ncols" arguments.
                 * "nrows" and "ncols" are mutually inclusive.
            Types: dict, both keys and values are tuples.
    RETURNS:
        tuple, with two elements. First element represents the object of Figure and
        second element represents list of objects of AxesSubplot.
        Note:
            The default width and height in figure object is 640 and 480 pixels
            respectively. However, incase of subplotting, the default width of
            width and height is 1920 and 1080 respectively.
    RAISES:
        TeradataMlException
    EXAMPLES:
        # Example 1: This example creates a figure with subplot with scatter plots.
        # Load example data.
        >>> load_example_data("uaf", "house_values")
        # Create teradataml DataFrame objects.
        >>> house_values = DataFrame("house_values")
        # Import subplots.
        >>> from teradataml subplots
        # This will help to create a figure with 2 subplots in 1 row.
        # fig and axes is passed to plot().
        >>> fig, axes = subplots(nrows=1, ncols=2)
        # Print the DataFrame.
        >>> print(house_values)
                               TD_TIMECODE  house_val    salary  mortgage
        cityid
        33      2020-07-01 08:00:00.000000    66000.0   29000.0     0.039
        33      2020-04-01 08:00:00.000000    80000.0   22000.0     0.029
        33      2020-05-01 08:00:00.000000   184000.0   49000.0     0.030
        33      2020-06-01 08:00:00.000000   320000.0  112000.0     0.017
        33      2020-09-01 08:00:00.000000   195000.0   72000.0     0.049
        33      2020-10-01 08:00:00.000000   134000.0   89000.0     0.045
        33      2020-11-01 08:00:00.000000   198000.0   49000.0     0.052
        33      2020-08-01 08:00:00.000000   144000.0   74000.0     0.034
        33      2020-03-01 08:00:00.000000   220000.0   76000.0     0.035
        33      2020-02-01 08:00:00.000000   144000.0   50000.0     0.040
        # Create plot with house_val, salary and salary and mortgage.
        >>> plot = house_values.plot(x=house_values.house_val, y=house_values.salary,
                                  ax=axes[0], figure=fig, kind="scatter",
                                  xlim=(100000,250000), ylim=(25000, 100000),
                                  title="Scatter plot of House Val v/s Salary",
                                  color="green")
        >>> plot = house_values.plot(x=house_values.salary, y=house_values.mortgage,
                                  ax=axes[1], figure=fig, kind="scatter",
                                  title="Scatter plot of House Val v/s Mortgage",
                                  color="red")
        # Show the plot.
        >>> plot.show()
        Example 2:
        # Subplot with grid. This will generate a figure with 2 subplots in first row
        # first column and second column respectively and 1 subplot in second row.
        >>> fig, axes = subplots(grid = {(1, 1): (1, 1), (1, 2): (1, 1),
                                         (2, 1): (1, 2)})
        # Print the DataFrame.
        >>> print(house_values)
                               TD_TIMECODE  house_val    salary  mortgage
        cityid
        33      2020-07-01 08:00:00.000000    66000.0   29000.0     0.039
        33      2020-04-01 08:00:00.000000    80000.0   22000.0     0.029
        33      2020-05-01 08:00:00.000000   184000.0   49000.0     0.030
        33      2020-06-01 08:00:00.000000   320000.0  112000.0     0.017
        33      2020-09-01 08:00:00.000000   195000.0   72000.0     0.049
        33      2020-10-01 08:00:00.000000   134000.0   89000.0     0.045
        33      2020-11-01 08:00:00.000000   198000.0   49000.0     0.052
        33      2020-08-01 08:00:00.000000   144000.0   74000.0     0.034
        33      2020-03-01 08:00:00.000000   220000.0   76000.0     0.035
        33      2020-02-01 08:00:00.000000   144000.0   50000.0     0.040
        # Create plot with house_val, salary and salary and mortgage.
        >>> plot = house_values.plot(x=house_values.house_val, y=house_values.salary,
                                  ax=axes[0], figure=fig, kind="scatter",
                                  title="Scatter plot of House Val v/s Salary",
                                  color="green")
        >>> plot = house_values.plot(x=house_values.salary, y=house_values.mortgage,
                                  ax=axes[1], figure=fig, kind="scatter",
                                  title="Scatter plot of Salary v/s Mortgage",
                                  color="red")
        >>> plot = house_values.plot(x=house_values.salary, y=house_values.mortgage,
                                  ax=axes[2], figure=fig, kind="scatter",
                                  title="Scatter plot of House Val v/s Mortgage",
                                  color="blue")
        # Show the plot.
        >>> plot.show()