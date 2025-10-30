# Plotting in teradataml

teradataml supports generating the following types of plots on its DataFrame:
* bar plot
* corr plot
* geometry plot
* line plot
* mesh plot
* scatter plot
* wiggle plot
teradataml exposes additional objects called Axis and Figure which help in customizing the plot with
various options such as grid, plot resolution, and so on. All of these options are discussed in detail in the
following sections.
Also, plotting allows you to combine multiple plots into a single plot with the help of subplot.
* Types of Plots
* Combine Multiple Plots into a Single Plot
* Options for Plotting
## Types of Plots
Note:
In this section, the term "DataFrame" refers to either teradataml DataFrame or
teradataml GeoDataFrame.
* Bar Plot
* Correlation Plot
* Geometry Plot
* Line Plot
* Mesh Plot
* Scatter Plot
* Wiggle Plot
### Bar Plot
Use plot() function to generate a bar plot on DataFrame.
* X-Axis must be a DataFrame Column.
                                        17
* Y-Axis must be a DataFrame Column or a list of DataFrame Columns.
  * If Y-Axis is a DataFrame Column, a simple plot is generated.
  * If Y-Axis is a list of DataFrame Columns, a composite plot is generated.
    See Composite Plot for more details.
#### Example
The following example generates a bar plot which shows the movement of stockprice against the
trading date.
```python
>>> load_example_data("movavg", "ibm_stock")
>>> ibm_stock = DataFrame("ibm_stock")
>>> ibm_stock
name    period  stockprice
id
223  ibm  62/04/05       521.0
19   ibm  61/06/13       487.0
263  ibm  62/06/01       364.0
61   ibm  61/08/11       497.0
183  ibm  62/02/07       552.0
265  ibm  62/06/05       370.0
244  ibm  62/05/04       475.0
305  ibm  62/08/01       385.0
326  ibm  62/08/30       387.0
122  ibm  61/11/08       596.0
>>> plot = ibm_stock.plot(x=ibm_stock.period,
y=ibm_stock.stockprice, kind='bar')
>>> plot.show()
```
### Correlation Plot
Use plot() function to generate a correlation plot on DataFrame.
* X-Axis must be a DataFrame Column.
* Y-Axis must be a tuple or list of tuples.
  Note:
    Tuple must contain only two elements. Both elements must be DataFrame Columns.
  * If Y-Axis is a tuple,, a simple correlation plot is generated.
  * If Y-Axis is a list of tuples, a composite correlation plot is generated.
    See Composite Plot for more details.
#### Example
The following example generates a correlation plot.
The color of this plot is orange and the plot has grid along with custom labels. All of these are additional
options for plotting. See Options for Plotting for more details.
```python
>>> load_example_data("uaf", ["acf"])
>>> df = DataFrame("acf")
>>> df
ROW_I     OUT_v  CONF_OFF_v  CONF_LOW_v  CONF_HI_v
id
1       2  0.828499    0.484255    0.344244   1.312753
1       4  0.481562    0.653203   -0.171641   1.134765
1       5  0.274737    0.682560   -0.407822   0.957297
1       6  0.064830    0.691846   -0.627016   0.756677
1       8 -0.310745    0.694562   -1.005307   0.383817
1       9 -0.454362    0.706218   -1.160581   0.251856
1       7 -0.134393    0.692360   -0.826753   0.557967
1       3  0.670858    0.592091    0.078767   1.262949
1       1  0.941700    0.290772    0.650928   1.232471
1       0  1.000000    0.000000    1.000000   1.000000
>>> df.plot(x=df.ROW_I, y=(df.OUT_v, df.CONF_OFF_v),
kind='corr', color="orange", grid_color='blood',
xlabel='xlabel', ylabel='ylabel',grid_linestyle="--")
```
### Geometry Plot
Use plot() function to generate a geometry plot on GeoDataFrame.
Geometry plot is a plot generated on GeoSpatial data or Geometry data, which is the geometry column
in teradataml GeoDataFrame. Only the columns with ST_GEOMETRY type are allowed for generating
geometry plot.
* The maximum size for ST_GEOMETRY must be less than or equal to 64000.
* The ST_GEOMETRY shape can be POINT, LINESTRING, and so on. POLYGON allows filling of
  different colors.
* X-Axis is not significant geometry plot.
* Y-Axis can be a tuple or DataFrame Column.
  * Geometry plot always requires geometry column and corresponding 'weight' column in a tuple
    format. 'weight' column represents the weight of a shape mentioned in geometry column.
  * If you do not specify geometry column and specifies Y-Axis as DataFrame Column, then the
    default geometry column is considered for plotting.
#### Example
The following example describes the density of population for all the states across US in year 1990 by
generating the geometry plot on a non default Figure.
Note:
* Shapes of US states are generated from Free Blank United States Map in SVG -
    Resources simplemaps.
* Population data is accessed from Historical Population Change Data (1910-2020) (census.gov)
    Historical Population Changes.
```python
>>> load_example_data("geodataframe", ["us_population", "us_states_shapes"])
>>> us_population = DataFrame("us_population")
>>> us_population
location_type  population_year  population
state_name
Georgia            State             1930   2908506.0
Georgia            State             1950   3444578.0
Georgia            State             1960   3943116.0
Georgia            State             1970   4589575.0
Georgia            State             1990   6478216.0
Georgia            State             2000   8186453.0
Georgia            State             1980   5463105.0
Georgia            State             1940   3123723.0
Georgia            State             1920   2895832.0
Georgia            State             1910   2609121.0
>>> us_states_shapes = GeoDataFrame("us_states_shapes")
>>> us_states_shapes
state_name                     state_shape
id
NM     New Mexico  POLYGON ((472.45213 324.75551,
VA       Virginia  POLYGON ((908.75086 270.98255,
ND   North Dakota  POLYGON ((556.50879 73.847349,
OK       Oklahoma  POLYGON ((609.50526 322.91131,
WI      Wisconsin  POLYGON ((705.79187 134.80299,
RI   Rhode Island  POLYGON ((946.50841 152.08022,
HI         Hawaii  POLYGON ((416.34965 514.99923,
KY       Kentucky  POLYGON ((693.17367 317.18459,
WV  West Virginia  POLYGON ((836.73002 223.71281,
NJ     New Jersey  POLYGON ((916.80709 207.30914,
```
* Join shapes with population and filter only 1990 data.
```python
>>> population_data = us_states_shapes.join(us_population,
on=us_population.state_name
== us_states_shapes.state_name,
lsuffix="us",
rsuffix="t2")
>>> population_data = population_data.select(["us_state_name",
"state_shape", "population_year", "population"])
>>> population_data_1990 = population_data[population_data.population_year
== 1990]
>>> population_data_1990
us_state_name                     state_shape  population_year  population
0     New Mexico  POLYGON ((472.45213 324.75551,             1990   1515069.0
1         Hawaii  POLYGON ((416.34965 514.99923,             1990   1108229.0
2       Kentucky  POLYGON ((693.17367 317.18459,             1990   3685296.0
3     New Jersey  POLYGON ((916.80709 207.30914,             1990   7730188.0
4   North Dakota  POLYGON ((556.50879 73.847349,             1990    638800.0
5       Oklahoma  POLYGON ((609.50526 322.91131,             1990   3145585.0
6  West Virginia  POLYGON ((836.73002 223.71281,             1990   1793477.0
7      Wisconsin  POLYGON ((705.79187 134.80299,             1990   4891769.0
8       Virginia  POLYGON ((908.75086 270.98255,             1990   6187358.0
9   Rhode Island  POLYGON ((946.50841 152.08022,             1990   1003464.0
>>> from teradataml import Figure
>>> figure = Figure(width=1550, height=860)
>>> figure.heading = "Geometry Plot"
>>> plot_1990 = population_data_1990.plot(y=(population_data_1990.population,
population_data_1990.state_shape),
cmap='rainbow',
figure=figure,
reverse_yaxis=True,
title="US 1990 Population",
xlabel="",
ylabel="")
>>> plot_1990.show()
```
### Line Plot
Use plot() function to generate a line plot on DataFrame.
* X-Axis must be a DataFrame Column.
* Y-Axis must be a DataFrame Column or a list of DataFrame Columns.
  * If Y-Axis is a DataFrame Column, a simple plot is generated.
  * If Y-Axis is a list of DataFrame Columns, a composite plot is generated.
    See Composite Plot for more details.
#### Example
The following example generates a line plot which shows the movement of stockprice against the
trading date.
```python
>>> load_example_data("movavg", "ibm_stock")
>>> ibm_stock = DataFrame("ibm_stock")
>>> ibm_stock
name    period  stockprice
id
223  ibm  62/04/05       521.0
19   ibm  61/06/13       487.0
263  ibm  62/06/01       364.0
61   ibm  61/08/11       497.0
183  ibm  62/02/07       552.0
265  ibm  62/06/05       370.0
244  ibm  62/05/04       475.0
305  ibm  62/08/01       385.0
326  ibm  62/08/30       387.0
122  ibm  61/11/08       596.0
>>> from teradataml import configure
>>> configure.inline_plot = False
>>> plot = ibm_stock.plot(x=ibm_stock.period, y=ibm_stock.stockprice,
xtick_format='MMM', ytick_format='9,99.99',
xlabel='xlabel', ylabel='ylabel', color="orange")
>>> plot.show()
>>> configure.inline_plot = True
```
### Mesh Plot
Use plot() function to generate a mesh plot on DataFrame.
* X-Axis must be a DataFrame Column.
* Y-Axis must be a DataFrame Column.
* Along with Y-Axis, mesh plot requires another argument called scale.
#### Example
```python
>>> load_example_data("uaf", ["waveletTable"])
>>> df = DataFrame("waveletTable")
>>> df
x      t      y             c
ID
a   94.0  800.0  701.0 -2.034400e-22
a   94.0  800.0  702.0 -4.217551e-22
a   94.0  800.0  702.5 -5.192715e-22
a   94.0  800.0  703.0 -5.182389e-22
a   94.0  800.0  704.0  5.473949e-22
a   94.0  800.0  704.5  2.389177e-21
a   94.0  800.0  703.5 -2.592409e-22
a   94.0  800.0  701.5 -3.051780e-22
a   94.0  800.0  700.5 -1.266145e-22
a   94.0  800.0  700.0 -7.378603e-23
>>> plot = df.plot(x=df.x,
y=df.y,
scale=df.c,
kind='mesh',
cmap='matter',
vmin=-0.5,
vmax=0.5)
>>> plot.show()
```
### Scatter Plot
Use plot() function to generate a scatter plot on DataFrame.
* X-Axis must be a DataFrame Column.
* Y-Axis must be a DataFrame Column or a list of DataFrame Columns.
  * If Y-Axis is a DataFrame Column, a simple plot is generated.
  * If Y-Axis is a list of DataFrame Columns, a composite plot is generated.
    See Composite Plot for more details.
Scatter plot supports different markers and you can also choose the size of the marker. See Options for
Plotting section for more details.
#### Example
```python
>>> load_example_data("movavg", "ibm_stock")
>>> ibm_stock = DataFrame("ibm_stock")
>>> ibm_stock
name    period  stockprice
id
223  ibm  62/04/05       521.0
19   ibm  61/06/13       487.0
263  ibm  62/06/01       364.0
61   ibm  61/08/11       497.0
183  ibm  62/02/07       552.0
265  ibm  62/06/05       370.0
244  ibm  62/05/04       475.0
305  ibm  62/08/01       385.0
326  ibm  62/08/30       387.0
122  ibm  61/11/08       596.0
>>> ibm_stock.plot(x=ibm_stock.period, y=ibm_stock.stockprice,
kind='scatter', xlabel='', ylabel='',
color="orange", grid_color='brown',
grid_linewidth=2, grid_linestyle="-.",
marker="p", marker_size=8)
```
### Wiggle Plot
Use plot() function to generate a wiggle plot on DataFrame.
* X-Axis must be a DataFrame Column.
* Y-Axis must be a DataFrame Column.
* Along with Y-Axis, wiggle plot requires another argument called scale.
#### Example
```python
>>> load_example_data("uaf", ["waveletTable"])
>>> wiggle = DataFrame("waveletTable")
>>> wiggle.plot(x=wiggle.x, y=wiggle.y, scale=wiggle.c, kind='wiggle')
```
## Combine Multiple Plots into a Single Plot
teradataml provides two ways to combine multiple plots into a single plot.
* Plot multiple data points against a single Y-Axis.
* Create multiple Axis on same figure and every Axis can have it’s own plot.
See details about these two ways with examples in the following sections:
* Composite Plot
* Subplot
### Composite Plot
If plot has only one Axis and the data is plotted between one DataFrame Column and multiple DataFrame
Columns, then it is called as composite plot.
* X-Axis represents DataFrame Column.
* Y-Axis represents a list DataFrame Columns.
Note:
Composite plots are not supported for wiggle plots and mesh plots.
* Compositing Line Plot
* Compositing Correlation Plot
#### Compositing Line Plot
This example shows the steps to composite line plot.
* Generating composite plot on a custom figure.
* Generating the JPEG image instead of default PNG image.
#### Example
```python
>>> load_example_data("movavg", "ibm_stock")
>>> ibm_stock = DataFrame("ibm_stock")
>>> ibm_stock
name    period  stockprice
id
223  ibm  62/04/05       521.0
19   ibm  61/06/13       487.0
263  ibm  62/06/01       364.0
61   ibm  61/08/11       497.0
183  ibm  62/02/07       552.0
265  ibm  62/06/05       370.0
244  ibm  62/05/04       475.0
305  ibm  62/08/01       385.0
326  ibm  62/08/30       387.0
122  ibm  61/11/08       596.0
>>> from teradataml import Figure
>>> figure = Figure(width=800, height=900, image_type="jpg",
heading="Composite Plot")
>>> df = ibm_stock.assign(double_price=ibm_stock.stockprice * 2)
>>> df.plot(x=df.period, y=[df.stockprice, df.double_price], style=['dark
orange', 'sand'], figure=figure)
```
#### Compositing Correlation Plot
#### Example
```python
>>> load_example_data("uaf", ["acf"])
>>> df = DataFrame("acf")
>>> df
ROW_I     OUT_v  CONF_OFF_v  CONF_LOW_v  CONF_HI_v
id
1       2  0.828499    0.484255    0.344244   1.312753
1       4  0.481562    0.653203   -0.171641   1.134765
1       5  0.274737    0.682560   -0.407822   0.957297
1       6  0.064830    0.691846   -0.627016   0.756677
1       8 -0.310745    0.694562   -1.005307   0.383817
1       9 -0.454362    0.706218   -1.160581   0.251856
1       7 -0.134393    0.692360   -0.826753   0.557967
1       3  0.670858    0.
091    0.078767   1.262949
1       1  0.941700    0.290772    0.650928   1.232471
1       0  1.000000    0.000000    1.000000   1.000000
>>>
>>> ndf = df.assign(OUT_v2=df.OUT_v * 2, CONF_OFF_v2=df.CONF_OFF_v * 2)
>>> ndf.plot(x=ndf.ROW_I, y=[(ndf.OUT_v, ndf.CONF_OFF_v),
(ndf.OUT_v2, ndf.CONF_OFF_v2)],
...          kind='corr', color=["orange", "brown"],
legend=["out1", "out2"])
```
### Subplot
If plot has more than one Axis, then it is called as subplot. Since the plot allows multiple Axis on the same
Figure, every Axis can occupy same space on Figure or every Axis can occupy different space on Figure.
The function subplots allows you to create a subplot.
Note:
You can use either a regular plot or a composite plot for every Axis in the subplot.
* Subplotting with Every Individual Plot Occupies Same Area in Figure
* Subplotting with Every Individual Plot Occupies Different Areas in Figure
#### Subplotting with Every Individual Plot Occupies Same Area in Figure
This example shows the steps to subplot with every individual plot occupies same area in figure.
Note:
* Shapes of US states are generated from Free Blank United States Map in SVG - Resources
    | Simplemaps.com
* Population data is accessed from Historical Population Change Data (1910-2020) (census.gov)
#### Example
```python
>>> load_example_data("geodataframe", ["us_population", "us_states_shapes"])
>>> from teradataml import subplots
>>> fig, axis = subplots(2, 2)
>>> fig.height = 1200
>>> fig.heading = "Change in population density in US across four decades."
>>> axis
[AxesSubplot(position=(1, 1), span=(1, 1)), AxesSubplot(position=(1,
2), span=(1, 1)), AxesSubplot(position=(2, 1), span=(1, 1)),
AxesSubplot(position=(2, 2), span=(1, 1))]
```
* Prepare the Data for subplotting.
```python
>>> us_population = DataFrame("us_population")
>>> us_population
location_type  population_year  population
state_name
Georgia            State             1930   2908506.0
Georgia            State             1950   3444578.0
Georgia            State             1960   3943116.0
Georgia            State             1970   4589575.0
Georgia            State             1990   6478216.0
Georgia            State             2000   8186453.0
Georgia            State             1980   5463105.0
Georgia            State             1940   3123723.0
Georgia            State             1920   2895832.0
Georgia            State             1910   2609121.0
>>> us_states_shapes = GeoDataFrame("us_states_shapes")
>>> us_states_shapes
state_name                     state_shape
id
NM     New Mexico  POLYGON ((472.45213 324.75551,
VA       Virginia  POLYGON ((908.75086 270.98255,
ND   North Dakota  POLYGON ((556.50879 73.847349,
OK       Oklahoma  POLYGON ((609.50526 322.91131,
WI      Wisconsin  POLYGON ((705.79187 134.80299,
RI   Rhode Island  POLYGON ((946.50841 152.08022,
HI         Hawaii  POLYGON ((416.34965 514.99923,
KY       Kentucky  POLYGON ((693.17367 317.18459,
WV  West Virginia  POLYGON ((836.73002 223.71281,
NJ     New Jersey  POLYGON ((916.80709 207.30914,
```
* Join shapes with population and filter only 1990 data.
```python
>>> population_data = us_states_shapes.join(us_population,
on=us_population.state_name
== us_states_shapes.state_name,
lsuffix="us",
rsuffix="t2")
>>> population_data = population_data.select(["us_state_name",
"state_shape", "population_year", "population"])
```
* Find out the minimum and maximum population. This helps in coloring the plot.
```python
>>> population_data.assign(min_population=population_data.population.min(),
max_population=population_data.population.max(), drop_columns=True)
max_population  min_population
0      39538223.0         55036.0
>>> population_data_2020 = population_data[population_data.population_year
== 2020]
>>> population_data_2010 = population_data[population_data.population_year
== 2010]
>>> population_data_2000 = population_data[population_data.population_year
== 2000]
>>> population_data_1990 = population_data[population_data.population_year
== 1990]
```
* Generate subplot.
  * Plot population_data_1990 on first axis.
```python
>>> plot_1990 =
population_data_1990.plot(y=(population_data_1990.population,
population_data_1990.state_shape),
cmap='rainbow',
figure=fig,
ax=axis[0],
reverse_yaxis=True,
vmin=55036.0,
vmax=39538223.0,
title="US 1990 Population",
xlabel="",
ylabel="")
```
  * Plot population_data_2000 on second axis.
```python
>>> plot_2000 =
population_data_2000.plot(y=(population_data_2000.population,
population_data_2000.state_shape),
cmap='rainbow',
figure=fig,
ax=axis[1],
reverse_yaxis=True,
vmin=55036.0,
vmax=39538223.0,
title="US 2000 Population",
xlabel="",
ylabel="")
```
  * Plot population_data_2010 on third axis.
```python
>>> plot_2010
= population_data_2010.plot(x=population_data_2010.population_year,
y=(population_data_2010.population, population_data_2010.state_shape),
cmap='rainbow',
figure=fig,
ax=axis[2],
reverse_yaxis=True,
vmin=55036.0,
vmax=39538223.0,
title="US 2010 Population",
xlabel="",
ylabel="",
xtick_values_format="")
```
  * Plot population_data_2020 on fourth axis.
```python
>>> plot
= population_data_2020.plot(x=population_data_2020.population_year,
y=(population_data_2020.population, population_data_2020.state_shape),
cmap='rainbow',
figure=fig,
ax=axis[3],
reverse_yaxis=True,
vmin=55036.0,
vmax=39538223.0,
title="US 2020 Population",
xlabel="",
ylabel="",
xtick_values_format="")
>>> plot.show()
```
#### Subplotting with Every Individual Plot Occupies Different Areas in Figure
This example shows the steps to subplot with every individual plot occupies different areas in figure.
#### Example
```python
>>> load_example_data("weightedmovavg", "stock_vol")
```
* Axis and Figure using subplots().
```python
>>> fig, axes = subplots(grid={(1,1): (1, 1), (1,2): (1,1), (2, 1): (1, 2)})
>>> stock_vol = DataFrame("stock_vol")
>>> stock_vol
name    period  stockprice     volume
id
3    pg  61/05/19      17.625  1582000.0
3    pg  61/05/23      17.625   608800.0
3    pg  61/05/24      17.750  1038800.0
3    pg  61/05/25      17.250   911200.0
3    pg  61/05/29      18.000  1672800.0
3    pg  61/05/31      18.000  1735600.0
3    pg  61/05/26      17.500  2005600.0
3    pg  61/05/22      17.750   317200.0
3    pg  61/05/18      18.250  1435200.0
3    pg  61/05/17      18.375  1032000.0
>>> ge_df = stock_vol[stock_vol.name == "ge"]
>>> pg_df = stock_vol[stock_vol.name == "pg"]
>>> ibm_df = stock_vol[stock_vol.name == "ibm"]
>>> ge_plot = ge_df.plot(x=ge_df.period,
y=ge_df.stockprice,
ax=axes[0],
figure=fig,
title="GE Stock Price",
style="green")
>>> pg_plot = pg_df.plot(x=pg_df.period,
y=pg_df.stockprice,
ax=axes[1],
figure=fig,
title="PG Stock Price",
style="red")
>>> ibm_plot = ibm_df.plot(x=ibm_df.period,
y=ibm_df.stockprice,
ax=axes[2],
figure=fig,
title="IBM Stock Price",
style="blue",
kind='bar')
```
## Options for Plotting
* Axis
* Figure
* Options Available for Plot Method
### Axis
Use the Axis() function to generate Axis for the plot.
Optional Arguments:
* cmap: Specifies the name of the colormap to be used for plotting.
  Note:
    This argument is significant only when corresponding type of plots is Mesh Plot or Geometry Plot.
    It is ignored for other types of plots.
  All the colormaps mentioned in the following URLs are supported.
  * Choosing Colormaps in Matplotlib
  * cmocean: Colormaps for Oceanography
* color: Specifies the color for the plot.
  Permitted values:
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
  * Colors mentioned in RGB Monitor Colors
  Default value is 'blue'.
  Note:
    Hexadecimal color codes are not supported.
* grid_color: Specifies the color of the grid.
  By default, grid is generated with Gray69(#b0b0b0) color.
  Permitted values:
  * 'blue'
  * 'orange'
  * 'green'
  * 'red'
  * 'purple
  * 'brown'
  * 'pink'
  * 'gray'
  * 'olive'
  * 'cyan'
  * Colors mentioned in RGB Monitor Colors
  Note:
    Hexadecimal color codes are not supported.
* grid_format: Specifies the format for the grid.
* grid_linestyle: Specifies the line style of the grid.
  Permitted values:
  * '-'
* '- -'
  * '- .'
  Default value is '-'.
* grid_linewidth: Specifies the line width of the grid.
  Valid range: [0.5, 10].
  Default value is 0.8.
* legend: Specifies the legends for the plot.
* legend_style: Specifies the location for legend to display on Plot image.
  By default, legend is displayed at upper right corner.
  Permitted values:
  * 'upper right'
  * 'upper left'
  * 'lower right'
  * 'lower left'
  * 'right'
  * 'center left
  * 'center right'
  * 'lower center'
  * 'upper center'
  * 'center'
* lineyle: Specifies the line style for the plot.
  Permitted values:
  * '-'
  * '- -'
  * '- .'
  * ':'
  Default value is '-'.
* linewidth: Specifies the line width for the plot.
  Valid range: [0.5, 10].
  Default value is 0.8.
* marker: Specifies the type of the marker to be used.
  All the markers mentioned in matplotlib.markers are supported.
* markersize: Specifies the size of the marker.
  Valid range: [1, 20].
  Default value is 6.
* position: Specifies the position of the axis in the Figure.
  This argument accepts a tuple of two elements: The first element represents the row and the second
  element represents column.
  Default value is (1,1).
* reverse_xaxis: Specifies whether to reverse tick values on x-axis or not.
  Default value is False.
* reverse_yaxis: Specifies whether to reverse tick values on y-axis or not.
  Default value is False.
* span: Specifies the span of the axis in the Figure.
  The argument accepts a tuple of two elements: The first element represents the row and the second
  element represents column.
  For example, span of (2, 1) specifies the Axis occupies 2 rows and 1 column in Figure.
  Default value is (1,1).
* series_identifier: Specifies the teradataml GeoDataFrame Column which represents the identifier for
  the data. As many plots with distinct "series_identifier" are generated in a single Axis.
  For example, consider the following data in teradataml GeoDataFrame:
```python
ID   x   y
0  1    1   1
1  1    2   2
2  2   10  10
3  2   20  20
```
  If series_identifier is not specified, simple plot is generated where every 'y' is plotted against 'x' in a
  single plot. However, specifying series_identifier as the column 'ID' generates two plots in a single axis:
  One plot is for' ID' 1 and another plot is for 'ID' 2.
* title: Specifies the title for the Axis.
* xlabel: Specifies the label for x-axis.
  * When set to empty string, label is not displayed for x-axis.
  * When set to None, name of the x-axis column is displayed as label.
* xlim: Specifies the range for xtick values. It is a tuple.
* xtick_format: Specifies how to format tick values for x-axis.
* ylabel: Specifies the label for y-axis.
  * When set to empty string, label is not displayed for y-axis.
  * When set to None, name of the y-axis columns are displayed as label.
* ylim: Specifies the range for ytick values. It is a tuple.
* ytick_format: Specifies how to format tick values for y-axis.
* vmin: Specifies the lower range of the colormap. By default, the range is derived from data and color
  codes are assigned accordingly.
  Note:
    This argument is significant only for Geometry Plot.
* vmax: Specifies the upper range of the color map. By default, the range is derived from data and color
  codes are assigned accordingly.
  Note:
    This argument is significant only for Geometry Plot.
  For example, assuming you want to use the 'matter' colormap and derive the colors for values which
  are in between 1 and 100.
  Note:
    The 'matter' colormap starts with pale yellow and ends with violet.
    * If vmin and vmax are not specified, then range is derived from existing values. Thus, colors
    are represented in the whole range as follows :
    1 as pale yellow.
    100 as violet.
    * If vmin and vmax are specified as -100 and 100, the value 1 is in the middle of the specified
    range. Thus, colors are represented in the whole range as follows:
    -100 as pale yellow.
    1 as orange.
    100 as violet.
#### Example 1: Create an Axis with marker as 'Pentagon'
```python
>>> from teradataml import Axis
>>> ax = Axis(marker="p")
```
#### Example 2: Create an Axis without x-tick and y-tick but have grid in the format
#### of '-.'
```python
>>> from teradataml import Axis
>>> ax = Axis(xtick_format="", ytick_format="", grid_linestyle="-.")
```
#### Example 3: Create an Axis plotting only for the values between -10 to 100
#### on x-axis
```python
>>> from teradataml import Axis
>>> ax = Axis(xlim=(-10, 100))
```
#### Example 4: Create an Axis displaying legend at upper left corner, and without
#### x and y axis labels
```python
>>> from teradataml import Axis
>>> ax = Axis(legend_style="upper left", xlabel="", ylabel="")
```
#### Example 5: Create an Axis to format the y-axis tick values to display up to two
#### decimal points
Use the color 'dark green' for plotting. Consider y-axis data has 5 digit floating numbers.
```python
>>> from teradataml import Axis
>>> ax = Axis(ytick_format="99999.99", color='dark green')
```
### Figure
Use the Figure() function to create a new figure for the plot in teradataml.
Optional Arguments:
* width: Specifies the width of the figure in pixels.
  Valid range: [400, 4096]
  Default value is 640.
* height: Specifies the height of the figure in pixels.
  Valid range: [400, 4096]
  Default value is 480.
  Note:
    The total number of pixels in an output image, that is, the product of width and height must not
    exceed 4,000,000.
* dpi: Specifies the number of dots per inch for the output image.
  Valid range: [72, 300]
Default Value is 100 for png and jpg type images.
* image_type: Specifies the type of output image.
  Permitted Values:
  * 'png'
  * 'jpg'
  * 'svg'
  Default Value is 'png'.
* heading: Specifies the heading for the plot.
* layout: Specifies the layout for the plot.
  This argument has a tuple type. The first element represents rows and the second element
  represents columns.
  Default value is (1,1).
#### Example 1: Create a Figure object with height and width as 500 pixels and 600
#### pixels respectively
```python
>>> from teradataml import Figure
>>> figure = Figure(height=500, width=600)
```
#### Example 2: Create a Figure object with default height and width along with
#### heading as 'Plot Heading'
```python
>>> from teradataml import Figure
>>> figure = Figure(heading="Plot Heading")
```
### Options Available for Plot Method
Use the Plot() method to generate plots on teradataml DataFrame.
Following types of plots are supported, which can be specified using argument kind:
* bar plot
* corr plot
* geometry plot
* line plot
* mesh plot
* scatter plot
* wiggle plot
Geometry plot is generated based on geometry column in teradataml GeoDataFrame. Only the columns
with ST_GEOMETRY type are allowed for generating geometry plot.
* The maximum size for ST_GEOMETRY must be less than or equal to 64000.
* The ST_GEOMETRY shape can be POINT, LINESTRING, and so on. POLYGON allows filling of
  different colors.
Note:
Many of the options available in plot method are also available in Axis and Figure. If Axis or Figure
object is passed for ax and figure respectively, then options related to Axis and Figure are ignored.
Required Arguments:
* y: Specifies GeoDataFrame columns to use for the y-axis data.
  Note:
    * Geometry plot always requires geometry column and corresponding 'weight' column
    in a tuple format. 'weight' column represents the weight of a shape mentioned in
    geometry column.
    * If you do not specify geometry column and specifies Y-Axis as DataFrame Column, then the
    default geometry column is considered for plotting.
Optional Arguments:
* x: Specifies a GeoDataFrame column to use for the x-axis data.
  Note:
    This argument is not significant for geometry plot. It is a required argument for other types
    of plots.
* scale: Specifies a GeoDataFrame column to use for scale data to Wiggle Plot and Mesh Plot.
  Note:
    This argument is significant for wiggle and mesh plots. It is ignored for other types of plots.
* kind: Specifies the type of plot.
  Permitted vales:
  * 'bar'
  * 'corr'
  * 'geometry'
  * 'line'
  * 'mesh'
* 'scatter'
  * 'wiggle'
  Default values is 'geometry'.
* ax: Specifies the axis for the plot.
* cmap: Specifies the name of the colormap to be used for plotting.
  Note:
    This argument is significant only when corresponding type of plot is mesh or geometry. It is
    ignored for other types of plots.
  All the colormaps mentioned in the following URLs are supported.
  * Choosing Colormaps in Matplotlib
  * cmocean: Colormaps for Oceanography
* color: Specifies the color for the plot.
  Note:
    Hexadecimal color codes are not supported.
  Permitted values:
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
  * Colors mentioned in RGB Monitor Colors
  Default value is 'blue'.
* figure: Specifies the figure for the plot.
* figsize: Specifies the size of the figure in a tuple of two elements: The first element represents width
  of the plot image in pixels and the second element represents height of plot image in pixels.
  Default value is (640, 480).
* figtype: Specifies the type of the image to generate.
  Permitted Values:
* 'png'
  * 'jpg'
  * 'svg'
  Default Value is 'png'.
* figdpi: Specifies the number of dots per inch for the output image.
  Valid range: [72, 300]
  Default Value is 100 for png and jpg type images.
* grid_color: Specifies the color of the grid.
  Permitted values:
  * 'blue'
  * 'orange'
  * 'green'
  * 'red'
  * 'purple
  * 'brown'
  * 'pink'
  * 'gray'
  * 'olive'
  * 'cyan'
  * Colors mentioned in RGB Monitor Colors
  Default value is 'gray'.
  Note:
    Hexadecimal color codes are not supported.
* grid_format: Specifies the format for the grid.
* grid_linestyle: Specifies the line style of the grid.
  Permitted values:
  * '-'
  * '–'
  * '-.'
  Default value is '-'.
* grid_linewidth: Specifies the line width of the grid.
  Valid range: [0.5, 10].
  Default value is 0.8.
* heading: Specifies the heading for the plot.
* legend: Specifies the legends for the plot.
* legend_style: Specifies the location for legend to display on Plot image.
  By default, legend is displayed at upper right corner.
  Permitted values:
  * 'upper right'
  * 'upper left'
  * 'lower right'
  * 'lower left'
  * 'right'
  * 'center left
  * 'center right'
  * 'lower center'
  * 'upper center'
  * 'center'
* lineyle: Specifies the line style for the plot.
  Permitted values:
  * '-'
  * '--'
  * '-.'
  * ':'
  Default value is '-'.
* linewidth: Specifies the line width for the plot.
  Valid range: [0.5, 10].
  Default value is 0.8.
* marker: Specifies the type of the marker to be used.
  All the markers mentioned in matplotlib.markers are supported.
* markersize: Specifies the size of the marker.
  Valid range: [1, 20].
  Default value is 6.
* position: Specifies the position of the axis in the Figure.
  This argument accepts a tuple of two elements: The first element represents the row and the second
  element represents column.
  Default value is (1,1).
* span: Specifies the span of the axis in the Figure.
The argument accepts a tuple of two elements: The first element represents the row and the second
  element represents column.
  For example, span of (2, 1) specifies the Axis occupies 2 rows and 1 column in Figure.
  Default value is (1,1).
* reverse_xaxis: Specifies whether to reverse tick values on x-axis or not.
  Default value is False.
* reverse_yaxis: Specifies whether to reverse tick values on y-axis or not.
  Default value is False.
* series_identifier: Specifies the teradataml GeoDataFrame Column which represents the identifier for
  the data. As many plots with distinct "series_identifier" are generated in a single Axis.
  For example, consider the following data in teradataml GeoDataFrame:
```python
ID   x   y
0  1    1   1
1  1    2   2
2  2   10  10
3  2   20  20
```
  If series_identifier is not specified, simple plot is generated where every 'y' is plotted against 'x' in a
  single plot. However, specifying series_identifier as the column 'ID' generates two plots in a single axis:
  One plot is for' ID' 1 and another plot is for 'ID' 2.
* title: Specifies the title for the Axis.
* xlabel: Specifies the label for x-axis.
  * When set to empty string, label is not displayed for x-axis.
  * When set to None, name of the x-axis column is displayed as label.
* xlim: Specifies the range for xtick values. It is a tuple.
* xtick_format: Specifies how to format tick values for x-axis.
* ylabel: Specifies the label for y-axis.
  * When set to empty string, label is not displayed for y-axis.
  * When set to None, name of the y-axis columns are displayed as label.
* ylim: Specifies the range for ytick values. It is a tuple.
* ytick_format: Specifies how to format tick values for y-axis.
* vmin: Specifies the lower range of the colormap. By default, the range is derived from data and color
  codes are assigned accordingly.
  Note:
    This argument is significant only for Geometry Plot.
* vmax: Specifies the upper range of the color map. By default, the range is derived from data and color
  codes are assigned accordingly.
  Note:
    This argument is significant only for Geometry Plot.
  For example, assuming you want to use the 'matter' colormap and derive the colors for values which
  are in between 1 and 100.
  Note:
    The 'matter' colormap starts with pale yellow and ends with violet.
    * If vmin and vmax are not specified, then range is derived from existing values. Thus, colors
    are represented in the whole range as follows :
    1 as pale yellow.
    100 as violet.
    * If vmin and vmax are specified as -100 and 100, the value 1 is in the middle of the specified
    range. Thus, colors are represented in the whole range as follows:
    -100 as pale yellow.
    1 as orange.
    100 as violet.
* wiggle_fill: Specifies whether to fill the wiggle area or not.
  By default (False), the right positive half of the wiggle is not filled. If specified as True, wiggle area
  is filled.
  Note:
    This argument is applicable only for Wiggle Plot.
* wiggle_scale: Specifies the scale of the wiggle.
  By default, the amplitude of wiggle is scaled relative to RMS of the first payload. In certain cases, it can
  lead to excessively large wiggles. Use this argument to adjust the relative size of the wiggle.
  Note:
    This argument is applicable only for Wiggle Plot and Mesh Plot.