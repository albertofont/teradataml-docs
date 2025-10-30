# Working with Geospatial Data

Geospatial information identifies the geographic location of features and boundaries on the planet. Vantage
provides Geospatial types that address the need to store, manage, retrieve, manipulate, and analyze
geospatial information by governmental bodies and commercial enterprises. For example, local governments
might use geospatial data for city planning, traffic management, or accident investigation.
Vantage provides geospatial types to represent geometries with up to three dimensions. Vantage provides
the ST_Geometry, MBB , and MBR data types for creating and manipulating geometric shapes in the
database. ST_Geometry is implemented as a user-defined type (UDT). ST_Geometry can be used as the
data type of a table column to represent most of the geospatial types specified in the standard.
Vantage supports the following types of geometry shapes, which can be stored in a table column of
ST_Geometry data type.
| Type | Description |
| ---- | ----------- |
| ST_Point | 0-dimensional geometry that represents a single location in two-dimensional |
|  | coordinate space. |
| ST_LineString | 1-dimensional geometry usually stored as a sequence of points with a linear |
|  | interpolation between points. |
| ST_Polygon | 2-dimensional geometry consisting of one exterior boundary and zero or more interior |
|  | boundaries, where each interior boundary defines a hole. |
| ST_GeomCollection | Collection of zero or more ST_Geometry values. |
| ST_MultiPoint | 0-dimensional geometry collection where the elements are restricted to ST_ |
|  | Point values. |
| ST_MultiLineString | 1-dimensional geometry collection where the elements are restricted to ST_ |
|  | LineString values. |
| ST_MultiPolygon | 2-dimensional geometry collection where the elements are restricted to ST_ |
|  | Polygon values. |
| GeoSequence | Extension of ST_LineString that can contain tracking information, such as time |
|  | stamps, in addition to geospatial information.GeoSequence is a Teradata extension to |
|  | SQL/MM Spatial. |

teradataml provides following generic functionalities to access and process geospatial information stored
in Vantage:
* teradataml Geometry Types
* teradataml GeoDataFrame
* teradataml GeoDataFrameColumn
                                        15
## teradataml Geometry Types
teradataml provides support to create the following geometry objects:
* Point
* LineString
* Polygon
* MultiPoint
* MultiLineString
* MultiPolygon
* GeometryCollection
* GeoSequence
This allows user to create a geometry object of any type, that can be used for processing teradataml
GeoDataFrame and teradataml GeoDataFrameColumn. These objects can be passed as argument
values or can be used in slice filtering with Geospatial specific methods and properties of teradataml
GeoDataFrame and teradataml GeoDataFrameColumn.
See Teradata Package for Python on VantageCloud Lake for detailed information and examples for
geometry objects.
#### Example: Geometry type generic usage
```python
from teradataml import GeoDataFrame, LineString
# Create a GeoDataFrame on table "sample_streets"
geoDF = GeoDataFrame("sample_streets")
# Create a LineString object.
l1 = LineString([(0, 0), (0, 20), (20, 20)])
# Case 1: Using Geometry type object as argument value for the Geospatial
specific functions.
geoDF[geoDF.streetShape.crosses(l1) < 1E0].select("streetName")
# Case 2: Using Geometry type object in slice filtering.
geoDF[geoDF.streetShape == l1]
```
### Point
This function enables end user to create an object for the single Point using the coordinates. It allows user
to use the same in GeoDataFrame manipulation and processing using any Geospatial function.
Optional argument:
coordinates: Specifies the coordinates of a point. Coordinates must be specified in positional fashion.
* If coordinates are not passed, an object for empty point is created.
* When coordinates are passed, you must pass one of the following:
  * Two values to define a point in 2-dimensions;
  * Three values to define a point in 3-dimensions.
#### Example 1: Create a Point in 2D, using x and y coordinates
```python
>>> from teradataml import Point
>>> p1 = Point(0, 20)
>>> # Print the coordinates.
>>> print(p1.coords)
(0, 20)
>>> # Print the geometry type.
>>> p1.geom_type
'Point'
```
#### Example 2: Create a Point in 3D, using x, y and z coordinates
```python
>>> from teradataml import Point
>>> p2 = Point(0, 20, 30)
>>> # Print the coordinates.
>>> print(p2.coords)
(0, 20, 30)
```
#### Example 3: Create an empty Point
```python
>>> from teradataml import Point
>>> pe = Point()
>>> # Print the coordinates.
>>> print(pe.coords)
EMPTY
```
### LineString
This function enables end user to create an object for the single LineString using the coordinates. It allows
user to use the same in GeoDataFrame manipulation and processing using any Geospatial function.
Optional argument:
coordinates: Specifies the coordinates of a line.
* If coordinates are not passed, an object for empty line is created.
* When coordinates are passed, you must pass a list of the following types:
  * Two-tuple of int or float;
  * Three-tuple of int or float;
  * Point geometry objects;
  * Mix of any of these three types.
#### Example 1: Create a LineString in 2D, using x and y coordinates
```python
>>> from teradataml import Point, LineString
>>> l1 = LineString([(0, 0), (0, 20), (20, 20)])
>>> # Print the coordinates.
>>> print(l1.coords)
[(0, 0), (0, 20), (20, 20)]
>>> # Print the geometry type.
>>> l1.geom_type
'LineString'
```
#### Example 2: Create a LineString in 3D, using x, y and z coordinates
```python
>>> from teradataml import Point, LineString
>>> l2 = LineString([(0, 0, 1), (0, 1, 3), (1, 3, 6), (3, 3, 6), (3, 6, 1), (6,
3, 3), (3, 3, 0)])
>>> # Print the coordinates.
>>> print(l2.coords)
[(0, 0, 1), (0, 1, 3), (1, 3, 6), (3, 3, 6), (3, 6, 1), (6, 3, 3), (3, 3, 0)]
```
#### Example 3: Create a LineString using Point geometry objects
```python
>>> from teradataml import Point, LineString
# Create some Points in 2D, using x and y coordinates.
>>> p1 = Point(0, 20)
>>> p2 = Point(0, 0)
>>> p3 = Point(20, 20)
>>> l3 = LineString([p1, p2, p3])
>>> # Print the coordinates.
>>> print(l3.coords)
[(0, 20), (0, 0), (20, 20)]
```
#### Example 4: Create a LineString using mix of Point geometry objects
#### and coordinates
```python
>>> from teradataml import Point, LineString
>>> p1 = Point(0, 20)
>>> p2 = Point(20, 20)
>>> l4 = LineString([(0, 0), p1, p2, (20, 0)])
>>> # Print the coordinates.
>>> print(l4.coords)
[(0, 0), (0, 20), (20, 20), (20, 0)]
```
#### Example 5: Create an empty LineString
```python
>>> from teradataml import Point, LineString
>>> le = LineString()
>>> # Print the coordinates.
>>> print(le.coords)
EMPTY
```
### Polygon
This function enables end user to create an object for the single Polygon using the coordinates. It allows
user to use the same in GeoDataFrame manipulation and processing using any Geospatial function.
Optional argument:
coordinates: Specifies the coordinates of a polygon.
* If coordinates are not passed, an object for empty polygon is created.
* When coordinates are passed, you must pass a list of the following types:
  * Two-tuple of int or float;
  * Three-tuple of int or float;
  * Point geometry objects;
* Polygon geometry objects;
  * Mix of any of these four types.
#### Example 1: Create a Polygon in 2D, using x and y coordinates
```python
>>> from teradataml import Point, LineString, Polygon
>>> go1 = Polygon([(0, 0), (0, 20), (20, 20), (20, 0), (0, 0)])
>>> # Print the coordinates.
>>> print(go1.coords)
[(0, 0), (0, 20), (20, 20), (20, 0), (0, 0)]
>>> # Print the geometry type.
>>> go1.geom_type
'Polygon'
```
#### Example 2: Create a Polygon in 3D, using x, y and z coordinates
```python
>>> from teradataml import Point, LineString, Polygon
>>> go2 = Polygon([(0, 0, 0), (0, 0, 20), (0, 20, 0), (0, 20, 20), (20, 0, 0),
(20, 0, 20), (20, 20, 0), (20, 20, 20), (0, 0, 0)])
>>> # Print the coordinates.
>>> print(go2.coords)
[(0, 0, 0), (0, 0, 20), (0, 20, 0), (0, 20, 20), (20, 0, 0), (20, 0, 20), (20,
20, 0), (20, 20, 20), (0, 0, 0)]
```
#### Example 3: Create a Polygon using Point geometry objects
```python
>>> from teradataml import Point, LineString, Polygon
# Create Point objects in 2D, using x and y coordinates.
>>> p1 = Point(0, 20)
>>> p2 = Point(0, 0)
>>> p3 = Point(20, 20)
>>> p4 = Point(20, 0)
>>> go3 = Polygon([p1, p2, p3, p4, p1])
>>> # Print the coordinates.
>>> print(go3.coords)
[(0, 20), (0, 0), (20, 20), (20, 0), (0, 0)]
```
#### Example 4: Create a Polygon using LineString geometry objects
```python
>>> from teradataml import Point, LineString, Polygon
# Create LineString objects in 2D, using x and y coordinates.
>>> l1 = LineString([(0, 0), (0, 20), (20, 20)])
>>> l2 = LineString([(20, 0), (0, 0)])
>>> go4 = Polygon([l1, l2])
>>> # Print the coordinates.
>>> print(go4.coords)
[(0, 20), (0, 0), (20, 20), (20, 0), (0, 0)]
```
#### Example 5: Create a Polygon using mix of Point, LineString geometry objects
#### and coordinates
```python
>>> from teradataml import Point, LineStromg, Polygon
>>> p1 = Point(0, 0)
>>> l1 = LineString([p1, (0, 20), (20, 20)])
>>> go5 = Polygon([l1, (20, 0), p1])
>>> # Print the coordinates.
>>> print(go5.coords)
[(0, 0), (0, 20), (20, 20), (20, 0)]
```
#### Example 5: Create an empty Polygon
```python
>>> from teradataml import Point, LineString, Polygon
>>> goe = Polygon()
>>> # Print the coordinates.
>>> print(poe.coords)
EMPTY
```
### MultiPoint
This function enables end user to create an object holding the multiple Point geometry objects. It allows
user to use the same in GeoDataFrame manipulation and processing using any Geospatial function.
Optional argument:
points: Specifies the list of Point objects. If no points are passed, an object for empty MultiPoint is created.
#### Example 1: Create a MultiPoint in 2D, using x and y coordinates
```python
>>> from teradataml import Point, MultiPoint
>>> p1 = Point(0, 0)
>>> p2 = Point(0, 20)
>>> p3 = Point(20, 20)
>>> p4 = Point(20, 0)
>>> go1 = MultiPoint([p1, p2, p3, p4, p1])
>>> # Print the coordinates.
>>> print(go1.coords)
[(0, 0), (0, 20), (20, 20), (20, 0), (0, 0)]
>>> # Print the geometry type.
>>> print(go1.geom_type)
MultiPoint
```
#### Example 2: Create an empty MultiPoint
```python
>>> from teradataml import Point, MultiPoint
>>> poe = MultiPoint()
>>> # Print the coordinates.
>>> print(poe.coords)
EMPTY
```
### MultiLineString
This function enables end user to create an object holding the multiple LineString geometry objects. It
allows user to use the same in GeoDataFrame manipulation and processing using any Geospatial function.
Optional argument:
lines: Specifies the list of LineString objects. If no lines are passed, an object for empty MultiLineString
is created.
#### Example 1: Create a MultiLineString in 2D, using x and y coordinates
```python
>>> from teradataml import LineString, MultiLineString
>>> l1 = LineString([(1, 3), (3, 0), (0, 1)])
>>> l2 = LineString([(1.35, 3.6456), (3.6756, 0.23), (0.345, 1.756)])
>>> go1 = MultiLineString([l1, l2])
>>> # Print the coordinates.
>>> print(go1.coords)
[[(1, 3), (3, 0), (0, 1)], [(1.35, 3.6456), (3.6756, 0.23), (0.345, 1.756)]]
>>> # Print the geometry type.
>>> print(go1.geom_type)
MultiLineString
```
#### Example 2: Create an empty MultiLineString
```python
>>> from teradataml import LineString, MultiLineString
>>> mls = MultiLineString()
>>> # Print the coordinates.
>>> print(mls.coords)
EMPTY
```
### MultiPolygon
This function enables end user to create an object holding the multiple Polygon geometry objects. It allows
user to use the same in GeoDataFrame manipulation and processing using any Geospatial function.
Optional argument:
polygon: Specifies the list of Polygon objects. If no polygons are passed, an object for empty MultiPolygon
is created.
#### Example 1: Create a MultiPolygon in 2D, using x and y coordinates
```python
>>> from teradataml import Polygon, MultiPolygon
>>> po1 = Polygon([(0, 0), (0, 20), (20, 20), (20, 0), (0, 0)])
>>> po2 = Polygon([(0.6, 0.8), (0.6, 20.8), (20.6, 20.8), (20.6, 0.8),
(0.6, 0.8)])
>>> go1 = MultiPolygon([po1, po2])
>>> # Print the coordinates.
>>> print(go1.coords)
[[(0, 0), (0, 20), (20, 20), (20, 0), (0, 0)], [(0.6, 0.8), (0.6, 20.8), (20.6,
20.8), (20.6, 0.8), (0.6, 0.8)]]
>>> # Print the geometry type.
>>> print(go1.geom_type)
MultiPolygon
```
#### Example 2: Create an empty MultiPolygon
```python
>>> from teradataml import Polygon, MultiPolygon
>>> poe = MultiPolygon()
>>> # Print the coordinates.
>>> print(poe.coords)
EMPTY
```
### GeometryCollection
This function enables end user to create an object holding the multiple types of geometry objects. It allows
user to use the same in GeoDataFrame manipulation and processing using any Geospatial function.
Optional argument:
geoms: Specifies a list of different types of geometry objects.
* If no geometry objects are passed, an object for empty GeometryCollection is created.
* When geometry objects are passed, you must pass a list of the following types:
  * Point;
  * LineString;
  * Polygon;
  * MultiPoint;
  * MultiLineString;
  * MultiPolygon;
  * GeometryCollection;
* Mix of any of these seven types.
#### Example 1: Create a GeometryCollection object with all geometry types
```python
>>> from teradataml import Point, LineString, Polygon, MultiPoint,
MultiLineString, MultiPolygon, GeometryCollection
>>> # Create Point objects.
>>> p1 = Point(1, 1)
>>> p2 = Point()
>>> p3 = Point(6, 3)
>>> p4 = Point(10, 5)
>>> p5 = Point()
>>> # Create LineString Objects.
>>> l1 = LineString([(1, 3), (3, 0), (0, 1)])
>>> l2 = LineString([(1.35, 3.6456), (3.6756, 0.23), (0.345, 1.756)])
>>> l3 = LineString()
>>> # Create Polygon Objects.
>>> po1 = Polygon([(0, 0, 0), (0, 0, 20), (0, 20, 0), (0, 20, 20),
(20, 0, 0), (20, 0, 20), (20, 20, 0), (20, 20, 20),
(0, 0, 0)])
>>> po2 = Polygon([(0, 0, 0), (0, 0, 20.435), (0, 20.435, 0),
(0, 20.435, 20.435), (20.435, 0, 0), (20.435, 0, 20.435),
(20.435, 20.435, 0), (20.435, 20.435, 20.435),
(0, 0, 0)])
>>> po3 = Polygon()
>>> # Create MultiPolygon Object.
>>> mpol = MultiPolygon([po1, po3, po2])
>>> # Create MultiLineString Object.
>>> mlin = MultiLineString([l1, l2, l3])
>>> # Create MultiPoint Object.
>>> mpnt = MultiPoint([p1, p2, p3, p4, p5])
>>> # Create a GeometryCollection object.
>>> gc1 = GeometryCollection([p1, p2, l1, l3, po2, po3, po1, mpol, mlin, mpnt])
>>> # Print the coordinates.
>>> print(gc1.coords)
[(1, 1), 'EMPTY', [(1, 3), (3, 0), (0, 1)], 'EMPTY', [(0, 0, 0), (0, 0,
20.435), (0, 20.435, 0), (0, 20.435, 20.435), (20.435, 0, 0), (20.435, 0,
20.435), (20.435, 20.435, 0), (20.435, 20.435, 20.435), (0, 0, 0)], 'EMPTY',
[(0, 0, 0), (0, 0, 20), (0, 20, 0), (0, 20, 20), (20, 0, 0), (20, 0, 20), (20,
20, 0), (20, 20, 20), (0, 0, 0)], [[(0, 0, 0), (0, 0, 20), (0, 20, 0), (0, 20,
20), (20, 0, 0), (20, 0, 20), (20, 20, 0), (20, 20, 20), (0, 0, 0)], 'EMPTY',
[(0, 0, 0), (0, 0, 20.435), (0, 20.435, 0), (0, 20.435, 20.435), (20.435, 0,
0), (20.435, 0, 20.435), (20.435, 20.435, 0), (20.435, 20.435, 20.435), (0,
0, 0)]], [[(1, 3), (3, 0), (0, 1)], [(1.35, 3.6456), (3.6756, 0.23), (0.345,
1.756)], 'EMPTY'], [(1, 1), 'EMPTY', (6, 3), (10, 5), 'EMPTY']]
>>> # Print the geometry type.
>>> print(gc1.geom_type)
GeometryCollection
```
#### Example 2: Create an empty GeometryCollection
```python
>>> gc2 = GeometryCollection()
>>> # Print the coordinates.
>>> print(gc2.coords)
EMPTY
```
### GeoSequence
This function enables end user to create an object holding the LineString geometry objects with tracking
information such as timestamps. It allows user to use the same in GeoDataFrame manipulation and
processing using any Geospatial function.
Optional arguments:
* coordinates: Specifies the coordinates of a Point.
  * If coordinates are not passed, an object for empty line is created.
  * When coordinates are passed, you must pass a list of the following types:
    - Two-tuple of int or float;
    - Three-tuple of int or float;
    - Point geometry objects;
    - Mix of any of these three types.
```python
timestamps: Specifies a list of timestamp values for each coordinate in the following format:
```
  yyyy-mm-dd hh:mi:ss.ms.
  The first timestamp value is associated with the first point, the second timestamp value is associated
  with the second point, and so forth.
Note:
    Total number of timestamp values must match the total number of points in the GeoSequence.
* link_ids: Specifies the list of values for the ID of the link on the road network for a point in
  the GeoSequence.
  The first link ID value is associated with the first point, the second link ID value is associated with the
  second point, and so forth.
  Note:
    Total number of link ID values must match the total number of points in the GeoSequence.
* user_field_count: Specifies the value that represents the number of user field elements for each point
  in the GeoSequence.
  The default value 0 indicates that no user field elements appear after count in the character string.
* user_fields: Specifies the list of user field tuples that represents a value associated with a Point.
  For example, certain tracking systems may associate velocity, direction, and acceleration values with
  each point.
  Note:
    * You must specify count groups of n user field values (where n is the number of points in
    the geosequence).
    * The first group provides the first user field values for each point, the second group provides
    the second user field values for each point, and so forth.
    * Each group can be formed using a tuple.
#### Example 1: Create a GeoSequence with 2D Points and no user fields
```python
>>> from teradataml import Point, GeoSequence
>>> coordinates = [(1, 3), (3, 0), (0, 1)]
>>> timestamps = ["2008-03-17 10:34:03.53", "2008-03-17 10:38:25.21",
"2008-03-17 10:41:41.48"]
>>> link_ids = [1001, 1002, 1003]
>>> gs1 = GeoSequence(coordinates=coordinates,
timestamps=timestamps, link_ids=link_ids)
>>> gs1.coords
[(1, 3), (3, 0), (0, 1)]
>>> str(gs1)
'GeoSequence((1 3, 3 0, 0 1), (2008-03-17 10:34:03.53, 2008-03-17 10:38:25.21,
2008-03-17 10:41:41.48), (1001, 1002, 1003), (0))
```
#### Example 2: Create a GeoSequence with 3D Points and 2 user fields
Note:
Coordinates can be provided as tuple of ints or floats, or Point objects.
```python
>>> from teradataml import Point, GeoSequence
>>> p1 = (3, 0, 6)
>>> coordinates = [(1, 3, 6), p1, (6, 0, 1)]
>>> timestamps = ["2008-03-17 10:34:03.53", "2008-03-17 10:38:25.21",
"2008-03-17 10:41:41.48"]
>>> link_ids = [1001, 1002, 1003]
>>> user_fields = [(1, 2), (3, 4), (5, 6)]
>>> gs2 = GeoSequence(coordinates=coordinates, timestamps=timestamps,
link_ids=link_ids, user_field_count=2, user_fields=user_fields)
>>> gs2.coords
[(1, 3, 6), (3, 0, 6), (6, 0, 1)]
>>> str(gs2)
'GeoSequence((1 3 6, 3 0 6, 6 0 1), (2008-03-17 10:34:03.53, 2008-03-17
10:38:25.21, 2008-03-17 10:41:41.48), (1001, 1002, 1003), (2, 1, 2, 3, 4,
5, 6))'
```
#### Example 3: Create an empty GeoSequence
```python
>>> from teradataml import Point, GeoSequence
>>> gs3 = GeoSequence()
>>> # Print the coordinates.
>>> print(gc3.coords)
EMPTY
```
## teradataml GeoDataFrame
User can create a GeoDataFrame on a Teradata table and view containing geospatial data. This allows
user to retrieve, manipulate, process, and analyze geospatial information stored in the table with help of the
GeoDataFrame methods and properties.
Note:
* If GeoDataFrame is created on a table that does not contain geometry data, then exception
    is raised.
* teradataml GeoDataFrame extends the teradataml DataFrame that allows user to access and use
    teradataml DataFrame properties and methods.
* Methods and Properties of teradataml GeoDataFrame will return teradataml GeoDataFrame,
    if the result contains geometry data. If result does not contain geometry data, then it returns
    teradataml DataFrame.
* Access Geospatial Data on Vantage
* GeoDataFrame Properties
* GeoDataFrame Methods
### Access Geospatial Data on Vantage
teradataml provides GeoDataFrame a couple ways to access a table containing Geospatial data in
Vantage and create a GeoDataFrame object from the same.
#### Create GeoDataFrame object from a table
Load the necessary teradataml modules first.
```python
from teradataml import GeoDataFrame
```
To access a table containing Geospatial data, user can use GeoDataFrame constructor. For example:
```python
geoDF = GeoDataFrame("sample_shapes")
```
Or user can create a GeoDataFrame using from_table() API. For example:
```python
geoDF = GeoDataFrame.from_table("sample_shapes")
```
#### Create GeoDataFrame object from a query
Load the necessary teradataml modules first.
```python
from teradataml import GeoDataFrame
```
To create a GeoDataFrame from query user must use from_query() API. For example:
```python
geoDF = GeoDataFrame.from_query("select * from sample_shapes")
```
### GeoDataFrame Properties
The following tables list properties of teradataml GeoDataFrame. For more details and examples, see
Teradata Package for Python Function Reference.
#### Generic Usage
```python
from teradataml import GeoDataFrame
geodf = GeoDataFrame("sample_shapes")
geodf.name_of_the_property
```
#### Properties inherited from teradataml DataFrame
| Property | Purpose | Return | Example |
| -------- | ------- | ------ | ------- |

| List containing |  |
| --------------- | - |
|  | >>> load_ |
| column names |  |
|  | example_data("geodataframe", |
|  | "sample_streets") |
|  | >>> df = GeoDataFrame.from_ |
|  | table('sample_streets') |
|  | >>> df.columns |

| teradataml |  |
| ---------- | - |
|  | >>> load_ |
| GeoDataFrame |  |
|  | example_data("geodataframe", |
|  | "sample_streets") |
|  | >>> geo_dataframe |
|  | = GeoDataFrame.from_ |
|  | table('sample_streets') |
|  | >>> geo_dataframe = |

columns Get the column names
    of GeoDataFrame.
dtypes Return a MetaData
    containing the column
    names and types.
              MetaData
              containing the
              column names and
              Python types
                          >>> load_
                          example_data("geodataframe",
                          "sample_streets")
                          >>> df = GeoDataFrame.from_
                          table('sample_streets')
                          >>> df.dtypes
iloc Access a group of
    rows and columns by
    integer values or a
    boolean array.
| Property | Purpose | Return | Example |
| -------- | ------- | ------ | ------- |

| teradataml |  |
| ---------- | - |
| GeoDataFrame |  |
|  | >>> load_example_ |
|  | data("geodataframe", ["sample_ |
|  | shapes"]) |
|  | >>> geo_dataframe = |
|  | GeoDataFrame("sample_shapes") |
|  | >>> geo_dataframe = |
|  | geo_dataframe.select(['skey', |
|  | 'linestrings', 'polygons']) |
|  | >>> geo_dataframe.loc[1004] |
|  | >>> geo_dataframe.loc[[1004, |
|  | 1010]] |
|  | >>> geo_dataframe.loc[1004, |
|  | 'linestrings'] |
|  | >>> geo_dataframe.loc[(1004, |
|  | 'linestrings')] |
|  | >>> geo_dataframe.loc[1001:1004, |
|  | 'skey':'linestrings'] |
|  | >>> geo_dataframe.loc[:, :] |
|  | >>> geo_dataframe.loc[geo_ |
|  | dataframe['skey'] > 1005] |
|  | >>> geo_dataframe.loc[geo_ |
|  | dataframe['skey'] > 1005, |
|  | ['skey', 'linestrings']] |
|  | >>> geo_dataframe.loc[geo_ |
|  | dataframe['skey'] == 1005, |
|  | 'skey':'polygons'] |
|  | >>> geo_dataframe.loc[geo_ |

                          geo_dataframe.select(['skey',
                          'points', 'linestrings'])>>>
                          geo_dataframe.iloc[1]>>> geo_
                          dataframe.iloc[[1, 2]]>>> geo_
                          dataframe.iloc[5, 1]
                          >>> geo_dataframe.iloc[(5, 1)]
                          >>> geo_dataframe.iloc[1:5, 2]
                          >>> geo_dataframe.iloc[1:5, 0:2]
                          >>> geo_dataframe.iloc[:, :]
                          >>> geo_dataframe.iloc[[0, 1,
                          2], [True, False, True]]
index Return the index_label
    of the teradataml
    GeoDataFrame.
              str or List of Strings
              (str) representing
              the index_label of
              the
              GeoDataFrame
                          >>> load_
                          example_data("geodataframe",
                          "sample_cities")
                          >>> df =
                          GeoDataFrame("sample_cities")
                          >>> df.index
                          >>> df = df.set_index(['city_
                          shape', 'city_name'])
                          >>> df
loc Access a group of rows
    and columns by labels
    or a boolean array.
| Property | Purpose | Return | Example |
| -------- | ------- | ------ | ------- |

| Property | Purpose | Return | Example |
| -------- | ------- | ------ | ------- |

| teradataml |  |
| ---------- | - |
|  | >>> load_example_ |
| GeoDataFrameColumn |  |
|  | data("geodataframe", |
|  | ["sample_cities", |
|  | "sample_streets"]) |
|  | >>> cities = |
|  | GeoDataFrame("sample_ |
|  | cities") |
|  | >>> streets = |
|  | GeoDataFrame("sample_ |
|  | streets") |
|  | >>> city_streets = |
|  | cities.join(streets, |
|  | how="cross", lsuffix="l", |

                          dataframe['skey'] == 1005,
                          [True, False, True]]
    Get the underlying
    object name on
    which GeoDataFrame
    is created.
              Str representing
              the object name
              of GeoDataframe.
                          >>> load_
                          example_data("geodataframe",
                          "sample_streets")
                          >>> df =
                          GeoDataFrame('sample_streets')
                          >>> df.db_object_name
shape Return a tuple
    representing the
    dimensionality of
    the GeoDataFrame.
              Tuple representing
              the dimensionality
              of this
              GeoDataFrame.
                          >>> load_
                          example_data("geodataframe",
                          "sample_streets")
                          >>> df =
                          GeoDataFrame('sample_streets')
                          >>> df.shape
size Return a value
    representing the
    number of elements in
    the GeoDataFrame.
              Value representing
              the number of
              elements in the
              GeoDataFrame.
                          >>> load_
                          example_data("geodataframe",
                          "sample_streets")
                          >>> df =
                          GeoDataFrame('sample_streets')
                          >>> df.size
tdtypes Get the teradataml
    GeoDataFrame
    metadata containing
    column names and
    corresponding
    teradatasqlalchemy
    types.
              Metadata
              containing the
              column names and
              Teradata types
                          >>> load_
                          example_data("geodataframe",
                          "sample_streets")
                          >>> df =
                          GeoDataFrame('sample_streets')
                          >>> df.tdtypes
#### Properties specific to Geospatial Data (All Geometry Types)
geometry Return a
    GeoColumnExpression
    for a column containing
    geometry data.
| Property | Purpose | Return | Example |
| -------- | ------- | ------ | ------- |

| GeoDataFrame with |  |
| ----------------- | - |
| result column containing |  |
|  | >>> gdf = |
| Geometry values |  |
|  | GeoDataFrame("sample_ |
|  | shapes") |
|  | >>> gdf = gdf. |
|  | select(["skey", |
|  | "polygons", |
|  | "linestrings"])[gdf. |
|  | skey.isin([1001, 1002, |
|  | 1003])] |
|  | >>> print(gdf.geometry. |
|  | name) |
|  | >>> gdf.centroid |

    Note:
      This property is
      used to run any
      geospatial operation on
      GeoDataFrame, that is,
      any geospatial function
      ran on the geometry
      column referenced by
      this property.
                              rsuffix="r")
                              >>> city_streets.
                              geometry.name
                              >>> city_streets.
                              geometry = city_streets.
                              street_shape
                              >>> city_streets.
                              geometry.name
                              >>> geom_type = city_
                              streets.geometry.geom_
                              type
                              >>> is_simple = city_
                              streets.geometry.is_
                              simple
                              >>> is_valid = city_
                              streets.geometry.is_
                              valid
boundary Return the boundary of
    the Geometry value.
                GeoDataFrame with
                result column containing
                Geometry values
                              >>> gdf =
                              GeoDataFrame("sample_
                              shapes")
                              >>> gdf = gdf.
                              select(["skey",
                              "polygons",
                              "linestrings"])
                              >>> print(gdf.geometry.
                              name)
                              >>> gdf.boundary
centroid Return the mathematical
    centroid of an
    ST_Polygon or ST_
    MultiPolygon value.
convex_
hull
    Return the convex hull of
    the Geometry value.
                GeoDataFrame with
                result column containing
                Geometry values
                              >>> gdf =
                              GeoDataFrame("sample_
                              shapes")
                              >>> gdf = gdf.
                              select(["skey",
                              "polygons",
                              "linestrings"])
                              >>> print(gdf.geometry.
| Property | Purpose | Return | Example |
| -------- | ------- | ------ | ------- |

                              name)
                              >>> gdf.convex_hull
coord_dim Return the coordinate
    dimension of a geometry.
                GeoDataFrame
                Resultant
                column contains:
                * 1, if the input geometry
                is 1D
                * 2, if the input geometry
                is 2D
                * 3, if the input geometry
                is 3D
                              >>> gdf =
                              GeoDataFrame("sample_
                              shapes")
                              >>> gdf = gdf.
                              select(["skey",
                              "polygons",
                              "linestrings"])
                              >>> print(gdf.geometry.
                              name)
                              >>> gdf.coord_dim
dimension Return the dimension of
    the Geometry type.
                GeoDataFrame
                Resultant
                column contains:
                * 0, for a 0-
                dimensional geometry
                * 1, for a 1D geometry
                * 2, for a 2D geometry
                * -1, if the input geometry
                is empty
                              >>> gdf =
                              GeoDataFrame("sample_
                              shapes")
                              >>> gdf = gdf.
                              select(["skey",
                              "polygons",
                              "linestrings"])
                              >>> print(gdf.geometry.
                              name)
                              >>> gdf.dimension
geom_
type
    Return the Geometry
    type of the
    Geometry value.
                GeoDataFrame
                Resultant column
                contains any of the
                following strings:
                * 'ST_Point'
                * 'ST_LineString'
                * 'ST_Polygon'
                * 'ST_MultiPoint'
                * 'ST_MultiLineString'
                * 'ST_MultiPolygon'
                * 'ST_GeomCollection'
                * 'GeoSequence'
                              >>> gdf =
                              GeoDataFrame("sample_
                              shapes")
                              >>> gdf = gdf.
                              select(["skey",
                              "polygons",
                              "linestrings"])
                              >>> print(gdf.geometry.
                              name)
                              >>> gdf.geom_type
is_3D Test if a Geometry value
    has Z coordinate value.
                GeoDataFrame
                Resultant
                column contains:
                * 1, if the Geometry
                contains Z coordinates
                * 0, if the Geometry
                does not contain
                Z coordinates
                              >>> gdf =
                              GeoDataFrame("sample_
                              shapes")
                              >>> gdf = gdf.
                              select(["skey",
                              "polygons",
                              "linestrings"])
                              >>> print(gdf.geometry.
                              name)
                              >>> gdf.is_3D
| Property | Purpose | Return | Example |
| -------- | ------- | ------ | ------- |

is_empty Test if a Geometry
    value corresponds to the
    empty set.
                GeoDataFrame
                Resultant
                column contains:
                * 1, if the geometry
                is empty
                * 0, if the geometry is
                not empty
                              >>> gdf =
                              GeoDataFrame("sample_
                              shapes")
                              >>> gdf = gdf.
                              select(["skey",
                              "polygons",
                              "linestrings"])
                              >>> print(gdf.geometry.
                              name)
                              >>> gdf.is_empty
is_simple Test if a Geometry
    value has no
    anomalous geometric
    points, such as self
    intersection tangency.
                GeoDataFrame
                Resultant
                column contains:
                * 1, if the geometry
                is simple, with no
                anomalous points
                * 0, if the geometry is
                not simple
                              >>> gdf =
                              GeoDataFrame("sample_
                              shapes")
                              >>> gdf = gdf.
                              select(["skey",
                              "polygons",
                              "linestrings"])
                              >>> print(gdf.geometry.
                              name)
                              >>> gdf.is_simple
is_valid Test if a Geometry value
    is well-formed.
                GeoDataFrame
                Resultant
                column contains:
                * 1, if the geometry
                is valid
                * 0, if the geometry is
                not valid
                              >>> gdf =
                              GeoDataFrame("sample_
                              shapes")
                              >>> gdf = gdf.
                              select(["skey",
                              "polygons",
                              "linestrings"])
                              >>> print(gdf.geometry.
                              name)
                              >>> gdf.is_valid
max_x Return the maximum
    X coordinate of a
    Geometry value.
                GeoDataFrame
                Resultant column
                contains a NULL, if the
                Geometry is an empty set.
                              >>> gdf =
                              GeoDataFrame("sample_
                              shapes")
                              >>> gdf = gdf.
                              select(["skey",
                              "polygons",
                              "linestrings"])
                              >>> print(gdf.geometry.
                              name)
                              >>> gdf.max_x
max_y Return the maximum
    Y coordinate of a
    Geometry value.
                GeoDataFrame
                Resultant column
                contains a NULL, if the
                Geometry is an empty set.
                              >>> gdf =
                              GeoDataFrame("sample_
                              shapes")
                              >>> gdf = gdf.
                              select(["skey",
| Property | Purpose | Return | Example |
| -------- | ------- | ------ | ------- |

                              "polygons",
                              "linestrings"])
                              >>> print(gdf.geometry.
                              name)
                              >>> gdf.max_y
max_z Return the maximum
    Z coordinate of a
    Geometry value.
                GeoDataFrame
                Resultant column
                contains a NULL, if the
                Geometry is an empty set.
                              >>> gdf =
                              GeoDataFrame("sample_
                              shapes")
                              >>> gdf = gdf.
                              select(["skey",
                              "polygons",
                              "linestrings"])
                              >>> print(gdf.geometry.
                              name)
                              >>> gdf.max_z
min_x Return the minimum
    X coordinate of a
    Geometry value.
                GeoDataFrame
                Resultant column
                contains a NULL, if the
                Geometry is an empty set.
                              >>> gdf =
                              GeoDataFrame("sample_
                              shapes")
                              >>> gdf = gdf.
                              select(["skey",
                              "polygons",
                              "linestrings"])
                              >>> print(gdf.geometry.
                              name)
                              >>> gdf.min_x
min_y Return the minimum
    Y coordinate of a
    Geometry value.
                GeoDataFrame
                Resultant column
                contains a NULL, if the
                Geometry is an empty set.
                              >>> gdf =
                              GeoDataFrame("sample_
                              shapes")
                              >>> gdf = gdf.
                              select(["skey",
                              "polygons",
                              "linestrings"])
                              >>> print(gdf.geometry.
                              name)
                              >>> gdf.min_y
min_z Return the minimum
    Z coordinate of a
    Geometry value.
                GeoDataFrame
                Resultant column
                contains a NULL, if the
                Geometry is an empty set.
                              >>> gdf =
                              GeoDataFrame("sample_
                              shapes")
                              >>> gdf = gdf.
                              select(["skey",
                              "polygons",
                              "linestrings"])
                              >>> print(gdf.geometry.
                              name)
                              >>> gdf.min_z
| Property | Purpose | Return | Example |
| -------- | ------- | ------ | ------- |

| GeoDataFrame |  |
| ------------ | - |
|  | >>> gdf = |
|  | GeoDataFrame("sample_ |
|  | shapes") |
|  | >>> gdf = gdf. |
|  | select(["skey", |
|  | "polygons", |
|  | "linestrings"]) |
|  | >>> print(gdf.geometry. |
|  | name) |
|  | >>> gdf.srid |

| Property | Purpose | Return | Example |
| -------- | ------- | ------ | ------- |

| Property | Purpose | Return | Example |
| -------- | ------- | ------ | ------- |

srid Get the spatial reference
    system identifier of the
    Geometry value.
#### Properties for Point Geometry
x Get the X
    coordinate of an
    ST_Point value.
            GeoDataFrame
                      >>> gdf = GeoDataFrame("sample_shapes")
                      >>> gdf = gdf.select(["skey", "points",
                      "linestrings"])[gdf.points.geom_type
                      == "ST_Point"]
                      >>> print(gdf.geometry.name)
                      >>> gdf.x
y Get the Y
    coordinate of an
    ST_Point value.
            GeoDataFrame
                      >>> gdf = GeoDataFrame("sample_shapes")
                      >>> gdf = gdf.select(["skey", "points",
                      "linestrings"])[gdf.points.geom_type
                      == "ST_Point"]
                      >>> print(gdf.geometry.name)
                      >>> gdf.y
z Get the Z
    coordinate of an
    ST_Point value.
            GeoDataFrame
                      >>> gdf = GeoDataFrame("sample_shapes")
                      >>> gdf = gdf.select(["skey", "points",
                      "linestrings"])[gdf.points.geom_type
                      == "ST_Point"]
                      >>> print(gdf.geometry.name)
                      >>> gdf.z
#### Properties for LineString Geometry
is_
closed_
3D
    Test whether a
    3D LineString or
    3D MultiLineString
    is closed, taking
    into account the
              GeoDataFrame
              Resultant column contains:
              * 1, if the 3D LineString
              or 3D MultiLineString
              is closed.
                            >>> gdf =
                            GeoDataFrame("sample_
                            shapes")
                            >>> gdf = gdf.select(["skey",
                            "polygons", "linestrings"])
| Property | Purpose | Return | Example |
| -------- | ------- | ------ | ------- |

| Property | Purpose | Return | Example |
| -------- | ------- | ------ | ------- |

| GeoDataFrame |  |
| ------------ | - |
|  | >>> gdf = |
|  | GeoDataFrame("sample_shapes") |
|  | >>> gdf = gdf.select(["skey", |
|  | "polygons", "linestrings"]) |
|  | >>> print(gdf.geometry.name) |
|  | >>> gdf.area |

    Z coordinates in
    the calculation.
              * 0, if the 3D LineString or
              3D MultiLineString is not
              closed or is empty.
                            [gdf.linestrings.is_3D == 1]
                            >>> print(gdf.geometry.name)
                            >>> gdf.geometry = gdf.
                            linestrings
                            >>> gdf.is_closed_3D
is_closed Test if a
    Geometry type
    that represents
    an ST_LineString,
    GeoSequence, or
    ST_MultiLineString
    value is closed.
              GeoDataFrame
              Resultant column contains:
              * 1, if the ST_LineString,
              GeoSequence, or ST_
              LineString components
              of an ST_MultiLineString
              are closed.
              * 0, if the ST_LineString,
              GeoSequence, or ST_
              LineString components
              of an ST_MultiLineString
              are not closed, or if the
              input geometry is empty.
                            >>> gdf =
                            GeoDataFrame("sample_
                            shapes")
                            >>> gdf = gdf.select(["skey",
                            "polygons", "linestrings"])
                            [gdf.skey.isin([1001, 1002,
                            1003])]
                            >>> print(gdf.geometry.name)
                            >>> gdf.geometry = gdf.
                            linestrings
                            >>> gdf.is_closed
is_ring Test if a
    Geometry type
    that represents an
    ST_LineString or
    a GeoSequence
    value is a ring.
              GeoDataFrame
              Resultant column contains:
              * 1, if the ST_LineString
              or GeoSequence value
              is simple (has no
              anomalous geometric
              points, such as self
              intersection tangency)
              and is closed (the
              start point of the ST_
              LineString value is equal
              to the end point)
              * 0, in all other cases.
                            >>> gdf =
                            GeoDataFrame("sample_
                            shapes")
                            >>> gdf = gdf.select(["skey",
                            "polygons", "linestrings"])
                            [~gdf.skey.isin([1009, 1008,
                            1010, 1007])]
                            >>> print(gdf.geometry.name)
                            >>> gdf.geometry = gdf.
                            linestrings
                            >>> gdf.is_ring
#### Properties for Polygon Geometry
area Return the area
    measurement of an
    ST_Polygon or ST_
    MultiPolygon. For ST_
    MultiPolygon, returns
    the sum of the area
    measurements of the
    component polygons.
exterior Get the exterior ring
    of a Geometry type
    that represents an ST_
    Polygon value.
                GeoDataFrame
                with result
                column
                containing ST_
                          >>> gdf =
                          GeoDataFrame("sample_shapes")
                          >>> gdf = gdf.select(["skey",
                          "polygons", "linestrings"])[gdf.
| Property | Purpose | Return | Example |
| -------- | ------- | ------ | ------- |

| GeoDataFrame |  |
| ------------ | - |
|  | >>> gdf = |
|  | GeoDataFrame("sample_shapes") |
|  | >>> gdf = gdf.select(["skey", |
|  | "polygons", "linestrings"]) |
|  | >>> print(gdf.geometry.name) |
|  | >>> gdf.perimeter |

| Method | Method Signature | Purpose | Return |
| ------ | ---------------- | ------- | ------ |

                LineString
                Geometry values
                          skey.isin([1001, 1002, 1003])]
                          >>> print(gdf.geometry.name)
                          >>> gdf.exterior
perimeter Return the boundary
    length of an ST_Polygon,
    or the sum of the
    boundary lengths of the
    component polygons of
    an ST_MultiPolygon.
### GeoDataFrame Methods
The following tables list methods of teradataml GeoDataFrame. For more details and examples, see
Teradata Package for Python Function Reference.
#### Generic Usage: Single Geospatial Function Usage
```python
# Create GeoDataFrame on a table containing Geospatial data.
geoDF = GeoDataFrame("sample_shapes")
# Assumption:
#   geoDF.geometry points to "geom_col1"
# Invoke "intersects" function on GeoDataFrame and pass the argument as
ColumnExpression a.k.a. GeoDataFrameColumn object of a column 'geom_col2'.
geoDF.intersects(geoDF.geom_col2)
#   where,
#       geoDF.geom_col2 is a ColumnExpression, i.e., GeoDataFrameColumn object
for 'geom_col2'.
# Instead of passing GeoDataFrameColumn object for 'geom_col2', user can pass
the column name as is.
geoDF.intersects("geom_col2")
```
#### Methods inherited from teradataml DataFrame
__init__ __init__(self, table_
    name=None,
    index=True, index_
    label=None,
                Constructor for
                teradataml GeoDataFrame.
                                teradataml
                                GeoDataFrame
| Method | Method Signature | Purpose | Return |
| ------ | ---------------- | ------- | ------ |

| __getattr__(self, name) | Return an attribute of |
| ----------------------- | ---------------------- |
|  | the GeoDataFrame. |

| __getitem__(self, key) | Return a column from |  |
| ---------------------- | -------------------- | - |
|  | the GeoDataFrame or filter |  |
|  | the GeoDataFrame using |  |
|  | an expression. |  |
|  | The following operators |  |
|  | are supported: |  |
|  | * comparison: ==, !=, <, <=, >, >= |  |
|  | * boolean: & (and), | (or), ~ (not), |  |
|  |  | ^ (xor) |
|  | Operands can be Python |  |
|  | literals and instances |  |
|  | of ColumnExpressions from |  |
|  | the GeoDataFrame. |  |

| alias | alias(self, alias_name) | Method to create an aliased |
| ----- | ----------------------- | --------------------------- |
|  |  | teradataml GeoDataFrame. |

    query=None,
    materialize=False)
from_
table
    from_table(cls, table_
    name, index=True,
    index_label=None)
                Class method for creating a
                GeoDataFrame from a table or
                a view.
                                teradataml
                                GeoDataFrame
from_
query
    from_query(cls, query,
    index=True, index_
    label=None,
    materialize=False)
                Class method for creating a
                GeoDataFrame using a query.
                                teradataml
                                GeoDataFrame
__
getattr__
                                Return the value of the
                                named attribute of object
                                (if found).
__
getitem__
                                GeoDataFrame or
                                ColumnExpression
                                instance
                Note:
                Teradata recommends using
                this method before performing
                self join using GeoDataFrame's
                join() API.
                                teradataml
                                GeoDataFrame
assign assign(self, drop_
    columns =
    False, **kwargs)
                Assign new columns to a
                teradataml GeoDataFrame.
                                teradataml
                                GeoDataFrame
                                A new GeoDataFrame is
                                returned with:
                                * New columns in
                                  addition to all
                                  the existing columns
                                  if "drop_columns"
                                  is False.
                                * Only new columns if
                                  "drop_columns" is True.
                                * New columns in
                                  addition to group
                                  by columns, i.e.,
| Method | Method Signature | Purpose | Return |
| ------ | ---------------- | ------- | ------ |

| Removes rows with null values. | teradataml |
| ------------------------------ | ---------- |
|  | GeoDataFrame |

| get | get(self, key) | Retrieve required columns from |
| --- | -------------- | ------------------------------ |
|  |  | GeoDataFrame using column |
|  |  | name(s) as key. |
|  |  | Return a new teradataml |
|  |  | GeoDataFrame with requested |
|  |  | columns only. |

                                  columns used for
                                  grouping, if assign() is
                                  run on GeoDataFrame.
                                  groupby().
concat concat(self, other,
    join='OUTER', allow_
    duplicates=True,
    sort=False,
    ignore_index=False)
                Concatenates two teradataml
                GeoDataFrames along the
                index axis.
                                teradataml
                                GeoDataFrame
drop drop(self, labels=None,
    axis=0, columns=None)
                Drop specified labels from rows
                or columns.
                                teradataml
                                GeoDataFrame
dropna dropna(self, how='any',
    thresh=None,
    subset=None)
filter filter(self, items = None,
    like = None, regex =
    None, axis = 1, **kw)
                Filter rows or columns of
                GeoDataFrame according to
                labels in the specified index.
                The filter is applied to the columns
                of the index when axis is set
                to 'rows'.
                                teradataml
                                GeoDataFrame
                                teradataml
                                GeoDataFrame
get_
values
    get_values(self, num_
    rows = 99999)
                Retrieve all values (only) present
                in a teradataml GeoDataFrame.
                Values are retrieved as per a
                numpy.ndarray representation of a
                teradataml GeoDataFrame.
                This format is equivalent to the
                get_values() representation of a
                Pandas GeoDataFrame.
                                Numpy.ndarray
                                representation of a
                                teradataml
                                GeoDataFrame
groupby groupby(self,
    columns_expr)
                Apply GroupBy to one
                or more columns of a
                teradataml GeoDataFrame.
                The result will always behaves
                like calling groupby with
                as_index=False
                in pandas.
                                teradataml
                                DataFrameGroupBy
                                Object
head head(self, n=display.
    max_rows)
                Print the first n rows of the sorted
                teradataml GeoDataFrame.
                                teradataml
                                GeoDataFrame
| Method | Method Signature | Purpose | Return |
| ------ | ---------------- | ------- | ------ |

| keys | keys(self) | Get the column names of |
| ---- | ---------- | ----------------------- |
|  |  | a GeoDataFrame. |

info info(self, verbose=True,
    buf=None, max_
    cols=None,
    null_counts=False)
                Print a summary of
                the GeoDataFrame.
                                None
join join(self, other, on=None,
    how="left", lsuffix=None,
    rsuffix=None)
                Join two different teradataml
                GeoDataFrames together based
                on column comparisons specified
                in argument 'on' and type of join is
                specified in the argument 'how'.
                                teradataml
                                GeoDataFrame
                                List containing the
                                column names
merge merge(self, right,
    on=None, how="inner",
    left_on=None, right_
    on=None, use_
    index=False,
    lsuffix=None,
    rsuffix=None)
                Merge two teradataml
                GeoDataFrames together.
                                teradataml
                                GeoDataFrame
sample sample(self, n = None,
    frac = None, replace
    = False, randomize =
    False, case_when_then
    = None, case_else
    = None)
                Allow to sample few rows from
                GeoDataFrame directly or based
                on conditions.
                Create a new column 'sampleid'
                which has a unique id for each
                sample sampled, it helps to
                uniquely identify each sample.
                                teradataml
                                GeoDataFrame
select select(self,
    select_expression)
                Select required columns
                from GeoDataFrame using
                an expression.
                Return a new teradataml
                GeoDataFrame with selected
                columns only.
                                teradataml
                                GeoDataFrame
                                /DataFrame
set_index set_index(self, keys, drop
    = True, append = False)
                Assign one or more existing
                columns as the new index to a
                teradataml GeoDataFrame.
                                teradataml
                                GeoDataFrame
show_
query
    show_query(self, full_
    query = False)
                Return underlying SQL for the
                teradataml GeoDataFrame. It
                is the same SQL that is
                used to view the data for a
                teradataml GeoDataFrame.
                                String
sort sort(self, columns,
    ascending=True)
                Get sorted data by one
                or more columns in either
                ascending or descending order for
                a GeoDataFrame.
                                teradataml
                                GeoDataFrame
sort_
index
    sort_index(self, axis=0,
    ascending=True,
    kind='quicksort')
                Gets sorted object by labels
                (along an axis) in either ascending
                                teradataml
                                GeoDataFrame
| Method | Method Signature | Purpose | Return |
| ------ | ---------------- | ------- | ------ |

| to_csv | to_csv | Get the data exported to CSV file. | None |
| ------ | ------ | ---------------------------------- | ---- |

                or descending order for a
                teradataml GeoDataFrame.
squeeze squeeze(self,
    axis=None)
                Squeeze one-dimensional axis
                objects into scalars.
                * teradataml GeoDataFrames
                with a single element are
                squeezed to a scalar.
                * teradataml GeoDataFrames
                with a single column are
                squeezed to a Series.
                * Otherwise the object
                is unchanged.
                                teradataml
                                GeoDataFrame,
                                teradataml Series,
                                or scalar,
                                the projection after
                                squeezing 'axis' or all
                                the axes.
tail tail(self, n=display.
    max_rows)
                Print the last n rows of the sorted
                teradataml GeoDataFrame.
                                teradataml
                                GeoDataFrame
to_
pandas
    to_pandas(self, index_
    column=None, num_
    rows=99999, all_
    rows=False,
    fastexport=False, catch_
    errors_warnings=False,
    **kwargs)
                Return a Pandas GeoDataFrame
                for the corresponding teradataml
                GeoDataFrame Object.
                                When "catch_errors_
                                warnings" is set to True
                                and if protocol used for
                                data transfer is fastexport,
                                then the function returns a
                                tuple containing:
                                * Pandas
                                  GeoDataFrame.
                                * Errors, if any, thrown
                                  by fastexport in a list
                                  of strings.
                                * Warnings, if any, thrown
                                  by fastexport in a list
                                  of strings.
                                Only Pandas
                                GeoDataFrame
                                otherwise.
to_sql to_sql(self, table_
    name, if_exists='fail',
    primary_index=None,
    temporary=False,
    schema_name=None,
    types = None, primary_
    time_index_name=None,
    timecode_column=None,
    timebucket_
    duration=None,
    timezero_date=None,
    columns_list=None,
    sequence_
    column=None,
    seq_max=None,
    set_table=False)
                Write records stored in a
                teradataml GeoDataFrame to
                Teradata Vantage.
                                None
#### Methods specific to Geospatial Data (All Geometry Types)
| Method | Method Signature | Purpose | Return |
| ------ | ---------------- | ------- | ------ |
| buffer | buffer(self, distance) | Return all points whose |  |
|  |  | distance from a Geometry |  |
|  |  | value is less than or equal to a |  |
|  |  | specified distance. |  |

| envelope | envelope(self) | Return the bounding rectangle |
| -------- | -------------- | ----------------------------- |
|  |  | for the Geometry value. |

                              teradataml GeoDataFrame
contains contains(self,
      geom_column)
                Test if a Geometry value
                spatially contains another
                Geometry value.
                              GeoDataFrame
                              Resultant column contains:
                              * 1, if the input geometry
                                contains other geometry
                              * 0, if the input geometry does
                                not contain other geometry
crosses crosses(self,
      geom_column)
                Test if a Geometry value
                spatially crosses another
                Geometry value.
                              GeoDataFrame
                              Resultant column contains:
                              * 1, if the geometries cross
                              * 0, if the geometries do
                                not cross
difference difference(self,
      geom_column)
                Return a Geometry value
                that represents the point
                set difference of two
                Geometry values.
                              teradataml GeoDataFrame
disjoint disjoint(self,
      geom_column)
                Test if a Geometry value is
                spatially disjoint from another
                Geometry value.
                              GeoDataFrame
                              Resultant column contains:
                              * 1, if the geometries are
                                spatially disjoint
                              * 0, if the geometries are not
                                spatially disjoint
distance distance(self,
      geom_column)
                Return the distance between
                two Geometry values.
                              teradataml GeoDataFrame
distance_3D distance_3D(self,
      geom_column)
                Return the distance
                between two Geometry
                values. Function considers Z
                coordinate values, if they exist
                in the geometries passed to it.
                              GeoDataFrame
                              Resultant column contains:
                              * a NULL, if the Geometry is
                                an empty set.
                              * 0, if the two
                                geometries intersect.
                              * a float in all other cases. The
                                distance units are those of
                                the two input geometries.
                              GeoDataFrame with
                              result column containing
                              Geometry values
geom_
equals
      geom_equals(self,
      geom_column)
                Test if a Geometry value
                is spatially equal to another
                Geometry value.
                              GeoDataFrame
                              Resultant column contains:
| Method | Method Signature | Purpose | Return |
| ------ | ---------------- | ------- | ------ |

| mbb | mbb(self) | Return the 3D minimum |
| --- | --------- | --------------------- |
|  |  | bounding box (MBB) that |
|  |  | encloses a 3D Geometry. |

| mbr | mbr(self) | Return the minimum bounding |
| --- | --------- | --------------------------- |
|  |  | rectangle (MBR) of a |
|  |  | Geometry value. |

                              * 1, if the geometries are
                                spatially equal
                              * 0, if the geometries are not
                                spatially equal
intersection intersection(self,
      geom_column)
                Return a Geometry type where
                the value represents the
                point set intersection of two
                Geometry values.
                              teradataml GeoDataFrame
intersects intersects(self,
      geom_column)
                Test if a Geometry value
                spatially intersects another
                Geometry value.
                              GeoDataFrame
                              Resultant column contains:
                              * 1, if the geometries intersect
                              * 0, if the geometries do
                                not intersect
make_2D make_2D(self,
      validate=False)
                Converts a 3D Geometry value
                to a 2D Geometry value, and
                optionally validates that the 2D
                geometry is well-formed. This
                method strips the z coordinate
                from 3D geometries.
                              teradataml GeoDataFrame
                              teradataml GeoDataFrame
                              teradataml GeoDataFrame
overlaps overlaps(self,
      geom_column)
                Test if a Geometry value
                spatially overlaps another
                Geometry value.
                              GeoDataFrame
                              Resultant column contains:
                              * 1, if the geometries overlap
                              * 0, if the geometries do
                                not overlap
relates relates(self, geom_
      column, amatrix)
                Test if a Geometry value is
                spatially related to another
                Geometry value.
                              GeoDataFrame
                              Resultant column contains:
                              * 1, if the spatial
                                relationship between the
                                geometries corresponds
                                to one of the acceptable
                                values represented
                                by "amatrix"
                              * 0, if the spatial relationship
                                between the geometries
                                does not correspond to one
                                of the acceptable values
                                represented by "amatrix"
| Method | Method Signature | Purpose | Return |
| ------ | ---------------- | ------- | ------ |
| set_srid | set_srid(self, srid) | Set the spatial reference |  |
|  |  | system identifier of the |  |
|  |  | Geometry value. |  |

| to_binary | to_binary(self) | Return the well-known binary |
| --------- | --------------- | ---------------------------- |
|  |  | (WKB) representation of a |
|  |  | Geometry value. |

| to_text | to_text(self) | Return the well-known text |
| ------- | ------------- | -------------------------- |
|  |  | (WKT) representation of a |
|  |  | Geometry value. |

                              GeoDataFrame
                              Resultant column contains the
                              Geometry with the spatial
                              reference system identifier
                              set to the specified spatial
                              reference system.
simplify simplify(self,
      tolerance)
                Simplify a geometry by
                removing points that would
                fall within a specified
                distance tolerance.
                              teradataml GeoDataFrame
sym_
difference
      sym_difference(self,
      geom_column)
                Return a Geometry value
                that represents the point set
                symmetric difference of two
                Geometry values.
                              teradataml GeoDataFrame
                              GeoDataFrame with
                              result column containing
                              BLOB value
                              GeoDataFrame with
                              result column containing
                              CLOB value
touches touches(self,
      geom_column)
                Test if a Geometry value
                spatially touches another
                Geometry value.
                              GeoDataFrame
                              Resultant column contains:
                              * 1, if the geometries touch
                              * 0, if the geometries do
                                not touch
transform transform(self, to_
      wkt_srs, from_wkt_
      srs, to_srsid=-12345)
                Return a Geometry value
                transformed to the specified
                spatial reference system.
                              teradataml GeoDataFrame
union union(self,
      geom_column)
                Return a Geometry value that
                represents the point set union
                of two Geometry values.
                              teradataml GeoDataFrame
within within(self,
      geom_column)
                Test if a Geometry value
                is spatially within another
                Geometry value.
                              GeoDataFrame
                              Resultant column contains:
                              * 1, if the geometry is within
                                other geometry
                              * 0, if the geometry is not
                                within other geometry
wkb_geom_
to_sql
      wkb_geom_to_
      sql(self, column)
                Return a specified
                Geometry value.
                              teradataml GeoDataFrame
wkt_geom_
to_sql
      wkt_geom_to_
      sql(self, column)
                Return a specified
                Geometry value.
                              teradataml GeoDataFrame
#### Methods for Point Geometry
| Method | Method Signature | Purpose | Return |
| ------ | ---------------- | ------- | ------ |

| set_x | set_x(self, xcoord) | Set the X coordinate of an ST_ |
| ----- | ------------------- | ------------------------------ |
|  |  | Point value. |

| set_y | set_y(self, ycoord) | Set the Y coordinate of an ST_ |
| ----- | ------------------- | ------------------------------ |
|  |  | Point value. |

| set_z | set_z(self, zcoord) | Set the Z coordinate of an ST_ |
| ----- | ------------------- | ------------------------------ |
|  |  | Point value. |

|  | Method |  |  |
| - | ------ | - | - |
| Method |  | Purpose | Return |
|  | Signature |  |  |

spherical_
buffer
      spherical_buffer(self,
      distance, radius=6371000.0)
                    Return an MBR that represents the
                    minimum and maximum latitude
                    and longitude of all points within
                    a given distance from a point. The
                    earth is modeled as a sphere.
                                    teradataml
                                    GeoDataFrame
spherical_
distance
      spherical_distance(self,
      geom_column)
                    Return the spherical distance
                    between two spherical coordinates
                    on the planet using the Haversine
                    Formula. Both coordinates must
                    be specified as ST_Point values.
                                    teradataml
                                    GeoDataFrame
spheroidal_
buffer
      spheroidal_buffer(self,
      distance,
      semimajor=6378137.0,
      invflattening=298.
      257223563)
                    Return an MBR that represents the
                    minimum and maximum latitude
                    and longitude of all points within a
                    given distance from the point. The
                    earth is modeled as a spheroid.
                                    teradataml
                                    GeoDataFrame
spheroidal_
distance
      spheroidal_distance(self,
      geom_column,
      semimajor=6378137.0,
      invflattening=298.
      257223563)
                    Return the distance, in
                    meters, between two
                    spherical coordinates.
                                    teradataml
                                    GeoDataFrame
                                    teradataml
                                    GeoDataFrame
                                    teradataml
                                    GeoDataFrame
                                    teradataml
                                    GeoDataFrame
#### Methods for LineString Geometry
end_point end_point(self) Return the end point of an ST_
              LineString or GeoSequence value.
                                GeoDataFrame
                                Resultant column contains
                                a NULL, if the Geometry is
                                an empty set.
length length(self) Return the length measurement of
              a Geometry type that represents an
              ST_LineString, GeoSequence, or ST_
              MultiLineString value.
                                teradataml
                                GeoDataFrame
length_3D length_3D(self) Return the length of a 3D LineString or
              MultiLineString, taking into account the
              Z coordinates in the calculation.
                                GeoDataFrame
                                Resultant column contains
                                a 0, if the LineString
|  | Method |  |  |
| - | ------ | - | - |
| Method |  | Purpose | Return |
|  | Signature |  |  |

| Method | Method Signature | Purpose | Return |
| ------ | ---------------- | ------- | ------ |

                                or MultiLineString is an
                                empty set.
line_
interpolate_
point
      line_
      interpolate_
      point(self,
      proportion)
              Return a point interpolated along a
              Geometry type that represents an
              ST_LineString or GeoSequence value,
              given a proportional distance along
              that line.
                                GeoDataFrame with result
                                column containing ST_
                                Point Geometry values
num_points num_
      points(self)
              Return the number of points in a
              Geometry type that represents an ST_
              LineString or GeoSequence value.
                                GeoDataFrame
                                Resultant column contains
                                a NULL, if the Geometry is
                                an empty set.
point point(self,
      position)
              Return the specified point from a
              Geometry type that represents an ST_
              LineString or GeoSequence value.
                                teradataml
                                GeoDataFrame
start_point start_point(self) Return the start point of an ST_
              LineString or GeoSequence value.
              Note:
              This method returns a point having
              a Z coordinate if the input geometry
              is three-dimensional.
                                teradataml
                                GeoDataFrame
#### Methods for Polygon Geometry
interiors interiors(self,
      position)
              Return the specified interior ring of a
              Geometry type that represents an ST_
              Polygon value.
                                  GeoDataFrame
                                  with result
                                  column containing
                                  ST_LineString
                                  Geometry values
num_
interior_ring
      num_
      interior_ring(self)
              Return the number of interior rings of a
              Geometry type that represents an ST_
              Polygon value.
                                  teradataml
                                  GeoDataFrame
point_on_
surface
      point_
      on_surface(self)
              Return a point that is guaranteed to
              spatially intersect an ST_Polygon, or at
              least one of the component polygons of an
              ST_MultiPolygon.
                                  teradataml
                                  GeoDataFrame
#### Methods for GeometryCollection Geometry
| Method | Method Signature | Purpose | Return |
| ------ | ---------------- | ------- | ------ |

| num_geometry(self) | Return the number of distinct geometries |
| ------------------ | ---------------------------------------- |
|  | that constitute a composite geometry type |
|  | (ST_GeomCollection, ST_MultiPoint, ST_ |
|  | MultiLineString, or ST_MultiPolygon). |

| Method | Method Signature | Purpose | Return |
| ------ | ---------------- | ------- | ------ |

| get_link | get_link(self, index) | Get the link ID of a specified point in |
| -------- | --------------------- | --------------------------------------- |
|  |  | a GeoSequence. |

geom_
component
      geom_
      component(self,
      position)
                Return the geometry of one component
                member of a composite geometry type
                (ST_GeomCollection, ST_MultiPoint, ST_
                MultiLineString, or ST_MultiPolygon). The
                element to be returned is specified by
                position with the collection.
                                    teradataml
                                    GeoDataFrame
num_
geometry
                                    teradataml
                                    GeoDataFrame
#### Methods for GeoSequence Geometry
clip clip(self, start_
      timestamp,
      end_timestamp)
                Return a GeoSequence type containing
                the subset of points and associated
                data that lie between the two timestamp
                input arguments.
                                    teradataml
                                    GeoDataFrame
get_final_
timestamp
      get_
      final_timestamp(self)
                Return the TimeStamp of the last point of
                a GeoSequence.
                                    teradataml
                                    GeoDataFrame
get_init_
timestamp
      get_
      init_timestamp(self)
                Return the TimeStamp of the first point of
                a GeoSequence.
                                    teradataml
                                    GeoDataFrame
                                    teradataml
                                    GeoDataFrame
get_user_
field
      get_user_field(self,
      field_index, index)
                Return the user field specified by "field_
                index" for the point specified by "index"
                for a GeoSequence type.
                                    teradataml
                                    GeoDataFrame
get_user_
field_count
      get_user_
      field_count(self)
                Return the number of user fields
                associated with each point of
                a GeoSequence.
                                    teradataml
                                    GeoDataFrame
point_
heading
      point_
      heading(self, index)
                Return the heading for the specified point
                of a GeoSequence.
                The value is calculated as the angle (in
                degrees) between a vertical line and the
                line segment from the specified point to
                the next point clockwise from the North.
                                    teradataml
                                    GeoDataFrame
set_link set_link(self, index,
      link_id)
                Set the link ID of a specified point in
                a GeoSequence.
                                    teradataml
                                    GeoDataFrame
speed speed(self,
      index=None, begin_
                Return the approximate speed at a
                specified point (speed(index INTEGER)
                ) or between two points (speed(iBegin
                                    teradataml
                                    GeoDataFrame
| Method | Method Signature | Purpose | Return |
| ------ | ---------------- | ------- | ------ |

|  | Method |  |  |
| - | ------ | - | - |
| Method |  | Purpose | Return |
|  | Signature |  |  |

      index=None,
      end_index=None)
                INTEGER, iEnd INTEGER)) for a
                GeoSequence type.
#### Filtering Functions and Methods
intersects_
mbb
      intersects_
      mbb(self,
      geom_
      column)
            Test whether a
            3D geometry
            spatially intersects
            a specified MBB.
                      GeoDataFrame
                      Resultant column contains:
                      * 1, if the input 3D geometry intersects the
                        MBB argument.
                      * 0, if the input 3D geometry does not intersect the
                        MBB argument.
                      * Return an error if the input geometry is not 3D
                        (does not have a z coordinate).
mbb_filter mbb_
      filter(self,
      geom_
      column)
            Test whether the
            MBBs of two
            3D geometries
            spatially intersect.
                      GeoDataFrame
                      Resultant column contains:
                      * 1, if the MBB of the input 3D geometry intersects
                        the MBB of the geometry passed to the method.
                      * 0, if the MBB of the input 3D geometry does not
                        intersect the MBB of the geometry passed to
                        the method.
                      * Return an error if either geometry is not 3D
                        (does not have a z coordinate).
mbr_filter mbr_filter(self,
      geom_
      column)
            Test whether the
            MBRs of two
            2D geometries
            spatially intersect.
                      GeoDataFrame
                      Resultant column contains:
                      * 1, if the MBR of the input geometry intersects the
                        MBR of the geometry passed to the method.
                      * 0, if the MBR of the input geometry does not
                        intersect the MBR of the geometry passed to
                        the method.
within_mbb within_
      mbb(self,
      geom_
      column)
            Test whether a
            3D geometry is
            spatially within
            the bounds of a
            specified MBB.
                      GeoDataFrame
                      Resultant column contains:
                      * 1, if the input 3D geometry is spatially within
                        MBB argument.
                      * 0, if the input 3D geometry is not spatially within
                        the MBB argument.
                      * Return an error if the input geometry is not 3D
                        (does not have a z coordinate).
## teradataml GeoDataFrameColumn
teradataml GeoDataFrameColumn represents a column of teradataml GeoDataFrame. User can
manipulate, process, and analyze the data using the GeoDataFrameColumn properties and methods.
This provides more flexibility to the user on processing the data.
* Access Individual Geometry Columns
* GeoDataFrameColumn Properties and Methods Specific to Geospatial Data
* GeoDataFrameColumn Properties
* GeoDataFrameColumn Methods
### Access Individual Geometry Columns
GeoDataFrame object can have non-geometry columns and at least one geometry column. Similar
to teradataml DataFrame, teradataml GeoDataFrame allows user to access individual columns in a
using df_object..column_name
The following are two ways to access individual geometry columns in a GeoDataFrame.
#### Access columns similar to teradataml DataFrame column access
A GeoDataFrame can contain multiple columns with Geospatial data.
In the following example, GeoDataFrame contains two columns with geometry type 'geom_col1' and
'geom_col1'. The following shows how to access geometry columns.
* Create GeoDataFrame on a table containing Geospatial data.
```python
geoDF = GeoDataFrame("sample_shapes")
```
* Access column 'geom_col1'.
  Note:
    The following function returns an object of class GeoDataFrameColumn.
```python
geoDF.geom_col1
```
* Access column 'geom_col2'.
```python
geoDF.geom_col2
```
#### Access using 'geometry' property
The most generic way to access the geometry data in any GeoDataFrame is using the 'geometry' property
of a GeoDataFrame. This property always points to a column containing geometry data. Regardless of the
column name, user can access the column with this property.
Note:
* If GeoDataFrame contains multiple columns with Geospatial data, then 'geometry' property
    points to only one of the columns.
    When GeoDataFrame is created, 'geometry' property automatically refers to the first geometry
    column encountered in the GeoDataFrame.
* User is allowed to set the 'geometry' property to point to another column of teradatasqlalchemy
    type Geometry in the same GeoDataFrame.
A GeoDataFrame can contain multiple columns with Geospatial data. In the following example, there
are two columns with geometry type 'geom_col1' and 'geom_col1'. The following shows how to access
geometry column, using 'geometry' property.
* Create GeoDataFrame on a table containing Geospatial data.
```python
geoDF = GeoDataFrame("sample_shapes")
```
* Access the geometry data in GeoDataFrame using 'geometry' property.
  Note:
    This returns an object of class GeoDataFrameColumn for one of the column 'geom_col1'
    or 'geom_col2'.
```python
geoDF.geometry
```
* To identify the name of the column the 'geometry' property points to, use 'name' property
  of GeoDataFrameColumn.
```python
geoDF.geometry.name
```
  If it returns 'geom_col1', that means geoDF.geometry is same as geoDF.geom_col1.
* User can change the geometry column to point to a different column.
  For example, run the following command to point to 'geom_col2'.
```python
geoDF.geometry = "geom_col2"
```
### GeoDataFrameColumn Properties and Methods Specific to
### Geospatial Data
teradataml provides methods and properties for GeoDataFrameColumn.
Note:
* Executing geospatial specific methods and properties always return a teradataml
    GeoDataFrameColumn object.
* Combine these operations with either GeoDataFrame.assign() or GeoDataFrame.[], that is, slice
    filtering to get the desired result.
* Use Case 1: Run a Single Geospatial Function
* Use Case 2: Run Multiple Geospatial Functions
* Use Case 3: Filtering using Geospatial Functions
#### Use Case 1: Run a Single Geospatial Function
To run a single Geospatial function, first create GeoDataFrame on a table containing Geospatial data.
```python
geoDF = GeoDataFrame("sample_shapes")
```
Assume geoDF.geometry points to "geom_col1".
Note:
This approach requires user to use GeoDataFrame.assign() function.
#### Syntax 1: Using 'geometry' property to invoke the function
This syntax uses 'geometry' property of GeoDataFrame to invoke the function.
```python
geoDF.assign(intersect_result = geoDF.geometry.intersects(geoDF.geom_col2))
```
#### Syntax 2: Using actual ColumnExpression of a column to invoke the function
This syntax uses actual ColumnExpression, also known as GeoDataFrameColumn object of a
column 'geom_col1'.
```python
geoDF.assign(intersect_result = geoDF.geom_col1.intersects(geoDF.geom_col2))
```
#### Syntax 3: Passing the column name instead of the GeoDataFrameColumn
#### object as value
This syntax uses the column name as is, instead of passing GeoDataFrameColumn object
for 'geom_col2'.
```python
geoDF.assign(intersect_result = geoDF.geometry.intersects("geom_col2"))
```
#### Use Case 2: Run Multiple Geospatial Functions
There are scenarios when user needs to run multiple functions on single column or multiple columns.
For example, there are two columns containing Geospatial data, 'geom_col1' and 'geom_col2', and you
want to:
* Check if geometry values in 'geom_col1' column intersects with geometry values from column
  'geom_col2' or not. (Use: intersects)
* Check if geometry values in 'geom_col1' column are spatially equal to geometry values from column
  'geom_col2' or not. (Use: geom_Equals)
* Calculate point set symmetric difference between geometry values in columns 'geom_col1' and
  'geom_col2'. (Use: sym_difference)
* Calculate point set difference between geometry values in columns 'geom_col1' and 'geom_col2'.
  (Use: difference)
With GeoDataFrame, several steps are required to get the desired result. With GeoDataFrame execution,
each function is ran individually and then the results of all are joined to get the final result.
With teradataml GeoDataFrameColumn, this can be achieved using a single step to run multiple
Geospatial functions, see Approach 2.
#### Approach 1: Run function individually on GeoDataFrameColumn and pass the
#### results to 'assign()'
```python
# Create GeoDataFrame on a table containing Geospatial data.
geoDF = GeoDataFrame("sample_shapes")
geo_intersect = geoDF.geom_col1.intersects(geoDF.geom_col2)
geo_equals = geoDF.geom_col1.equals("geom_col2")
geo_setdiff = geoDF.geom_col1.difference(geoDF.geom_col2)
geo_symdiff = geoDF.geom_col1.symmetric_difference(geoDF.geom_col2)
geoDF.assign(intersect_result = geo_intersect,
equals_result = geo_equals,
set_diff_result = geo_setdiff,
symmetric_set_result = geo_symdiff)
```
#### Approach 2: Run multiple functions in one step
```python
# Create GeoDataFrame on a table containing Geospatial data.
geoDF = GeoDataFrame("sample_shapes")
# ColumnExpression a.k.a GeoDataFrameColumn object.
geocol1 = geoDF.geom_col1
# This can be achieved using GeoDataFrame.assign() and
GeoDataFrameColumn methods.
geoDF.assign(intersect_result = geocol1.intersects(geoDF.geom_col2),
equals_result = geocol1.equals(geoDF.geom_col2),
set_diff_result = geocol1.difference(geoDF.geom_col2),
symmetric_set_result
= geocol1.symmetric_difference(geoDF.geom_col2),
```
Note:
* User can use 'geoDF.geometry' property to invoke GeoDataFrameColumn method instead of
    'geoDF.geom_col1', if geometry points to 'geom_col1'.
* User can pass column name as string, that is, 'geom_col2', instead of GeoDataFrameColumn
    object of the column.
* In Approach 2, a variable 'geocol1' is created. Creation of the variable is optional. User can
    directly use 'geoDF.geom_col1' instead of 'geo_col1' in assign() function call.
#### Use Case 3: Filtering using Geospatial Functions
User can run Geospatial functions to filter the data.
In the following example, there are two tables: 'sample_cities' and 'sample_streets'. Both contain a
geometry column 'cityShape' and 'streetShape' respectively. The following steps return the street name
and city name, if 'cityShape' contains a 'streetShape'.
```python
# Create GeoDataFrame on tables containing Geospatial data.
cities = GeoDataFrame("sample_cities")
streets = GeoDataFrame("sample_streets")
# First we must perform cross join between the two GeoDataFrames.
city_streets = cities.join(streets, how="cross")
# Apply slice filter and invoke Geospatial function contains in the filter and
use the same as predicate.
city_streets[city_streets.cityShape.contains(city_streets.streetShape) ==
1].select("streetName", "cityName")
# Slice filtering with Geometry Type object passed for comparison.
# Create a LineString object.
line_object = LineString([(2,2), (3,2), (4,1)])
streets[streets.streetShape == line_object]
```
Note:
* User can use 'city_streets.geometry' property to invoke GeoDataFrameColumn method
    instead of 'city_streets.cityShape', if geometry points to 'cityShape'.
* User can pass column name as string, that is, 'streetShape' instead of GeoDataFrameColumn
    object of the column.
### GeoDataFrameColumn Properties
The following tables list properties of teradataml GeoDataFrameColumn. For more details and examples,
see Teradata Package for Python Function Reference.
#### Properties inherited from teradataml DataFrameColumn
| Property | Purpose | Return |
| -------- | ------- | ------ |
| name | Get the name of the GeoDataFrameColumn. | string |
| type | Get the Teradata type of the GeoDataFrameColumn. | teradatasqlalchemy type |

| Property | Purpose | Return |
| -------- | ------- | ------ |

#### Properties specific to Geospatial Data (All Geometry Types)
boundary Return the boundary of the
      Geometry value.
                    GeoDataFrameColumn with result column containing
                    Geometry values
centroid Return the mathematical
      centroid of an ST_Polygon or
      ST_MultiPolygon value.
                    GeoDataFrameColumn with result column containing
                    Geometry values
convex_hull Return the convex hull of the
      Geometry value.
                    GeoDataFrameColumn with result column containing
                    Geometry values
coord_dim Return the coordinate dimension
      of a geometry.
                    GeoDataFrameColumn
                    Resultant column contains:
                    * 1, if the input geometry is 1D
                    * 2, if the input geometry is 2D
                    * 3, if the input geometry is 3D
dimension Return the dimension of the
      Geometry type.
                    GeoDataFrameColumn
                    Resultant column contains:
                    * 0, for a 0-dimensional geometry
| Property | Purpose | Return |
| -------- | ------- | ------ |

                    * 1, for a 1D geometry
                    * 2, for a 2D geometry
                    * -1, if the input geometry is empty
geom_type Return the Geometry type of the
      Geometry value.
                    GeoDataFrameColumn
                    Resultant column contains any of the following strings:
                    * 'ST_Point'
                    * 'ST_LineString'
                    * 'ST_Polygon'
                    * 'ST_MultiPoint'
                    * 'ST_MultiLineString'
                    * 'ST_MultiPolygon'
                    * 'ST_GeomCollection'
                    * 'GeoSequence'
is_3D Test if a Geometry value has Z
      coordinate value.
                    GeoDataFrameColumn
                    Resultant column contains:
                    * 1, if the Geometry contains Z coordinates
                    * 0, if the Geometry does not contain Z coordinates
is_empty Test if a Geometry value
      corresponds to the empty set.
                    GeoDataFrameColumn
                    Resultant column contains:
                    * 1, if the geometry is empty
                    * 0, if the geometry is not empty
is_simple Test if a Geometry value
      has no anomalous geometric
      points, such as self
      intersection tangency.
                    GeoDataFrameColumn
                    Resultant column contains:
                    * 1, if the geometry is simple, with no anomalous points
                    * 0, if the geometry is not simple
is_valid Test if a Geometry value is well-
      formed.
                    GeoDataFrameColumn
                    Resultant column contains:
                    * 1, if the geometry is valid
                    * 0, if the geometry is not valid
max_x Return the maximum X
      coordinate of a Geometry value.
                    GeoDataFrameColumn
                    Resultant column contains a NULL, if the Geometry is
                    an empty set.
max_y Return the maximum Y
      coordinate of a Geometry value.
                    GeoDataFrameColumn
                    Resultant column contains a NULL, if the Geometry is
                    an empty set.
max_z Return the maximum Z
      coordinate of a Geometry value.
                    GeoDataFrameColumn
                    Resultant column contains a NULL, if the Geometry is
                    an empty set.
| Property | Purpose | Return |
| -------- | ------- | ------ |

| Property | Purpose | Return |
| -------- | ------- | ------ |
| x | Get the X coordinate of an ST_Point value. | GeoDataFrameColumn |
| y | Get the Y coordinate of an ST_Point value. | GeoDataFrameColumn |
| z | Get the Z coordinate of an ST_Point value. | GeoDataFrameColumn |

| Property | Purpose | Return |
| -------- | ------- | ------ |

min_x Return the minimum X
      coordinate of a Geometry value.
                    GeoDataFrameColumn
                    Resultant column contains a NULL, if the Geometry is
                    an empty set.
min_y Return the minimum Y
      coordinate of a Geometry value.
                    GeoDataFrameColumn
                    Resultant column contains a NULL, if the Geometry is
                    an empty set.
min_z Return the minimum Z
      coordinate of a Geometry value.
                    GeoDataFrameColumn
                    Resultant column contains a NULL, if the Geometry is
                    an empty set.
srid Get the spatial reference system
      identifier of the Geometry value.
                    GeoDataFrameColumn
#### Properties for Point Geometry
#### Properties for LineString Geometry
is_closed_
3D
      Tests whether a 3D LineString
      or 3D MultiLineString is
      closed, taking into account
      the Z coordinates in
      the calculation.
                  GeoDataFrameColumn
                  Resultant column contains:
                  * 1, if the 3D LineString or 3D MultiLineString is closed.
                  * 0, if the 3D LineString or 3D MultiLineString is not closed
                    or is empty.
is_closed Tests if a Geometry type
      that represents an ST_
      LineString, GeoSequence, or
      ST_MultiLineString value
      is closed.
                  GeoDataFrameColumn
                  Resultant column contains:
                  * 1, if the ST_LineString, GeoSequence, or ST_LineString
                    components of an ST_MultiLineString are closed.
                  * 0, if the ST_LineString, GeoSequence, or ST_LineString
                    components of an ST_MultiLineString are not closed, or
                    if the input geometry is empty.
is_ring Tests if a Geometry type that
      represents an ST_LineString
      or a GeoSequence value is
      a ring.
                  GeoDataFrameColumn
                  Resultant column contains:
                  * 1, if the ST_LineString or GeoSequence value is simple
                    (has no anomalous geometric points, such as self
                    intersection tangency) and is closed (the start point of
                    the ST_LineString value is equal to the end point)
| Property | Purpose | Return |
| -------- | ------- | ------ |

| Property | Purpose | Return |
| -------- | ------- | ------ |

| Method | Method Signature | Purpose | Return |
| ------ | ---------------- | ------- | ------ |
| buffer | buffer(self, distance) | Return all points whose |  |
|  |  | distance from a Geometry |  |
|  |  | value is less than or equal |  |
|  |  | to a specified distance. |  |

                  * 0, in all other cases.
#### Properties for Polygon Geometry
area Return the area measurement of an ST_Polygon or ST_
    MultiPolygon. For ST_MultiPolygon, returns the sum of the
    area measurements of the component polygons.
                              GeoDataFrameColumn
exterior Get the exterior ring of a Geometry type that represents an
    ST_Polygon value.
                              GeoDataFrameColumn with
                              result column containing ST_
                              LineString Geometry values
perimeter Return the boundary length of an ST_Polygon, or the sum
    of the boundary lengths of the component polygons of an
    ST_MultiPolygon.
                              GeoDataFrameColumn
### GeoDataFrameColumn Methods
The following tables list methods of teradataml GeoDataFrameColumn. For more details and examples,
see Teradata Package for Python Function Reference .
#### Methods specific to Geospatial Data (All Geometry Types)
                            teradataml GeoDataFrameColumn
contains contains(self,
      geom_column)
                Test if a Geometry value
                spatially contains another
                Geometry value.
                            GeoDataFrameColumn
                            Resultant column contains:
                            * 1, if the input geometry contains
                              other geometry
                            * 0, if the input geometry does not
                              contain other geometry
crosses crosses(self,
      geom_column)
                Test if a Geometry value
                spatially crosses another
                Geometry value.
                            GeoDataFrameColumn
                            Resultant column contains:
                            * 1, if the geometries cross
                            * 0, if the geometries do not cross
difference difference(self,
      geom_column)
                Return a Geometry value
                that represents the point
                set difference of two
                Geometry values.
                            teradataml GeoDataFrameColumn
| Method | Method Signature | Purpose | Return |
| ------ | ---------------- | ------- | ------ |

| envelope | envelope(self) | Return the bounding |
| -------- | -------------- | ------------------- |
|  |  | rectangle for the |
|  |  | Geometry value. |

disjoint disjoint(self,
      geom_column)
                Test if a Geometry value
                is spatially disjoint from
                another Geometry value.
                            GeoDataFrameColumn
                            Resultant column contains:
                            * 1, if the geometries are
                              spatially disjoint
                            * 0, if the geometries are not
                              spatially disjoint
distance distance(self,
      geom_column)
                Return the distance
                between two
                Geometry values.
                            teradataml GeoDataFrameColumn
distance_3D distance_3D(self,
      geom_column)
                Return the distance
                between two Geometry
                values. Function considers
                Z coordinate values, if they
                exist in the geometries
                passed to it.
                            GeoDataFrameColumn
                            Resultant column contains:
                            * a NULL, if the Geometry is an
                              empty set.
                            * 0, if the two geometries intersect.
                            * a float in all other cases. The
                              distance units are those of the
                              two input geometries.
                            GeoDataFrameColumn with
                            result column containing
                            Geometry values
geom_
equals
      geom_equals(self,
      geom_column)
                Test if a Geometry value is
                spatially equal to another
                Geometry value.
                            GeoDataFrameColumn
                            Resultant column contains:
                            * 1, if the geometries are
                              spatially equal
                            * 0, if the geometries are not
                              spatially equal
intersection intersection(self,
      geom_column)
                Return a Geometry type
                where the value represents
                the point set intersection of
                two Geometry values.
                            teradataml GeoDataFrameColumn
intersects intersects(self,
      geom_column)
                Test if a Geometry value
                spatially intersects another
                Geometry value.
                            GeoDataFrameColumn
                            Resultant column contains:
                            * 1, if the geometries intersect
                            * 0, if the geometries do
                              not intersect
make_2D make_2D(self,
      validate=False)
                Converts a 3D Geometry
                value to a 2D Geometry
                value, and optionally
                validates that the 2D
                geometry is well-formed.
                This method strips
                the z coordinate from
                3D geometries.
                            teradataml GeoDataFrameColumn
| Method | Method Signature | Purpose | Return |
| ------ | ---------------- | ------- | ------ |
| mbb | mbb(self) | Return the 3D minimum |  |
|  |  | bounding box (MBB) that |  |
|  |  | encloses a 3D Geometry. |  |

| mbr | mbr(self) | Return the minimum |
| --- | --------- | ------------------ |
|  |  | bounding rectangle (MBR) |
|  |  | of a Geometry value. |

| set_srid | set_srid(self, srid) | Set the spatial reference |
| -------- | -------------------- | ------------------------- |
|  |  | system identifier of the |
|  |  | Geometry value. |

| to_binary | to_binary(self) | Return the well- |
| --------- | --------------- | ---------------- |
|  |  | known binary (WKB) |
|  |  | representation of a |
|  |  | Geometry value. |

| to_text | to_text(self) | Return the well-known text |
| ------- | ------------- | -------------------------- |
|  |  | (WKT) representation of a |
|  |  | Geometry value. |

                            teradataml GeoDataFrameColumn
                            teradataml GeoDataFrameColumn
overlaps overlaps(self,
      geom_column)
                Test if a Geometry value
                spatially overlaps another
                Geometry value.
                            GeoDataFrameColumn
                            Resultant column contains:
                            * 1, if the geometries overlap
                            * 0, if the geometries do
                              not overlap
relates relates(self, geom_
      column, amatrix)
                Test if a Geometry value is
                spatially related to another
                Geometry value.
                            GeoDataFrameColumn
                            Resultant column contains:
                            * 1, if the spatial
                              relationship between the
                              geometries corresponds
                              to one of the acceptable values
                              represented by "amatrix"
                            * 0, if the spatial relationship
                              between the geometries does
                              not correspond to one of the
                              acceptable values represented
                              by "amatrix"
                            GeoDataFrameColumn
                            Resultant column contains the
                            Geometry with the spatial
                            reference system identifier
                            set to the specified spatial
                            reference system.
simplify simplify(self,
      tolerance)
                Simplify a geometry by
                removing points that would
                fall within a specified
                distance tolerance.
                            teradataml GeoDataFrameColumn
sym_
difference
      sym_difference(self,
      geom_column)
                Return a Geometry value
                that represents the point
                set symmetric difference of
                two Geometry values.
                            teradataml GeoDataFrameColumn
                            GeoDataFrameColumn with result
                            column containing BLOB value
                            GeoDataFrameColumn with result
                            column containing CLOB value
| Method | Method Signature | Purpose | Return |
| ------ | ---------------- | ------- | ------ |

| Method | Method Signature | Purpose | Return |
| ------ | ---------------- | ------- | ------ |

touches touches(self,
      geom_column)
                Test if a Geometry value
                spatially touches another
                Geometry value.
                            GeoDataFrameColumn
                            Resultant column contains:
                            * 1, if the geometries touch
                            * 0, if the geometries do not touch
transform transform(self,
      to_wkt_srs,
      from_wkt_srs,
      to_srsid=-12345)
                Return a Geometry
                value transformed to
                the specified spatial
                reference system.
                            teradataml GeoDataFrameColumn
union union(self,
      geom_column)
                Return a Geometry
                value that represents the
                point set union of two
                Geometry values.
                            teradataml GeoDataFrameColumn
within within(self,
      geom_column)
                Test if a Geometry value
                is spatially within another
                Geometry value.
                            GeoDataFrameColumn
                            Resultant column contains:
                            * 1, if the geometry is within
                              other geometry
                            * 0, if the geometry is not within
                              other geometry
wkb_geom_
to_sql
      wkb_geom_to_
      sql(self, column)
                Return a specified
                Geometry value.
                            teradataml GeoDataFrameColumn
wkt_geom_
to_sql
      wkt_geom_to_
      sql(self, column)
                Return a specified
                Geometry value.
                            teradataml GeoDataFrameColumn
#### Methods for Point Geometry
spherical_
buffer
      spherical_
      buffer(self, distance,
      radius=6371000.0)
                  Return an MBR that
                  represents the minimum
                  and maximum latitude and
                  longitude of all points within
                  a given distance from a point.
                  The earth is modeled as
                  a sphere.
                                teradataml
                                GeoDataFrameColumn
spherical_
distance
      spherical_distance(self,
      geom_column)
                  Return the spherical distance
                  between two spherical
                  coordinates on the planet
                  using the Haversine Formula.
                  Both coordinates must be
                  specified as ST_Point values.
                                teradataml
                                GeoDataFrameColumn
spheroidal_
buffer
      spheroidal_buffer(self,
      distance,
      semimajor=6378137.0,
      invflattening=298.
      257223563)
                  Return an MBR that
                  represents the minimum
                  and maximum latitude and
                  longitude of all points within
                  a given distance from the
                                teradataml
                                GeoDataFrameColumn
| Method | Method Signature | Purpose | Return |
| ------ | ---------------- | ------- | ------ |

| set_x | set_x(self, xcoord) | Set the X coordinate of an |
| ----- | ------------------- | -------------------------- |
|  |  | ST_Point value. |

| set_y | set_y(self, ycoord) | Set the Y coordinate of an |
| ----- | ------------------- | -------------------------- |
|  |  | ST_Point value. |

| set_z | set_z(self, zcoord) | Set the Z coordinate of an |
| ----- | ------------------- | -------------------------- |
|  |  | ST_Point value. |

|  | Method |  |  |
| - | ------ | - | - |
| Method |  | Purpose | Return |
|  | Signature |  |  |

                  point. The earth is modeled
                  as a spheroid.
spheroidal_
distance
      spheroidal_distance(self,
      geom_column,
      semimajor=6378137.0,
      invflattening=298.
      257223563)
                  Return the distance, in
                  meters, between two
                  spherical coordinates.
                                teradataml
                                GeoDataFrameColumn
                                teradataml
                                GeoDataFrameColumn
                                teradataml
                                GeoDataFrameColumn
                                teradataml
                                GeoDataFrameColumn
#### Methods for LineString Geometry
end_point end_point(self) Return the end point of an ST_
              LineString or GeoSequence value.
                              GeoDataFrameColumn
                              Resultant column contains a
                              NULL, if the Geometry is an
                              empty set.
length length(self) Return the length measurement of
              a Geometry type that represents
              an ST_LineString, GeoSequence,
              or ST_MultiLineString value.
                              teradataml
                              GeoDataFrameColumn
length_3D length_
      3D(self)
              Return the length of a
              3D LineString or MultiLineString,
              taking into account the Z
              coordinates in the calculation.
                              GeoDataFrameColumn
                              Resultant column contains a 0, if
                              the LineString or MultiLineString
                              is an empty set.
line_
interpolate_
point
      line_
      interpolate_
      point(self,
      proportion)
              Return a point interpolated along a
              Geometry type that represents an
              ST_LineString or GeoSequence
              value, given a proportional
              distance along that line.
                              GeoDataFrameColumn with
                              result column containing ST_
                              Point Geometry values
num_points num_
      points(self)
              Return the number of points
              in a Geometry type that
              represents an ST_LineString or
              GeoSequence value.
                              GeoDataFrameColumn
                              Resultant column contains a
                              NULL, if the Geometry is an
                              empty set.
point point(self,
      position)
              Return the specified point
              from a Geometry type that
              represents an ST_LineString or
              GeoSequence value.
                              teradataml
                              GeoDataFrameColumn
|  | Method |  |  |
| - | ------ | - | - |
| Method |  | Purpose | Return |
|  | Signature |  |  |

|  | Method |  |  |
| - | ------ | - | - |
| Method |  | Purpose | Return |
|  | Signature |  |  |

| Method | Method Signature | Purpose | Return |
| ------ | ---------------- | ------- | ------ |

start_point start_
      point(self)
              Return the start point of an ST_
              LineString or GeoSequence value.
              Note:
              This method returns a point
              having a Z coordinate if the input
              geometry is three-dimensional.
                              teradataml
                              GeoDataFrameColumn
#### Methods for Polygon Geometry
interiors interiors(self,
      position)
              Return the specified interior ring of
              a Geometry type that represents an
              ST_Polygon value.
                                GeoDataFrameColumn with
                                result column containing ST_
                                LineString Geometry values
num_
interior_
ring
      num_interior_
      ring(self)
              Return the number of interior rings of
              a Geometry type that represents an
              ST_Polygon value.
                                teradataml
                                GeoDataFrameColumn
point_on_
surface
      point_on_
      surface(self)
              Return a point that is guaranteed
              to spatially intersect an ST_Polygon,
              or at least one of the component
              polygons of an ST_MultiPolygon.
                                teradataml
                                GeoDataFrameColumn
#### Methods for GeometryCollection Geometry
geom_
component
      geom_
      component(self,
      position)
              Return the geometry of one
              component member of a composite
              geometry type (ST_GeomCollection,
              ST_MultiPoint, ST_MultiLineString, or
              ST_MultiPolygon). The element to be
              returned is specified by position with
              the collection.
                                teradataml
                                GeoDataFrameColumn
num_
geometry
      num_
      geometry(self)
              Return the number of distinct
              geometries that constitute a
              composite geometry type (ST_
              GeomCollection, ST_MultiPoint, ST_
              MultiLineString, or ST_MultiPolygon).
                                teradataml
                                GeoDataFrameColumn
#### Methods for GeoSequence Geometry
| Method | Method Signature | Purpose | Return |
| ------ | ---------------- | ------- | ------ |

| get_link | get_link(self, index) | Get the link ID of a specified point in |
| -------- | --------------------- | --------------------------------------- |
|  |  | a GeoSequence. |

|  | Method |  |  |
| - | ------ | - | - |
| Method |  | Purpose | Return |
|  | Signature |  |  |

clip clip(self, start_
      timestamp,
      end_timestamp)
                Return a GeoSequence type
                containing the subset of points and
                associated data that lie between the
                two timestamp input arguments.
                                teradataml
                                GeoDataFrameColumn
get_final_
timestamp
      get_
      final_timestamp(self)
                Return the TimeStamp of the last
                point of a GeoSequence.
                                teradataml
                                GeoDataFrameColumn
get_init_
timestamp
      get_
      init_timestamp(self)
                Return the TimeStamp of the first
                point of a GeoSequence.
                                teradataml
                                GeoDataFrameColumn
                                teradataml
                                GeoDataFrameColumn
get_user_
field
      get_user_field(self,
      field_index, index)
                Return the user field specified by
                "field_index" for the point specified
                by "index" for a GeoSequence type.
                                teradataml
                                GeoDataFrameColumn
get_user_
field_count
      get_user_
      field_count(self)
                Return the number of user fields
                associated with each point of
                a GeoSequence.
                                teradataml
                                GeoDataFrameColumn
point_
heading
      point_
      heading(self, index)
                Return the heading for the specified
                point of a GeoSequence.
                The value is calculated as the angle
                (in degrees) between a vertical
                line and the line segment from the
                specified point to the next point
                clockwise from the North.
                                teradataml
                                GeoDataFrameColumn
set_link set_link(self, index,
      link_id)
                Set the link ID of a specified point in
                a GeoSequence.
                                teradataml
                                GeoDataFrameColumn
speed speed(self,
      index=None, begin_
      index=None,
      end_index=None)
                Return the approximate speed
                at a specified point (speed(index
                INTEGER)) or between two
                points (speed(iBegin INTEGER,
                iEnd INTEGER)) for a
                GeoSequence type.
                                teradataml
                                GeoDataFrameColumn
#### Filtering Functions and Methods
intersects_
mbb
      intersects_
      mbb(self,
      geom_
      column)
            Test whether a 3D
            geometry
            spatially
            intersects a
            specified MBB.
                      GeoDataFrameColumn
                      Resultant column contains:
                      * 1, if the input 3D geometry intersects the
                      MBB argument.
|  | Method |  |  |
| - | ------ | - | - |
| Method |  | Purpose | Return |
|  | Signature |  |  |

                      * 0, if the input 3D geometry does not intersect the
                      MBB argument.
                      * Return an error if the input geometry is not 3D
                      (does not have a z coordinate).
mbb_filter mbb_
      filter(self,
      geom_
      column)
            Test whether the
            MBBs of two 3D
            geometries
            spatially intersect.
                      GeoDataFrameColumn
                      Resultant column contains:
                      * 1, if the MBB of the input 3D geometry intersects
                      the MBB of the geometry passed to the method.
                      * 0, if the MBB of the input 3D geometry does
                      not intersect the MBB of the geometry passed to
                      the method.
                      * Return an error if either geometry is not 3D (does
                      not have a z coordinate).
mbr_filter mbr_
      filter(self,
      geom_
      column)
            Test whether the
            MBRs of two 2D
            geometries
            spatially intersect.
                      GeoDataFrameColumn
                      Resultant column contains:
                      * 1, if the MBR of the input geometry intersects the
                      MBR of the geometry passed to the method.
                      * 0, if the MBR of the input geometry does not
                      intersect the MBR of the geometry passed to
                      the method.
within_mbb within_
      mbb(self,
      geom_
      column)
            Test whether a 3D
            geometry is
            spatially within
            the bounds of a
            specified MBB.
                      GeoDataFrameColumn
                      Resultant column contains:
                      * 1, if the input 3D geometry is spatially within
                      MBB argument.
                      * 0, if the input 3D geometry is not spatially within
                      the MBB argument.
                      * Return an error if the input geometry is not 3D
                      (does not have a z coordinate).