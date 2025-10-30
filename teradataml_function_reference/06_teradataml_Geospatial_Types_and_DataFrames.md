# teradataml Geospatial Types and DataFrames

Geometry Types
Point
teradataml.geospatial.geometry_types.Point = class Point(GeometryType)
teradataml.geospatial.geometry_types.Point(*coordinates)
Class Point enables end user to create an object for the single Point
using the coordinates. Allows user to use the same in GeoDataFrame
manipulation and processing.
Method resolution order:
Point
GeometryType
builtins.object
Methods deﬁned here:
__init__(self, *coordinates)
DESCRIPTION:
    Enables end user to create an object for the single Point
    using the coordinates. Allows user to use the same in GeoDataFrame
    manipulation and processing using any Geospatial function.
PARAMETERS:
    *coordinates:
        Optional Argument.
        Specifies the coordinates of a Point. Coordinates must be
        specified in positional fashion.
        If coordinates are not passed, an object for empty point is
        created.
        When coordinates are passed, one must pass either 2 or 3
        values to define a Point in 2-dimentions or 3-dimentions.
        Types: int, float
RETURNS:
    Point
RAISES:
    TeradataMlException, TypeError, ValueError
EXAMPLES:
    >>> from teradataml import Point
    # Example 1: Create a Point in 2D, using x and y coordinates.
    >>> p1 = Point(0, 20)
    >>> # Print the coordinates.
    >>> print(p1.coords)
    (0, 20)
    >>> # Print the geometry type.
    >>> p1.geom_type
    'Point'
    >>>
    # Example 2: Create a Point in 3D, using x, y and z coordinates.
    >>> p2 = Point(0, 20, 30)
    >>> # Print the coordinates.
    >>> print(p2.coords)
    (0, 20, 30)
    >>>
    # Example 3: Create an empty Point.
    >>> pe = Point()
    >>> # Print the coordinates.
    >>> print(pe.coords)
    EMPTY
    >>>
Readonly properties deﬁned here:
coords
Returns the coordinates of the Point Geometry object.
Methods inherited from GeometryType:
__getattr__(self, item)
__str__(self)
Return String Representation for a Geometry object.
Readonly properties inherited from GeometryType:
geom_type
Returns the type of a Geometry.
LineString
teradataml.geospatial.geometry_types.LineString = class LineString(GeometryType)
teradataml.geospatial.geometry_types.LineString(coordinates=None)
Class LineString enables end user to create an object for the single
LineString using the coordinates. Allows user to use the same in
GeoDataFrame manipulation and processing.
Method resolution order:
LineString
GeometryType
builtins.object
Methods deﬁned here:
__init__(self, coordinates=None)
DESCRIPTION:
    Enables end user to create an object for the single LineString
    using the coordinates. Allows user to use the same in GeoDataFrame
    manipulation and processing using any Geospatial function.
PARAMETERS:
    coordinates:
        Optional Argument.
        Specifies the coordinates of a Line. While passing coordinates
        for a line, one must always pass coordinates in list of either
        two-tuples for 2D or list of three-tuples for 3D.
        Argument also accepts list of Point as well instead of tuples.
        If coordinates are not passed, an object for empty line is
        created.
        Types: List of
                a. Point geometry objects or
                b. two-tuple of int or float or
                c. three-tuple of int or float or
                d. Mix of any of the above.
RETURNS:
    LineString
RAISES:
    TeradataMlException, TypeError, ValueError
EXAMPLES:
    >>> from teradataml import Point, LineString
    # Example 1: Create a LineString in 2D, using x and y coordinates.
    >>> l1 = LineString([(0, 0), (0, 20), (20, 20)])
    >>> # Print the coordinates.
    >>> print(l1.coords)
    [(0, 0), (0, 20), (20, 20)]
    >>> # Print the geometry type.
    >>> l1.geom_type
    'LineString'
    >>>
    # Example 2: Create a LineString in 3D, using x, y and z coordinates.
    >>> l2 = LineString([(0, 0, 1), (0, 1, 3), (1, 3, 6), (3, 3, 6),
    ...                  (3, 6, 1), (6, 3, 3), (3, 3, 0)])
    >>> # Print the coordinates.
    >>> print(l1.coords)
    [(0, 0), (0, 20), (20, 20)]
    >>>
    # Example 3: Create a LineString using Point geometry objects.
    # Create some Points in 2D, using x and y coordinates.
    >>> p1 = Point(0, 20)
    >>> p2 = Point(0, 0)
    >>> p3 = Point(20, 20)
    >>> l3 = LineString([p1, p2, p3])
    >>> # Print the coordinates.
    >>> print(l3.coords)
    [(0, 20), (0, 0), (20, 20)]
    >>>
    # Example 4: Create a LineString using mix of Point geometry objects
    #            and coordinates.
    >>> p1 = Point(0, 20)
    >>> p2 = Point(20, 20)
    >>> l4 = LineString([(0, 0), p1, p2, (20, 0)])
    >>> # Print the coordinates.
    >>> print(l4.coords)
    [(0, 0), (0, 20), (20, 20), (20, 0)]
    >>>
    # Example 5: Create an empty LineString.
    >>> le = LineString()
    >>> # Print the coordinates.
    >>> print(le.coords)
    EMPTY
    >>>
Methods inherited from GeometryType:
__getattr__(self, item)
__str__(self)
Return String Representation for a Geometry object.
Readonly properties inherited from GeometryType:
coords
Returns the coordinates of the Geometry object.
geom_type
Returns the type of a Geometry.
Polygon
teradataml.geospatial.geometry_types.Polygon = class Polygon(GeometryType)
teradataml.geospatial.geometry_types.Polygon(coordinates=None)
Class Polygon enables end user to create an object for the single Polygon
using the coordinates. Allows user to use the same in GeoDataFrame
manipulation and processing.
Method resolution order:
Polygon
GeometryType
builtins.object
Methods deﬁned here:
__init__(self, coordinates=None)
DESCRIPTION:
    Enables end user to create an object for the single Polygon
    using the coordinates. Allows user to use the same in GeoDataFrame
    manipulation and processing using any Geospatial function.
PARAMETERS:
    coordinates:
        Optional Argument.
        Specifies the coordinates of a polygon. While passing coordinates
        for a polygon, one must always pass coordinates in list of either
        two-tuples for 2D or list of three-tuples for 3D.
        Argument also accepts list of Point and/or LineString as well
        instead of tuples.
        If coordinates are not passed, an object for empty polygon is
        created.
        Types: List of
                a. two-tuple of int or float or
                b. three-tuple of int or float or
                c. Point geometry objects or
                d. LineString geometry objects or
                e. Mix of any of the above.
RETURNS:
    Polygon
RAISES:
    TeradataMlException, TypeError, ValueError
EXAMPLES:
    >>> from teradataml import Point, LineString, Polygon
    # Example 1: Create a Polygon in 2D, using x and y coordinates.
    >>> go1 = Polygon([(0, 0), (0, 20), (20, 20), (20, 0), (0, 0)])
    >>> # Print the coordinates.
    >>> print(go1.coords)
    [(0, 0), (0, 20), (20, 20), (20, 0), (0, 0)]
    >>> # Print the geometry type.
    >>> go1.geom_type
    'Polygon'
    >>>
    # Example 2: Create a Polygon in 3D, using x, y and z coordinates.
    >>> go2 = Polygon([(0, 0, 0), (0, 0, 20), (0, 20, 0), (0, 20, 20),
    ...                       (20, 0, 0), (20, 0, 20), (20, 20, 0), (20, 20, 20),
    ...                       (0, 0, 0)])
    >>> # Print the coordinates.
    >>> print(go2.coords)
    [(0, 0, 0), (0, 0, 20), (0, 20, 0), (0, 20, 20), (20, 0, 0), (20, 0, 20), (20, 20, 0), (20, 20, 20), (0, 0, 0)]
    >>>
    # Example 3: Create a Polygon using Point geometry objects.
    # Create Point objects in 2D, using x and y coordinates.
    >>> p1 = Point(0, 0)
    >>> p2 = Point(0, 20)
    >>> p3 = Point(20, 20)
    >>> p4 = Point(20, 0)
    >>> go3 = Polygon([p1, p2, p3, p4, p1])
    >>> # Print the coordinates.
    >>> print(go3.coords)
    [(0, 0), (0, 20), (20, 20), (20, 0), (0, 0)]
    >>>
    # Example 4: Create a Polygon using LineString geometry objects.
    # Create some LineString objects in 2D, using x and y coordinates.
    >>> l1 = LineString([(0, 0), (0, 20), (20, 20)])
    >>> l2 = LineString([(20, 0), (0, 0)])
    >>> go4 = Polygon([l1, l2])
    >>> # Print the coordinates.
    >>> print(go4.coords)
    [(0, 0), (0, 20), (20, 20), (20, 0), (0, 0)]
    >>>
    # Example 5: Create a Polygon using mix of Point, LineString
    #            geometry objects and coordinates.
    >>> p1 = Point(0, 0)
    >>> l1 = LineString([p1, (0, 20), (20, 20)])
    >>> go5 = Polygon([l1, (20, 0), p1])
    >>> # Print the coordinates.
    >>> print(go5.coords)
    [(0, 0), (0, 20), (20, 20), (20, 0), (0, 0)]
    >>>
    # Example 6: Create an empty Polygon.
    >>> poe = Polygon()
    >>> # Print the coordinates.
    >>> print(poe.coords)
    EMPTY
    >>>
Methods inherited from GeometryType:
__getattr__(self, item)
__str__(self)
Return String Representation for a Geometry object.
Readonly properties inherited from GeometryType:
coords
Returns the coordinates of the Geometry object.
geom_type
Returns the type of a Geometry.
MultiPoint
teradataml.geospatial.geometry_types.MultiPoint = class MultiPoint(GeometryType)
teradataml.geospatial.geometry_types.MultiPoint(points=None)
Class MultiPoint enables end user to create an object holding multiple
Point geometry objects. Allows user to use the same in GeoDataFrame
manipulation and processing.
Method resolution order:
MultiPoint
GeometryType
builtins.object
Methods deﬁned here:
__init__(self, points=None)
DESCRIPTION:
    Enables end user to create an object holding the multiple Point
    geometry objects. Allows user to use the same in GeoDataFrame
    manipulation and processing using any Geospatial function.
PARAMETERS:
    points:
        Optional Argument.
        Specifies the list of points. If no points are passed, an object
        for empty MultiPoint is created.
        Types: List of Point objects
RETURNS:
    MultiPoint
RAISES:
    TeradataMlException, TypeError, ValueError
EXAMPLES:
    >>> from teradataml import Point, MultiPoint
    # Example 1: Create a MultiPoint in 2D, using x and y coordinates.
    >>> p1 = Point(0, 0)
    >>> p2 = Point(0, 20)
    >>> p3 = Point(20, 20)
    >>> p4 = Point(20, 0)
    >>> go1 = MultiPoint([p1, p2, p3, p4, p1])
    >>> # Print the coordinates.
    >>> print(go1.coords)
    [(0, 0), (0, 20), (20, 20), (20, 0), (0, 0)]
    >>> # Print the geometry type.
    >>> print(go1.geom_type)
    MultiPoint
    >>>
    # Example 2: Create an empty MultiPoint.
    >>> poe = MultiPoint()
    >>> # Print the coordinates.
    >>> print(poe.coords)
    EMPTY
    >>>
Methods inherited from GeometryType:
__getattr__(self, item)
__str__(self)
Return String Representation for a Geometry object.
Readonly properties inherited from GeometryType:
coords
Returns the coordinates of the Geometry object.
geom_type
Returns the type of a Geometry.
MultiLineString
teradataml.geospatial.geometry_types.MultiLineString = class MultiLineString(GeometryType)
teradataml.geospatial.geometry_types.MultiLineString(lines=None)
Class MultiLineString enables end user to create an object holding multiple
LineString geometry objects. Allows user to use the same in GeoDataFrame
manipulation and processing.
Method resolution order:
MultiLineString
GeometryType
builtins.object
Methods deﬁned here:
__init__(self, lines=None)
DESCRIPTION:
    Enables end user to create an object holding the multiple LineString
    geometry objects. Allows user to use the same in GeoDataFrame
    manipulation and processing using any Geospatial function.
PARAMETERS:
    lines:
        Optional Argument.
        Specifies the list of lines. If no lines are passed, an object
        for empty MultiLineString is created.
        Types: List of LineString objects
RETURNS:
    MultiLineString
RAISES:
    TeradataMlException, TypeError, ValueError
EXAMPLES:
    >>> from teradataml import LineString, MultiLineString
    # Example 1: Create a MultiPoint in 2D, using x and y coordinates.
    >>> l1 = LineString([(1, 3), (3, 0), (0, 1)])
    >>> l2 = LineString([(1.35, 3.6456), (3.6756, 0.23), (0.345, 1.756)])
    >>> go1 = MultiLineString([l1, l2])
    >>> # Print the coordinates.
    >>> print(go1.coords)
    [[(1, 3), (3, 0), (0, 1)], [(1.35, 3.6456), (3.6756, 0.23), (0.345, 1.756)]]
    >>> # Print the geometry type.
    >>> print(go1.geom_type)
    MultiLineString
    >>>
    # Example 2: Create an empty MultiLineString.
    >>> mls = MultiLineString()
    >>> # Print the coordinates.
    >>> print(mls.coords)
    EMPTY
    >>>
Methods inherited from GeometryType:
__getattr__(self, item)
__str__(self)
Return String Representation for a Geometry object.
Readonly properties inherited from GeometryType:
coords
Returns the coordinates of the Geometry object.
geom_type
Returns the type of a Geometry.
MultiPolygon
teradataml.geospatial.geometry_types.MultiPolygon = class MultiPolygon(GeometryType)
teradataml.geospatial.geometry_types.MultiPolygon(polygons=None)
Class MultiPolygon enables end user to create an object holding multiple
Polygon geometry objects. Allows user to use the same in GeoDataFrame
manipulation and processing.
Method resolution order:
MultiPolygon
GeometryType
builtins.object
Methods deﬁned here:
__init__(self, polygons=None)
DESCRIPTION:
    Enables end user to create an object holding the multiple Polygon
    geometry objects. Allows user to use the same in GeoDataFrame
    manipulation and processing using any Geospatial function.
PARAMETERS:
    polygons:
        Optional Argument.
        Specifies the list of polygons. If no polygons are passed, an
        object for empty MultiPolygon is created.
        Types: List of Polygon objects
RETURNS:
    MultiPolygon
RAISES:
    TeradataMlException, TypeError, ValueError
EXAMPLES:
    >>> from teradataml import Polygon, MultiPolygon
    # Example 1: Create a MultiPolygon in 2D, using x and y coordinates.
    >>> po1 = Polygon([(0, 0), (0, 20), (20, 20), (20, 0), (0, 0)])
    >>> po2 = Polygon([(0.6, 0.8), (0.6, 20.8), (20.6, 20.8), (20.6, 0.8), (0.6, 0.8)])
    >>> go1 = MultiPolygon([po1, po2])
    >>> # Print the coordinates.
    >>> print(go1.coords)
    [[(0, 0), (0, 20), (20, 20), (20, 0), (0, 0)], [(0.6, 0.8), (0.6, 20.8), (20.6, 20.8), (20.6, 0.8), (0.6, 0.8)]]
    >>> # Print the geometry type.
    >>> print(go1.geom_type)
    MultiPolygon
    >>>
    # Example 2: Create an empty MultiPolygon.
    >>> poe = MultiPolygon()
    >>> # Print the coordinates.
    >>> print(poe.coords)
    EMPTY
    >>>
Methods inherited from GeometryType:
__getattr__(self, item)
__str__(self)
Return String Representation for a Geometry object.
Readonly properties inherited from GeometryType:
coords
Returns the coordinates of the Geometry object.
geom_type
Returns the type of a Geometry.
GeometryCollection
teradataml.geospatial.geometry_types.GeometryCollection = class GeometryCollection(GeometryType)
teradataml.geospatial.geometry_types.GeometryCollection(geometries=None)
Class GeometryCollection enables end user to create an object for the
single GeometryCollection, i.e., collection of different geometry objects
using the geometries. This allows user to use the same in GeoDataFrame
manipulation and processing.
Method resolution order:
GeometryCollection
GeometryType
builtins.object
Methods deﬁned here:
__init__(self, geometries=None)
DESCRIPTION:
    Enables end user to create an object holding the multiple types of
    geometry objects. Allows user to use the same in GeoDataFrame
    manipulation and processing using any Geospatial function.
PARAMETERS:
    geoms:
        Optional Argument.
        Specifies the list of different geometry types.
        If no geometries are are passed, an object for empty
        GeometryCollection is created.
        Types: List of geometry objects of types:
                1. Point
                2. LineString
                3. Polygon
                4. MultiPoint
                5. MultiLineString
                6. MultiPolygon
                7. GeometryCollection
                8. Mixture of any of these.
RETURNS:
    GeometryCollection
RAISES:
    TeradataMlException, TypeError, ValueError
EXAMPLES:
    >>> from teradataml import Point, LineString, Polygon, MultiPoint,
    ...    MultiLineString, MultiPolygon, GeometryCollection
    # Example 1: Create a MultiPoint in 2D, using x and y coordinates.
    >>> # Create Point objects.
    >>> p1 = Point(1, 1)
    >>> p2 = Point()
    >>> p3 = Point(6, 3)
    >>> p4 = Point(10, 5)
    >>> p5 = Point()
    >>>
    >>> # Create LineString Objects.
    >>> l1 = LineString([(1, 3), (3, 0), (0, 1)])
    >>> l2 = LineString([(1.35, 3.6456), (3.6756, 0.23), (0.345, 1.756)])
    >>> l3 = LineString()
    >>>
    >>> # Create Polygon Objects.
    >>> po1 = Polygon([(0, 0, 0), (0, 0, 20), (0, 20, 0), (0, 20, 20),
    ...        (20, 0, 0), (20, 0, 20), (20, 20, 0), (20, 20, 20),
    ...        (0, 0, 0)])
    >>> po2 = Polygon([(0, 0, 0), (0, 0, 20.435), (0, 20.435, 0),
    ...        (0, 20.435, 20.435), (20.435, 0, 0), (20.435, 0, 20.435),
    ...        (20.435, 20.435, 0), (20.435, 20.435, 20.435),
    ...        (0, 0, 0)])
    >>> po3 = Polygon()
    >>>
    >>> # Create MultiPolygon Object.
    >>> mpol = MultiPolygon([po1, Polygon(), po2])
    >>>
    >>> # Create MultiLineString Object.
    >>> mlin = MultiLineString([l1, l2, l3])
    >>>
    >>> # Create MultiPoint Object.
    >>> mpnt = MultiPoint([p1, p2, p3, p4, p5])
    >>>
    >>> # Create a GeometryCollection object.
    >>> gc1 = GeometryCollection([p1, p2, l1, l3, po2, po3, po1, mpol, mlin, mpnt])
    >>> # Print the coordinates.
    >>> print(gc1.coords)
    [(1, 1), 'EMPTY', [(1, 3), (3, 0), (0, 1)], 'EMPTY', [(0, 0, 0), (0, 0, 20.435), (0, 20.435, 0), (0, 20.435, 20.435),
    >>> # Print the geometry type.
    >>> print(gc1.geom_type)
    GeometryCollection
    >>>
    # Example 2: Create an empty GeometryCollection.
    >>> gc2 = GeometryCollection()
    >>> # Print the coordinates.
    >>> print(gc2.coords)
    EMPTY
    >>>
Methods inherited from GeometryType:
__getattr__(self, item)
__str__(self)
Return String Representation for a Geometry object.
Readonly properties inherited from GeometryType:
coords
Returns the coordinates of the Geometry object.
geom_type
Returns the type of a Geometry.
GeoSequence
teradataml.geospatial.geometry_types.GeoSequence = class GeoSequence(LineString)
teradataml.geospatial.geometry_types.GeoSequence(coordinates=None, timestamps=None, link_ids=None, user_field_count=0, user_fiel
Class GeoSequence enables end user to create an object for the
LineString geometry objects with tracking information such as
timestamp. This allows user to use the same in GeoDataFrame
manipulation and processing.
Method resolution order:
GeoSequence
LineString
GeometryType
builtins.object
Methods deﬁned here:
__init__(self, coordinates=None, timestamps=None, link_ids=None, user_ﬁeld_count=0, user_ﬁelds=None)
DESCRIPTION:
    Enables end user to create an object holding the LineString
    geometry objects with tracking information such as timestamps.
    Allows user to use the same in GeoDataFrame manipulation and
    processing using any Geospatial function.
PARAMETERS:
    coordinates:
        Optional Argument.
        Specifies the list of coordinates of a Point. While passing
        coordinates, one must always pass coordinates in list of either
        two-tuples for 2D or list of three-tuples for 3D.
        Argument also accepts list of Points as well instead of tuples.
        If coordinates are not passed, an object for empty line is
        created.
        Types: List of
                a. Point geometry objects or
                b. two-tuple of int or float or
                c. three-tuple of int or float or
                d. Mix of any of the above.
    timestamps:
        Optional Argument.
        Specifies the list of timestamp values for each coordinate with
        the following format:
            yyyy-mm-dd hh:mi:ss.ms
        The first timestamp value is associated with the first point, the
        second timestamp value is associated with the second point, and
        so forth.
        Note:
            You must specify n timestamp values, where n is the number of
            points in the geosequence.
        Types: list of strings
    link_ids:
        Optional Argument.
        Specifies the list of values for the ID of the link on the road
        network for a point in the geosequence.
        This value is reserved for a future release.
        The first link ID value is associated with the first point, the
        second link ID value is associated with the second point, and
        so forth.
        Note:
            You must specify n link ID values, where n is the number of
            points in the geosequence.
        Types: list of ints
    user_field_count:
        Optional Argument.
        Specifies the value that represents the number of user field
        elements for each point in the geosequence.
        A value of 0 indicates that no user field elements appear after
        count in the character string.
        Default Value: 0
        Types: int
    user_fields:
        Optional Argument.
        Specifies the list of user field tuples that represents a value to
        associated with a point. For example, certain tracking systems may
        associate velocity, direction, and acceleration values with each point.
        Note:
            1. You must specify count groups of n user field values (where n is
               the number of points in the geosequence).
            2. The first group provides the first user field values for each point,
               the second group provides the second user field values for each point,
               and so forth.
            3. Each group can be formed using a tuple.
        Types: list of tuples of ints or floats
RETURNS:
    GeoSequence
RAISES:
    TeradataMlException, TypeError, ValueError
EXAMPLES:
    >>> from teradataml import Point, GeoSequence
    # Example 1: Create a GeoSequence with 2D points and no user fields.
    >>> coordinates = [(1, 3), (3, 0), (0, 1)]
    >>> timestamps = ["2008-03-17 10:34:03.53", "2008-03-17 10:38:25.21", "2008-03-17 10:41:41.48"]
    >>> link_ids = [1001, 1002, 1003]
    >>> gs1 = GeoSequence(coordinates=coordinates, timestamps=timestamps, link_ids=link_ids)
    >>> gs1.coords
    [(1, 3), (3, 0), (0, 1)]
    >>> str(gs1)
    'GeoSequence((1 3, 3 0, 0 1), (2008-03-17 10:34:03.53, 2008-03-17 10:38:25.21, 2008-03-
17 10:41:41.48), (1001, 1002, 1003), (0))'
    >>>
    # Example 2: Create a GeoSequence with 3D points and 2 user fields.
    #            Note that coordinates can be provided as tuple of ints/floats
    #            or Point objects.
    >>> p1 = (3, 0, 6)
    >>> coordinates = [(1, 3, 6), p1, (6, 0, 1)]
    >>> timestamps = ["2008-03-17 10:34:03.53", "2008-03-17 10:38:25.21", "2008-03-17 10:41:41.48"]
    >>> link_ids = [1001, 1002, 1003]
    >>> user_fields = [(1, 2), (3, 4), (5, 6)]
    >>> gs2 = GeoSequence(coordinates=coordinates, timestamps=timestamps, link_ids=link_ids,
    ...                   user_field_count=2, user_fields=user_fields)
    >>> gs2.coords
    [(1, 3, 6), (3, 0, 6), (6, 0, 1)]
    >>> str(gs2)
    'GeoSequence((1 3 6, 3 0 6, 6 0 1), (2008-03-17 10:34:03.53, 2008-03-17 10:38:25.21, 2008-03-
17 10:41:41.48), (1001, 1002, 1003), (2, 1, 2, 3, 4, 5, 6))'
    >>>
    # Example 3: Create an empty GeoSequence.
    >>> gs3 = GeoSequence()
    >>> # Print the coordinates.
    >>> print(gc3.coords)
    EMPTY
    >>>
Methods inherited from GeometryType:
__getattr__(self, item)
__str__(self)
Return String Representation for a Geometry object.
Readonly properties inherited from GeometryType:
coords
Returns the coordinates of the Geometry object.
geom_type
Returns the type of a Geometry.
Data descriptors inherited from GeometryType:
__dict__
dictionary for instance variables (if defined)
__weakref__
list of weak references to the object (if defined)
teradataml GeoDataFrame
Properties
columns
teradataml.geospatial.geodataframe.GeoDataFrame.columns
DESCRIPTION:
    Get the column names of GeoDataFrame.
RETURNS:
    List containing column names.
EXAMPLES:
    >>> load_example_data("geodataframe","sample_streets")
    >>> df = GeoDataFrame.from_table('sample_streets')
    >>> df.columns
    ['skey', 'street_name', 'street_shape']
    >>>
db_object_name
teradataml.geospatial.geodataframe.GeoDataFrame.db_object_name
DESCRIPTION:
    Get the underlying database object name, on which GeoDataFrame is
    created.
RETURNS:
    str representing object name of GeoDataFrame
EXAMPLES:
    >>> load_example_data("geodataframe", "sample_streets")
    >>> gdf = GeoDataFrame('sample_streets')
    >>> gdf.db_object_name
    '"sample_streets"'
dtypes
teradataml.geospatial.geodataframe.GeoDataFrame.dtypes
DESCRIPTION:
    Returns a MetaData containing the column names and types.
PARAMETERS:
    None
RETURNS:
    MetaData containing the column names and Python types
RAISES:
    None
EXAMPLES:
    >>> load_example_data("geodataframe","sample_streets")
    >>> df = GeoDataFrame('sample_streets')
    >>> print(df.dtypes)
    skey            int
    street_name     str
    street_shape    str
    >>>
geometry
teradataml.geospatial.geodataframe.GeoDataFrame.geometry
DESCRIPTION:
    Returns a GeoColumnExpression for a column containing geometry data.
    If GeoDataFrame contains, multiple columns containing geometry data,
    then it returns reference to only one of them.
    Columns containing geometry data can be of following types:
        1. ST_GEOMETRY
        2. MBB
        3. MBR
    Refer 'GeoDataFrame.tdtypes' to view the Teradata column data types.
    Note:
        This property is used to execute any geospatial operation on
        GeoDataFrame, i.e., any geospatial function executed on
        GeoDataFrame, is executed on the geomtry column referenced by
        this property.
RETURNS:
    GeoDataFrameColumn
EXAMPLES:
    >>> load_example_data("geodataframe", ["sample_cities", "sample_streets"])
    >>> cities = GeoDataFrame("sample_cities")
    >>> streets = GeoDataFrame("sample_streets")
    >>> city_streets = cities.join(streets, how="cross", lsuffix="l", rsuffix="r")
    >>> city_streets
       l_skey  r_skey   city_name                                 city_shape  street_name              street_shape
    0       0       1  Oceanville            POLYGON ((1 1,1 3,6 3,6 0,1 1))  Main Street  LINESTRING (2 2,3 2,4 1)
    1       0       1  Oceanville            POLYGON ((1 1,1 3,6 3,6 0,1 1))   Coast Blvd  LINESTRING (12 12,18 17)
    2       1       1     Seaside  POLYGON ((10 10,10 20,20 20,20 15,10 10))   Coast Blvd  LINESTRING (12 12,18 17)
    3       1       1     Seaside  POLYGON ((10 10,10 20,20 20,20 15,10 10))  Main Street  LINESTRING (2 2,3 2,4 1)
    >>>
    # Check the name of the column containing geometry data, where
    # 'geometry' property references.
    >>> city_streets.geometry.name
    'city_shape'
    >>>
    # Check all the column types.
    >>> city_streets.tdtypes
    l_skey                                    INTEGER()
    r_skey                                    INTEGER()
    city_name       VARCHAR(length=40, charset='LATIN')
    city_shape                               GEOMETRY()
    street_name     VARCHAR(length=40, charset='LATIN')
    street_shape                             GEOMETRY()
    >>>
    >>>
    # Set the 'geometry' property to refer 'street_shape' column.
    >>> city_streets.geometry = city_streets.street_shape
    >>> city_streets.geometry.name
    'street_shape'
    >>>
    # Check whether the geometry referenced by 'geometry' property are 3D
    # or not.
    >>> city_streets.is_3D
       l_skey  r_skey   city_name                                 city_shape  street_name              street_shape  is_3D_stre
    0       1       1     Seaside  POLYGON ((10 10,10 20,20 20,20 15,10 10))  Main Street  LINESTRING (2 2,3 2,4 1)            
    1       1       1     Seaside  POLYGON ((10 10,10 20,20 20,20 15,10 10))   Coast Blvd  LINESTRING (12 12,18 17)            
    2       0       1  Oceanville            POLYGON ((1 1,1 3,6 3,6 0,1 1))   Coast Blvd  LINESTRING (12 12,18 17)            
    3       0       1  Oceanville            POLYGON ((1 1,1 3,6 3,6 0,1 1))  Main Street  LINESTRING (2 2,3 2,4 1)            
    >>>
    # Use the geometry property to execute multiple geospatial functions
    # in conjunctions with GeoDataFrame.assign()
    # Get the geometry type.
    >>> geom_type = city_streets.geometry.geom_type
    # Check if geometry is simple or not.
    >>> is_simple = city_streets.geometry.is_simple
    # Check if geometry is valid or not.
    >>> is_valid = city_streets.geometry.is_valid
    >>>
    # Call GeoDataFrame.assign() and pass the above GeoDataFrameColumn, i.e.,
    # ColumnExpressions as input.
    >>> city_streets.assign(geom_type = geom_type,
    ...                     is_simple = is_simple,
    ...                     is_valid = is_valid
    ...                     )
       l_skey  r_skey   city_name                                 city_shape  street_name              street_shape      geom_t
    0       0       1  Oceanville            POLYGON ((1 1,1 3,6 3,6 0,1 1))  Main Street  LINESTRING (2 2,3 2,4 1)  ST_LineStr
    1       0       1  Oceanville            POLYGON ((1 1,1 3,6 3,6 0,1 1))   Coast Blvd  LINESTRING (12 12,18 17)  ST_LineStr
    2       1       1     Seaside  POLYGON ((10 10,10 20,20 20,20 15,10 10))   Coast Blvd  LINESTRING (12 12,18 17)  ST_LineStr
    3       1       1     Seaside  POLYGON ((10 10,10 20,20 20,20 15,10 10))  Main Street  LINESTRING (2 2,3 2,4 1)  ST_LineStr
    >>>
iloc
teradataml.geospatial.geodataframe.GeoDataFrame.iloc = iloc(self)
Access a group of rows and columns by integer values or a boolean array.
VALID INPUTS:
    - A single integer values, e.g. 5.
    - A list or array of integer values, e.g. ``[1, 2, 3]``.
    - A slice object with integer values, e.g. ``1:6``.
      Note: The stop value is excluded.
    - A boolean array of the same length as the column axis for column access,
    Note: For integer indexing on row access, the integer index values are
    applied to a sorted teradataml GeoDataFrame on the index column or the first column if
    there is no index column.
RETURNS:
    teradataml GeoDataFrame
RAISE:
    TeradataMlException
EXAMPLES
--------
    >>> load_example_data("geodataframe", ["sample_shapes"])
    >>> geo_dataframe = GeoDataFrame("sample_shapes")
    >>> geo_dataframe = geo_dataframe.select(['skey', 'points', 'linestrings'])
    >>> geo_dataframe
                                                                         points                                                
    skey
    1006                                           POINT (235.52 54.546 7.4564)                             LINESTRING (1.35 3.
    1007                                     MULTIPOINT (1 1,1 3,6 3,10 5,20 1)                                              MU
(10 5,20 1))
    1005                                                          POINT (1 3 5)                                                
    1004                                                       POINT (10 20 30)                                                
    1003                                                  POINT (235.52 54.546)                                         LINESTR
    1001                                                          POINT (10 20)                                                
    1002                                                            POINT (1 3)                                                
    1009                                MULTIPOINT (10 20 30,40 50 60,70 80 80)                              MULTILINESTRING ((
(70 80 80,90 100 110))
    1008       MULTIPOINT (1.65 1.76,1.23 3.76,6.23 3.78,10.76 5.9,20.32 1.231)                    MULTILINESTRING ((1 3,3 0,0 
(1.35 3.6456,3.6756 0.23,0.345 1.756))
    1010  MULTIPOINT (10.345 20.32 30.6,40.234 50.23 60.24,70.234 80.56 80.234)  MULTILINESTRING ((1 3 6,3 0 6,6 0 1),
(1.35 3.6456 4.5,3.6756 0.23 6.8,0.345 1.756 8.9))
    # Example 1: Slice a teradataml GeoDataFrame using a single integer.
    >>> geo_dataframe.iloc[1]
               points               linestrings
    skey
    1002  POINT (1 3)  LINESTRING (1 3,3 0,0 1)
    # Example 2: Slice a teradataml GeoDataFrame using list of integers. Note using ``[[]]``.
    >>> geo_dataframe.iloc[[1, 2]]
                         points                                       linestrings
    skey
    1003  POINT (235.52 54.546)  LINESTRING (1.35 3.6456,3.6756 0.23,0.345 1.756)
    1002            POINT (1 3)                          LINESTRING (1 3,3 0,0 1)
    # Example 3: Slice a teradataml GeoDataFrame using single integer for row and column.
    >>> geo_dataframe.iloc[5, 1]
                             points
    0  POINT (235.52 54.546 7.4564)
    # Example 4: Slice a teradataml GeoDataFrame using single integer for row and column access using a tuple.
    >>> geo_dataframe.iloc[(5, 1)]
                             points
    0  POINT (235.52 54.546 7.4564)
    # Example 5: Slice a teradataml GeoDataFrame using range for row and a single integer for column access.
    #            Note the stop for the range is excluded.
    >>> geo_dataframe.iloc[1:5, 2]
                                            linestrings
    0           LINESTRING (10 20 30,40 50 60,70 80 80)
    1                    LINESTRING (1 3 6,3 0 6,6 0 1)
    2  LINESTRING (1.35 3.6456,3.6756 0.23,0.345 1.756)
    3                          LINESTRING (1 3,3 0,0 1)
    # Example 6: Slice a teradataml GeoDataFrame using range for row and column access.
    #            Note the stop for the range is excluded.
    >>> geo_dataframe.iloc[1:5, 0:2]
                         points
    skey
    1004       POINT (10 20 30)
    1005          POINT (1 3 5)
    1003  POINT (235.52 54.546)
    1002            POINT (1 3)
    # Example 7: Slice a teradataml GeoDataFrame using empty range for row and column access.
    #            All rows and columns are retrieved.
    >>> geo_dataframe.iloc[:, :]
                                                                         points                                                
    skey
    1003                                                  POINT (235.52 54.546)                                         LINESTR
    1010  MULTIPOINT (10.345 20.32 30.6,40.234 50.23 60.24,70.234 80.56 80.234)  MULTILINESTRING ((1 3 6,3 0 6,6 0 1),
(1.35 3.6456 4.5,3.6756 0.23 6.8,0.345 1.756 8.9))
    1008       MULTIPOINT (1.65 1.76,1.23 3.76,6.23 3.78,10.76 5.9,20.32 1.231)                    MULTILINESTRING ((1 3,3 0,0 
(1.35 3.6456,3.6756 0.23,0.345 1.756))
    1006                                           POINT (235.52 54.546 7.4564)                             LINESTRING (1.35 3.
    1007                                     MULTIPOINT (1 1,1 3,6 3,10 5,20 1)                                              MU
(10 5,20 1))
    1005                                                          POINT (1 3 5)                                                
    1009                                MULTIPOINT (10 20 30,40 50 60,70 80 80)                              MULTILINESTRING ((
(70 80 80,90 100 110))
    1001                                                          POINT (10 20)                                                
    1002                                                            POINT (1 3)                                                
    1004                                                       POINT (10 20 30)                                                
    # Example 8: Slice a teradataml GeoDataFrame using list of integers and boolean array for column access.
    >>> geo_dataframe.iloc[[0, 1, 2], [True, False, True]]
                                               linestrings
    skey
    1003  LINESTRING (1.35 3.6456,3.6756 0.23,0.345 1.756)
    1002                          LINESTRING (1 3,3 0,0 1)
    1001                      LINESTRING (1 1,2 2,3 3,4 4)
index
teradataml.geospatial.geodataframe.GeoDataFrame.index
DESCRIPTION:
    Returns the index_label of the teradataml GeoDataFrame.
RETURNS:
    str or List of Strings (str) representing the index_label of the GeoDataFrame.
RAISES:
    None
EXAMPLES:
    >>> load_example_data("geodataframe","sample_cities")
    >>> df = GeoDataFrame("sample_cities")
    >>> df
           city_name                                 city_shape
    skey
    1        Seaside  POLYGON ((10 10,10 20,20 20,20 15,10 10))
    0     Oceanville            POLYGON ((1 1,1 3,6 3,6 0,1 1))
    >>>
    # Get the index_label
    >>> df.index
    ['skey']
    >>>
    # Set new index_label
    >>> df = df.set_index(['city_shape', 'city_name'])
    >>> df
                                                          skey
    city_name  city_shape
    Seaside    POLYGON ((10 10,10 20,20 20,20 15,10 10))     1
    Oceanville POLYGON ((1 1,1 3,6 3,6 0,1 1))               0
    >>>
    # Get the index_label
    >>> df.index
    ['city_name', 'city_shape']
    >>>
loc
teradataml.geospatial.geodataframe.GeoDataFrame.loc = loc(self)
Access a group of rows and columns by label(s) or a boolean array.
VALID INPUTS:
    - A single label, e.g. ``5`` or ``'a'``, (note that ``5`` is
    interpreted as a label of the index, it is not interpreted as an
    integer position along the index).
    - A list or array of column or index labels, e.g. ``['a', 'b', 'c']``.
    - A slice object with labels, e.g. ``'a':'f'``.
    Note that unlike the usual python slices where the stop index is not included, both the
        start and the stop are included
    - A conditional expression for row access.
    - A boolean array of the same length as the column axis for column access.
RETURNS:
    teradataml GeoDataFrame
RAISE:
    TeradataMlException
EXAMPLES
--------
    >>> load_example_data("geodataframe", ["sample_shapes"])
    >>> geo_dataframe = GeoDataFrame("sample_shapes")
    >>> geo_dataframe = geo_dataframe.select(['skey', 'linestrings', 'polygons'])
    >>> geo_dataframe
                                                                                      linestrings                              
    skey
    1006                             LINESTRING (1.35 3.6456 4.5,3.6756 0.23 6.8,0.345 1.756 8.9)                              
    1007                                              MULTILINESTRING ((1 1,1 3,6 3),
(10 5,20 1))                                                                                                                   
((10 5,10 10,20 10,20 5,10 5)))
    1005                                                           LINESTRING (1 3 6,3 0 6,6 0 1)                              
    1004                                                  LINESTRING (10 20 30,40 50 60,70 80 80)                              
(5 5 5,5 10 10,10 10 10,10 10 5,5 5 5))
    1003                                         LINESTRING (1.35 3.6456,3.6756 0.23,0.345 1.756)                              
    1001                                                             LINESTRING (1 1,2 2,3 3,4 4)                              
    1002                                                                 LINESTRING (1 3,3 0,0 1)                              
(5 5,5 10,10 10,10 5,5 5))
    1009                              MULTILINESTRING ((10 20 30,40 50 60),
(70 80 80,90 100 110))                                                                                                         
((50 50 50,50 90 90,90 90 90,90 50 90,50 50 50)))
    1008                    MULTILINESTRING ((1 3,3 0,0 1),
(1.35 3.6456,3.6756 0.23,0.345 1.756))                                                                                         
((0.6 0.8,0.6 20.8,20.6 20.8,20.6 0.8,0.6 0.8)))
    1010  MULTILINESTRING ((1 3 6,3 0 6,6 0 1),
(1.35 3.6456 4.5,3.6756 0.23 6.8,0.345 1.756 8.9))  MULTIPOLYGON (((0 0 0,0 0 20,0 20 0,0 20 20,20 0 0,20 0 20,20 20 0,20 20 20
((0 0 0,0 0 20.435,0.0 20.435 0,0.0 20.435 20.435,20.435 0.0 0,20.435 0.0 20.435,20.435 20.435 0,20.435 20.435 20.435,0 0 0)))
    # Example 1: Slice a teradataml GeoDataFrame using a single label.
    >>> geo_dataframe.loc[1004]
                                      linestrings                                                                              
    skey
    1004  LINESTRING (10 20 30,40 50 60,70 80 80)  POLYGON ((0 0 0,0 10 20,20 20 30,20 10 0,0 0 0),
(5 5 5,5 10 10,10 10 10,10 10 5,5 5 5))
    # Example 2: Slice a teradataml GeoDataFrame using list of labels. Note using ``[[]]``.
    >>> geo_dataframe.loc[[1004, 1010]]
                                                                                      linestrings                              
    skey
    1010  MULTILINESTRING ((1 3 6,3 0 6,6 0 1),
(1.35 3.6456 4.5,3.6756 0.23 6.8,0.345 1.756 8.9))  MULTIPOLYGON (((0 0 0,0 0 20,0 20 0,0 20 20,20 0 0,20 0 20,20 20 0,20 20 20
((0 0 0,0 0 20.435,0.0 20.435 0,0.0 20.435 20.435,20.435 0.0 0,20.435 0.0 20.435,20.435 20.435 0,20.435 20.435 20.435,0 0 0)))
    1004                                                  LINESTRING (10 20 30,40 50 60,70 80 80)                              
(5 5 5,5 10 10,10 10 10,10 10 5,5 5 5))
    # Example 3: Slice a teradataml GeoDataFrame using single label both for row and column.
    >>> geo_dataframe.loc[1004, 'linestrings']
                                   linestrings
    0  LINESTRING (10 20 30,40 50 60,70 80 80)
    # Example 4: Slice a teradataml GeoDataFrame using single label for row and column access using a tuple.
    >>> geo_dataframe.loc[(1004, 'linestrings')]
                                   linestrings
    0  LINESTRING (10 20 30,40 50 60,70 80 80)
    # Example 5: Slice a teradataml GeoDataFrame using with range of labels for row and single label for column.
                 Note that both the start and stop of the slice are included.
    >>> geo_dataframe.loc[1001:1004, 'linestrings']
                                            linestrings
    0  LINESTRING (1.35 3.6456,3.6756 0.23,0.345 1.756)
    1                      LINESTRING (1 1,2 2,3 3,4 4)
    2                          LINESTRING (1 3,3 0,0 1)
    3           LINESTRING (10 20 30,40 50 60,70 80 80)
    # Example 6: Slice a teradataml GeoDataFrame with range of labels for both rows and columns.
                 Note that both the start and stop of the range are included.
    >>> geo_dataframe.loc[1001:1004, 'skey':'linestrings']
                                               linestrings
    skey
    1003  LINESTRING (1.35 3.6456,3.6756 0.23,0.345 1.756)
    1001                      LINESTRING (1 1,2 2,3 3,4 4)
    1002                          LINESTRING (1 3,3 0,0 1)
    1004           LINESTRING (10 20 30,40 50 60,70 80 80)
    # Example 7: Slice a teradataml GeoDataFrame using empty range for rows and columns.
                 All rows and columns are returned.
    >>> geo_dataframe.loc[:, :]
                                                                                      linestrings                              
    skey
    1006                             LINESTRING (1.35 3.6456 4.5,3.6756 0.23 6.8,0.345 1.756 8.9)                              
    1007                                              MULTILINESTRING ((1 1,1 3,6 3),
(10 5,20 1))                                                                                                                   
((10 5,10 10,20 10,20 5,10 5)))
    1005                                                           LINESTRING (1 3 6,3 0 6,6 0 1)                              
    1004                                                  LINESTRING (10 20 30,40 50 60,70 80 80)                              
(5 5 5,5 10 10,10 10 10,10 10 5,5 5 5))
    1003                                         LINESTRING (1.35 3.6456,3.6756 0.23,0.345 1.756)                              
    1001                                                             LINESTRING (1 1,2 2,3 3,4 4)                              
    1002                                                                 LINESTRING (1 3,3 0,0 1)                              
(5 5,5 10,10 10,10 5,5 5))
    1009                              MULTILINESTRING ((10 20 30,40 50 60),
(70 80 80,90 100 110))                                                                                                         
((50 50 50,50 90 90,90 90 90,90 50 90,50 50 50)))
    1008                    MULTILINESTRING ((1 3,3 0,0 1),
(1.35 3.6456,3.6756 0.23,0.345 1.756))                                                                                         
((0.6 0.8,0.6 20.8,20.6 20.8,20.6 0.8,0.6 0.8)))
    1010  MULTILINESTRING ((1 3 6,3 0 6,6 0 1),
(1.35 3.6456 4.5,3.6756 0.23 6.8,0.345 1.756 8.9))  MULTIPOLYGON (((0 0 0,0 0 20,0 20 0,0 20 20,20 0 0,20 0 20,20 20 0,20 20 20
((0 0 0,0 0 20.435,0.0 20.435 0,0.0 20.435 20.435,20.435 0.0 0,20.435 0.0 20.435,20.435 20.435 0,20.435 20.435 20.435,0 0 0)))
    # Example 8: Slice a teradataml GeoDataFrame based on a conditional expression.
    >>> geo_dataframe.loc[geo_dataframe['skey'] > 1005]
                                                                                      linestrings                              
    skey
    1006                             LINESTRING (1.35 3.6456 4.5,3.6756 0.23 6.8,0.345 1.756 8.9)                              
    1007                                              MULTILINESTRING ((1 1,1 3,6 3),
(10 5,20 1))                                                                                                                   
((10 5,10 10,20 10,20 5,10 5)))
    1009                              MULTILINESTRING ((10 20 30,40 50 60),
(70 80 80,90 100 110))                                                                                                         
((50 50 50,50 90 90,90 90 90,90 50 90,50 50 50)))
    1008                    MULTILINESTRING ((1 3,3 0,0 1),
(1.35 3.6456,3.6756 0.23,0.345 1.756))                                                                                         
((0.6 0.8,0.6 20.8,20.6 20.8,20.6 0.8,0.6 0.8)))
    1010  MULTILINESTRING ((1 3 6,3 0 6,6 0 1),
(1.35 3.6456 4.5,3.6756 0.23 6.8,0.345 1.756 8.9))  MULTIPOLYGON (((0 0 0,0 0 20,0 20 0,0 20 20,20 0 0,20 0 20,20 20 0,20 20 20
((0 0 0,0 0 20.435,0.0 20.435 0,0.0 20.435 20.435,20.435 0.0 0,20.435 0.0 20.435,20.435 20.435 0,20.435 20.435 20.435,0 0 0)))
    # Example 9: Slice a teradataml GeoDataFrame based on a conditional expression
    #            and column labels specified.
    >>> geo_dataframe.loc[geo_dataframe['skey'] > 1005, ['skey', 'linestrings']]
                                                                                      linestrings
    skey
    1006                             LINESTRING (1.35 3.6456 4.5,3.6756 0.23 6.8,0.345 1.756 8.9)
    1007                                              MULTILINESTRING ((1 1,1 3,6 3),(10 5,20 1))
    1009                              MULTILINESTRING ((10 20 30,40 50 60),(70 80 80,90 100 110))
    1008                    MULTILINESTRING ((1 3,3 0,0 1),(1.35 3.6456,3.6756 0.23,0.345 1.756))
    1010  MULTILINESTRING ((1 3 6,3 0 6,6 0 1),(1.35 3.6456 4.5,3.6756 0.23 6.8,0.345 1.756 8.9))
    # Example 10: Slice a teradataml GeoDataFrame based on a conditional expression
    #             and a range of column labels specified.
    #             Note that both the start and stop of the slice are included.
    >>> geo_dataframe.loc[geo_dataframe['skey'] == 1005, 'skey':'polygons']
                             linestrings                                                                                       
    skey
    1005  LINESTRING (1 3 6,3 0 6,6 0 1)  POLYGON ((0 0 0,0 0 20.435,0.0 20.435 0,0.0 20.435 20.435,20.435 0.0 0,20.435 0.0 20.
    # Example 11: Slice a teradataml GeoDataFrame based on a conditional expression
    #             and boolean array for column access.
    >>> geo_dataframe.loc[geo_dataframe['skey'] == 1005, [True, False, True]]
    skey
    1005  POLYGON ((0 0 0,0 0 20.435,0.0 20.435 0,0.0 20.435 20.435,20.435 0.0 0,20.435 0.0 20.435,20.435 20.435 0,20.435 20.43
    >>>
shape
teradataml.geospatial.geodataframe.GeoDataFrame.shape
Returns a tuple representing the dimensionality of the GeoDataFrame.
PARAMETERS:
    None
RETURNS:
    Tuple representing the dimensionality of this GeoDataFrame.
RAISES:
    TeradataMlException (TDMLDF_INFO_ERROR)
EXAMPLES:
    >>> load_example_data("geodataframe","sample_streets")
    >>> df = GeoDataFrame('sample_streets')
    >>> df.shape
    (2, 3)
    >>>
size
teradataml.geospatial.geodataframe.GeoDataFrame.size
Returns a value representing the number of elements in the GeoDataFrame.
PARAMETERS:
    None
RETURNS:
    Value representing the number of elements in the GeoDataFrame.
RAISES:
    None
EXAMPLES:
    >>> load_example_data("geodataframe","sample_streets")
    >>> df = GeoDataFrame('sample_streets')
    >>> df.size
    6
    >>>
tdtypes
teradataml.geospatial.geodataframe.GeoDataFrame.tdtypes
DESCRIPTION:
    Get the teradataml GeoDataFrame metadata containing column names and
    corresponding teradatasqlalchemy types.
RETURNS:
    MetaData containing the column names and Teradata types
RAISES:
    None.
EXAMPLES:
    >>> load_example_data("geodataframe","sample_streets")
    >>> df = GeoDataFrame('sample_streets')
    >>> print(df.tdtypes)
    skey                                      INTEGER()
    street_name     VARCHAR(length=40, charset='LATIN')
    street_shape                             GEOMETRY()
    >>>
Geospatial Speciﬁc Properties
Properties for All Types of Geometries
boundary
teradataml.geospatial.geodataframe.GeoDataFrame.boundary
DESCRIPTION:
    Returns the boundary of the Geometry value.
    Notes:
        The boundary of a Geometry value is a set of Geometry values of the
        next lower dimension.
        +=====================================+=====================================+
        | IF the Geometry represents ...      | THEN the boundary ...               |
        +=====================================+=====================================+
        | an ST_Point or ST_MulitPoint value  | is the empty set.                   |
        +-------------------------------------+-------------------------------------+
        | a nonclosed ST_LineString or        | consists of the start and end       |
        | GeoSequence value                   | ST_Point values.                    |
        +-------------------------------------+-------------------------------------+
        | a closed ST_LineString or           | is empty.                           |
        | GeoSequence value                   |                                     |
        +-------------------------------------+-------------------------------------+
        | an ST_MultiLineString value         | that are in the boundaries of an    |
        | consists of ST_Point values         | odd number of its element           |
        |                                     | ST_LineString values.               |
        +-------------------------------------+-------------------------------------+
        | an ST_Polygon value                 | consists of its set of linear rings |
        +-------------------------------------+-------------------------------------+
        | an ST_MultiPolygon value            | consists of the set of linear rings |
        |                                     |  of its ST_Polygon values.          |
        +-------------------------------------+-------------------------------------+
        | an ST_GeomCollection where the      | consists of geometry values drawn   |
        | interiors of the geometries in the  | from the boundaries of the element  |
        | collection are disjoint             | geometries, where the geometry      |
        |                                     | values are in the boundaries of an  |
        |                                     | odd number of element geometries.   |
        +=====================================+=====================================+
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
    Note:
        This method can be called on 3D geometries (those that include Z
        coordinates). However, the Z coordinate is ignored in method
        calculations. Consequently, any Z coordinates returned by this
        method should be ignored. Teradata recommends using the make_2D()
        method to strip out the Z coordinates of the return value.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame with result column containing Geometry values
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    gdf = GeoDataFrame("sample_shapes")
    gdf = gdf.select(["skey", "polygons", "linestrings"])
    gdf
    # Example 1: Get the boundary of all Polygon geometries in column 'polygons'.
    # Let's check where the GeoDataFrame.geometry property points to.
    print(gdf.geometry.name)
    # Note: By default, GeoDataFrame.geometry property points to the first geometry column,
    #       in this case it is 'polygons'. Thus, no need to set it again.
    gdf.boundary
centroid
teradataml.geospatial.geodataframe.GeoDataFrame.centroid
DESCRIPTION:
    Returns the mathematical centroid of an ST_Polygon or ST_MultiPolygon
    value.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    ST_Polygon or ST_MultiPolygon.
    Note:
        This method can be called on 3D geometries (those that include Z
        coordinates). However, the Z coordinate is dropped, and only x and y
        coordinates are returned.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame with result column containing ST_Point Geometry values
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    gdf = GeoDataFrame("sample_shapes")
    gdf = gdf.select(["skey", "polygons", "linestrings"])[gdf.skey.isin([1001, 1002, 1003])]
    gdf
    # Example 1: Get the centroid of all Polygon geometries in column 'polygons'.
    # Let's check where the GeoDataFrame.geometry property points to.
    print(gdf.geometry.name)
    # Note: By default, GeoDataFrame.geometry property points to the first geometry column,
    #       in this case it is 'polygons'. Thus, no need to set it again.
    gdf.centroid
convex_hull
teradataml.geospatial.geodataframe.GeoDataFrame.convex_hull
DESCRIPTION:
    Returns the convex hull of the Geometry value.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
    Note:
        This method can be called on 3D geometries (those that include Z
        coordinates). However, the Z coordinate is ignored in method
        calculations. Consequently, any Z coordinates returned by this
        method should be ignored. Teradata recommends using the make_2D()
        method to strip out the Z coordinates of the return value.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame with result column containing Geometry values
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    gdf = GeoDataFrame("sample_shapes")
    gdf = gdf.select(["skey", "polygons", "linestrings"])
    gdf
    # Example 1: Get the convex hull of all geometries in column 'polygons'.
    # Let's check where the GeoDataFrame.geometry property points to.
    print(gdf.geometry.name)
    # Note: By default, GeoDataFrame.geometry property points to the first geometry column,
    #       in this case it is 'polygons'. Thus, no need to set it again.
    gdf.convex_hull
coord_dim
teradataml.geospatial.geodataframe.GeoDataFrame.coord_dim
DESCRIPTION:
    Returns the coordinate dimension of a geometry.
    Note:
        *  The coordinate dimension is the same as the coordinate dimension of
          the spatial reference system for the Geometry value.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
    Resultant column contains:
        * 1, if the input geometry is 1D
        * 2, if the input geometry is 2D
        * 3, if the input geometry is 3D
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    gdf = GeoDataFrame("sample_shapes")
    gdf = gdf.select(["skey", "polygons", "linestrings"])
    gdf
    # Example 1: Get the coordinate dimension of all geometries in column 'polygons'.
    # Let's check where the GeoDataFrame.geometry property points to.
    print(gdf.geometry.name)
    # Note: By default, GeoDataFrame.geometry property points to the first geometry column,
    #       in this case it is 'polygons'. Thus, no need to set it again.
    gdf.coord_dim
dimension
teradataml.geospatial.geodataframe.GeoDataFrame.dimension
DESCRIPTION:
    Returns the dimension of the Geometry type.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
    Resultant column contains:
        * 0, for a 0-dimensional geometry
        * 1, for a 1D geometry
        * 2, for a 2D geometry
        * -1, if the input geometry is empty
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    gdf = GeoDataFrame("sample_shapes")
    gdf = gdf.select(["skey", "polygons", "linestrings"])
    gdf
    # Example 1: Get the dimension of all geometries in column 'polygons'.
    # Let's check where the GeoDataFrame.geometry property points to.
    print(gdf.geometry.name)
    # Note: By default, GeoDataFrame.geometry property points to the first geometry column,
    #       in this case it is 'polygons'. Thus, no need to set it again.
    gdf.dimension
geom_type
teradataml.geospatial.geodataframe.GeoDataFrame.geom_type
DESCRIPTION:
    Returns the Geometry type of the Geometry value.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
    Resultant column contains any of the following strings:
        * 'ST_Point'
        * 'ST_LineString'
        * 'ST_Polygon'
        * 'ST_MultiPoint'
        * 'ST_MultiLineString'
        * 'ST_MultiPolygon'
        * 'ST_GeomCollection'
        * 'GeoSequence'
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    gdf = GeoDataFrame("sample_shapes")
    gdf = gdf.select(["skey", "polygons", "linestrings"])
    gdf
    # Example 1: Get the geometry type of all geometries in column 'polygons'.
    # Let's check where the GeoDataFrame.geometry property points to.
    print(gdf.geometry.name)
    # Note: By default, GeoDataFrame.geometry property points to the first geometry column,
    #       in this case it is 'polygons'. Thus, no need to set it again.
    gdf.geom_type
is_3D
teradataml.geospatial.geodataframe.GeoDataFrame.is_3D
DESCRIPTION:
    Tests if a Geometry value has Z coordinate value.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
    Resultant column contains:
        * 1, if the Geometry contains Z coordinates
        * 0, if the Geometry does not contain Z coordinates
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    gdf = GeoDataFrame("sample_shapes")
    gdf = gdf.select(["skey", "polygons", "linestrings"])
    gdf
    # Example 1: Check if geometries in column 'polygons' is 3D or not.
    # Let's check where the GeoDataFrame.geometry property points to.
    print(gdf.geometry.name)
    # Note: By default, GeoDataFrame.geometry property points to the first geometry column,
    #       in this case it is 'polygons'. Thus, no need to set it again.
    gdf.is_3D
is_empty
teradataml.geospatial.geodataframe.GeoDataFrame.is_empty
DESCRIPTION:
    Tests if a Geometry value corresponds to the empty set.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
    Resultant column contains:
        * 1, if the geometry is empty
        * 0, if the geometry is not empty
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    gdf = GeoDataFrame("sample_shapes")
    gdf = gdf.select(["skey", "polygons", "linestrings"])
    gdf
    # Example 1: Check if geometries in column 'polygons' is empty or not.
    # Let's check where the GeoDataFrame.geometry property points to.
    print(gdf.geometry.name)
    # Note: By default, GeoDataFrame.geometry property points to the first geometry column,
    #       in this case it is 'polygons'. Thus, no need to set it again.
    gdf.is_empty
is_simple
teradataml.geospatial.geodataframe.GeoDataFrame.is_simple
DESCRIPTION:
    Tests if a Geometry value has no anomalous geometric points, such as
    self intersection tangency.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
    Resultant column contains:
        * 1, if the geometry is simple, with no anomalous points
        * 0, if the geometry is not simple
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    gdf = GeoDataFrame("sample_shapes")
    gdf = gdf.select(["skey", "polygons", "linestrings"])
    gdf
    # Example 1: Check if geometries in column 'polygons' is simple or not.
    # Let's check where the GeoDataFrame.geometry property points to.
    print(gdf.geometry.name)
    # Note: By default, GeoDataFrame.geometry property points to the first geometry column,
    #       in this case it is 'polygons'. Thus, no need to set it again.
    gdf.is_simple
is_valid
teradataml.geospatial.geodataframe.GeoDataFrame.is_valid
DESCRIPTION:
    Tests if a Geometry value is well-formed.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
    Note:
        This method can be called on 3D geometries (those that include Z
        coordinates). However, the Z coordinate is ignored in method
        calculations.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
    Resultant column contains:
        * 1, if the geometry is valid
        * 0, if the geometry is not valid
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    gdf = GeoDataFrame("sample_shapes")
    gdf = gdf.select(["skey", "polygons", "linestrings"])
    gdf
    # Example 1: Check if geometries in column 'polygons' is valid or not.
    # Let's check where the GeoDataFrame.geometry property points to.
    print(gdf.geometry.name)
    # Note: By default, GeoDataFrame.geometry property points to the first geometry column,
    #       in this case it is 'polygons'. Thus, no need to set it again.
    gdf.is_valid
max_x
teradataml.geospatial.geodataframe.GeoDataFrame.max_x
DESCRIPTION:
    Returns the maximum X coordinate of a Geometry value.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
    Resultant column contains a NULL, if the Geometry is an empty set.
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    gdf = GeoDataFrame("sample_shapes")
    gdf = gdf.select(["skey", "polygons", "linestrings"])
    gdf
    # Example 1: Get the maximum X coordinate of all Polygon geometries in column 'polygons'.
    # Let's check where the GeoDataFrame.geometry property points to.
    print(gdf.geometry.name)
    # Note: By default, GeoDataFrame.geometry property points to the first geometry column,
    #       in this case it is 'polygons'. Thus, no need to set it again.
    gdf.max_x
max_y
teradataml.geospatial.geodataframe.GeoDataFrame.max_y
DESCRIPTION:
    Returns the maximum Y coordinate of a Geometry value.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
    Resultant column contains a NULL, if the Geometry is an empty set.
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    gdf = GeoDataFrame("sample_shapes")
    gdf = gdf.select(["skey", "polygons", "linestrings"])
    gdf
    # Example 1: Get the maximum Y coordinate of all Polygon geometries in column 'polygons'.
    # Let's check where the GeoDataFrame.geometry property points to.
    print(gdf.geometry.name)
    # Note: By default, GeoDataFrame.geometry property points to the first geometry column,
    #       in this case it is 'polygons'. Thus, no need to set it again.
    gdf.max_y
max_z
teradataml.geospatial.geodataframe.GeoDataFrame.max_z
DESCRIPTION:
    Returns the maximum Z coordinate of a Geometry value.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
    Resultant column contains a NULL, if the Geometry is an empty set.
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    gdf = GeoDataFrame("sample_shapes")
    gdf = gdf.select(["skey", "polygons", "linestrings"])
    gdf
    # Example 1: Get the maximum Z coordinate of all Polygon geometries in column 'polygons'.
    # Let's check where the GeoDataFrame.geometry property points to.
    print(gdf.geometry.name)
    # Note: By default, GeoDataFrame.geometry property points to the first geometry column,
    #       in this case it is 'polygons'. Thus, no need to set it again.
    gdf.max_z
min_x
teradataml.geospatial.geodataframe.GeoDataFrame.min_x
DESCRIPTION:
    Returns the minimum X coordinate of a Geometry value.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
    Resultant column contains a NULL, if the Geometry is an empty set.
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    gdf = GeoDataFrame("sample_shapes")
    gdf = gdf.select(["skey", "polygons", "linestrings"])
    gdf
    # Example 1: Get the minimum X coordinate of all Polygon geometries in column 'polygons'.
    # Let's check where the GeoDataFrame.geometry property points to.
    print(gdf.geometry.name)
    # Note: By default, GeoDataFrame.geometry property points to the first geometry column,
    #       in this case it is 'polygons'. Thus, no need to set it again.
    gdf.min_x
min_y
teradataml.geospatial.geodataframe.GeoDataFrame.min_y
DESCRIPTION:
    Returns the minimum Y coordinate of a Geometry value.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
    Resultant column contains a NULL, if the Geometry is an empty set.
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    gdf = GeoDataFrame("sample_shapes")
    gdf = gdf.select(["skey", "polygons", "linestrings"])
    gdf
    # Example 1: Get the minimum Y coordinate of all Polygon geometries in column 'polygons'.
    # Let's check where the GeoDataFrame.geometry property points to.
    print(gdf.geometry.name)
    # Note: By default, GeoDataFrame.geometry property points to the first geometry column,
    #       in this case it is 'polygons'. Thus, no need to set it again.
    gdf.min_y
min_z
teradataml.geospatial.geodataframe.GeoDataFrame.min_z
DESCRIPTION:
    Returns the minimum Z coordinate of a Geometry value.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
    Resultant column contains a NULL, if the Geometry is an empty set.
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    gdf = GeoDataFrame("sample_shapes")
    gdf = gdf.select(["skey", "polygons", "linestrings"])
    gdf
    # Example 1: Get the minimum Z coordinate of all Polygon geometries in column 'polygons'.
    # Let's check where the GeoDataFrame.geometry property points to.
    print(gdf.geometry.name)
    # Note: By default, GeoDataFrame.geometry property points to the first geometry column,
    #       in this case it is 'polygons'. Thus, no need to set it again.
    gdf.min_z
srid
teradataml.geospatial.geodataframe.GeoDataFrame.srid
DESCRIPTION:
    Get the spatial reference system identifier of the Geometry value.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    gdf = GeoDataFrame("sample_shapes")
    gdf = gdf.select(["skey", "polygons", "linestrings"])
    gdf
    # Example 1: Get the spatial reference system identifier of all geometries in column 'polygons'.
    # Let's check where the GeoDataFrame.geometry property points to.
    print(gdf.geometry.name)
    # Note: By default, GeoDataFrame.geometry property points to the first geometry column,
    #       in this case it is 'polygons'. Thus, no need to set it again.
    gdf.srid
Properties for Point Geometry
x
teradataml.geospatial.geodataframe.GeoDataFrame.x
DESCRIPTION:
    Get the X coordinate of an ST_Point value.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    ST_Point
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    gdf = GeoDataFrame("sample_shapes")
    gdf = gdf.select(["skey", "points", "linestrings"])[gdf.points.geom_type == "ST_Point"]
    gdf
    # Example 1: Get the X coordinate of all geometries in column "points".
    # Let's check where the GeoDataFrame.geometry property points to.
    print(gdf.geometry.name)
    # Note: By default, GeoDataFrame.geometry property points to the first geometry column,
    #       in this case it is "points". Thus, no need to set it again.
    gdf.x
y
teradataml.geospatial.geodataframe.GeoDataFrame.y
DESCRIPTION:
    Get the Y coordinate of an ST_Point value.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    ST_Point
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    gdf = GeoDataFrame("sample_shapes")
    gdf = gdf.select(["skey", "points", "linestrings"])[gdf.points.geom_type == "ST_Point"]
    gdf
    # Example 1: Get the Y coordinate of all geometries in column "points".
    # Let's check where the GeoDataFrame.geometry property points to.
    print(gdf.geometry.name)
    # Note: By default, GeoDataFrame.geometry property points to the first geometry column,
    #       in this case it is "points". Thus, no need to set it again.
    gdf.y
z
teradataml.geospatial.geodataframe.GeoDataFrame.z
DESCRIPTION:
    Get the Z coordinate of an ST_Point value.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    ST_Point
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    gdf = GeoDataFrame("sample_shapes")
    gdf = gdf.select(["skey", "points", "linestrings"])[(gdf.points.is_3D == 1) &
    (gdf.points.geom_type == "ST_Point")]
    gdf
    # Example 1: Get the Z coordinate of all geometries in column "points".
    # Let's check where the GeoDataFrame.geometry property points to.
    print(gdf.geometry.name)
    # Note: By default, GeoDataFrame.geometry property points to the first geometry column,
    #       in this case it is "points". Thus, no need to set it again.
    gdf.z
Properties for LineString Geometry
is_closed_3D
teradataml.geospatial.geodataframe.GeoDataFrame.is_closed_3D
DESCRIPTION:
    Tests whether a 3D LineString or 3D MultiLineString is closed, taking
    into account the Z coordinates in the calculation.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    LineString and MultiLineString types that include Z coordinates
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
    Resultant column contains:
        * 1, if the 3D LineString or 3D MultiLineString is closed.
        * 0, if the 3D LineString or 3D MultiLineString is not closed or is empty.
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    gdf = GeoDataFrame("sample_shapes")
    # Select few columns and filter rows containing only 3D geometries in 'linestrings' column.
    gdf = gdf.select(["skey", "polygons", "linestrings"])[gdf.linestrings.is_3D == 1]
    gdf
    # Example 1: Check if 3D geometries in column "linestings" is are closed or not.
    # Let's check where the GeoDataFrame.geometry property points to.
    print(gdf.geometry.name)
    # Note: By default, GeoDataFrame.geometry property points to the first geometry column,
    #       in this case it is 'polygons'.
    # Set the geometry property to 'linestrings' column.
    gdf.geometry = gdf.linestrings
    gdf.is_closed_3D
is_closed
teradataml.geospatial.geodataframe.GeoDataFrame.is_closed
DESCRIPTION:
    Tests if a Geometry type that represents an ST_LineString, GeoSequence,
    or ST_MultiLineString value is closed.
    Note:
        *  An ST_LineString value is closed if the start point of the
          ST_LineString value is equal to the end point.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    ST_LineString, GeoSequence, and ST_MultiLineString
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
    Resultant column contains:
        * 1, if the ST_LineString, GeoSequence, or ST_LineString components
          of an ST_MultiLineString are closed.
        * 0, if the ST_LineString, GeoSequence, or ST_LineString components
          of an ST_MultiLineString are not closed, or if the input geometry
          is empty.
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    gdf = GeoDataFrame("sample_shapes")
    gdf = gdf.select(["skey", "polygons", "linestrings"])[gdf.skey.isin([1001, 1002, 1003])]
    gdf
    # Example 1: Check if geometries in column 'linestrings' is are closed or not.
    # Let's check where the GeoDataFrame.geometry property points to.
    print(gdf.geometry.name)
    # Note: By default, GeoDataFrame.geometry property points to the first geometry column,
    #       in this case it is 'polygons'.
    # Set the geometry property to 'linestrings' column.
    gdf.geometry = gdf.linestrings
    gdf.is_closed
is_ring
teradataml.geospatial.geodataframe.GeoDataFrame.is_ring
DESCRIPTION:
    Tests if a Geometry type that represents an ST_LineString or a
    GeoSequence value is a ring.
    Note:
        *  is_ring returns zero if the geometry value passed to it is an
          empty set.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    ST_LineString or GeoSequence
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
    Resultant column contains:
        * 1, if the ST_LineString or GeoSequence value is simple (has no
          anomalous geometric points, such as self intersection tangency)
          and is closed (the start point of the ST_LineString value is
          equal to the end point)
        * 0, in all other cases.
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    gdf = GeoDataFrame("sample_shapes")
    gdf = gdf.select(["skey", "polygons", "linestrings"])[~gdf.skey.isin([1009, 1008, 1010, 1007])]
    gdf
    # Example 1: Check if geometries in column 'linestrings' is ring or not.
    # Let's check where the GeoDataFrame.geometry property points to.
    print(gdf.geometry.name)
    # Note: By default, GeoDataFrame.geometry property points to the first geometry column,
    #       in this case it is 'polygons'.
    # Set the geometry property to 'linestrings' column.
    gdf.geometry = gdf.linestrings
    gdf.is_ring
Properties for Polygon Geometry
area
teradataml.geospatial.geodataframe.GeoDataFrame.area
DESCRIPTION:
    Returns the area measurement of an ST_Polygon or ST_MultiPolygon. For
    ST_MultiPolygon, returns the sum of the area measurements of the
    component polygons.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    ST_Polygon and ST_MultiPolygon
    Note:
        This method can be called on 3D geometries (those that include Z
        coordinates). However, the Z coordinate is ignored in method
        calculations. Consequently, any Z coordinates returned by this
        method should be ignored. Teradata recommends using the make_2D()
        method to strip out the Z coordinates of the return value.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    gdf = GeoDataFrame("sample_shapes")
    gdf = gdf.select(["skey", "polygons", "linestrings"])
    gdf
    # Example 1: Get the area of all Polygon geometries in column 'polygons'.
    # Let's check where the GeoDataFrame.geometry property points to.
    print(gdf.geometry.name)
    # Note: By default, GeoDataFrame.geometry property points to the first geometry column,
    #       in this case it is 'polygons'. Thus, no need to set it again.
    gdf.area
exterior
teradataml.geospatial.geodataframe.GeoDataFrame.exterior
DESCRIPTION:
    Get the exterior ring of a Geometry type that represents an
    ST_Polygon value.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    ST_Polygon
    Note:
        This method can be called on 3D geometries (those that include Z
        coordinates). However, the Z coordinate is ignored in method
        calculations. Consequently, any Z coordinates returned by this
        method should be ignored. Teradata recommends using the make_2D()
        method to strip out the Z coordinates of the return value.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame with result column containing ST_LineString Geometry values
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    gdf = GeoDataFrame("sample_shapes")
    gdf = gdf.select(["skey", "polygons", "linestrings"])[gdf.skey.isin([1001, 1002, 1003])]
    gdf
    # Example 1: Get the exterior ring of all Polygon geometries in column 'polygons'.
    # Let's check where the GeoDataFrame.geometry property points to.
    print(gdf.geometry.name)
    # Note: By default, GeoDataFrame.geometry property points to the first geometry column,
    #       in this case it is 'polygons'. Thus, no need to set it again.
    gdf.exterior
perimeter
teradataml.geospatial.geodataframe.GeoDataFrame.perimeter
DESCRIPTION:
    Returns the boundary length of an ST_Polygon, or the sum of the boundary
    lengths of the component polygons of an ST_MultiPolygon.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    ST_Polygon or ST_MultiPolygon
    Note:
        This method can be called on 3D geometries (those that include Z
        coordinates). However, the Z coordinate is ignored in method
        calculations.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    gdf = GeoDataFrame("sample_shapes")
    gdf = gdf.select(["skey", "polygons", "linestrings"])
    gdf
    # Example 1: Get the perimeter of all geometries in column 'polygons'.
    # Let's check where the GeoDataFrame.geometry property points to.
    print(gdf.geometry.name)
    # Note: By default, GeoDataFrame.geometry property points to the first geometry column,
    #       in this case it is 'polygons'. Thus, no need to set it again.
    gdf.perimeter
Methods
__getattr__
teradataml.geospatial.geodataframe.GeoDataFrame.__getattr__ = __getattr__(self, name)
Returns an attribute of the GeoDataFrame.
PARAMETERS:
    name: 
        Required Argument. 
        Specifies the name of the attribute.
        Types: str
RETURNS:
    Return the value of the named attribute of object (if found).
RAISES:
    Attribute Error when the named attribute is not found
EXAMPLES:
    >>> from teradataml import GeoDataFrame
    >>> load_example_data("geodataframe", "sample_cities")
    >>> gdf = GeoDataFrame('sample_cities')
    # You can access a column from the GeoDataFrame
    >>> gdf.city_shape
__getitem__
Docs.GeospatialDocs.GeoDataFrameInheritedDocumentation.GeoDataFrame.__getitem__ = __getitem__(self, key)
Return a column from the GeoDataFrame or filter the GeoDataFrame using an expression
or select columns from a GeoDataFrame using the specified list of columns.
The following operators are supported:
    comparison: ==, !=, <, <=, >, >=
    boolean: & (and), | (or), ~ (not), ^ (xor)
Operands can be Python literals and instances of ColumnExpressions from the GeoDataFrame.
PARAMETERS:
    key:
        Required Argument.
        Specifies the column name(s) of filter expression (ColumnExpression).
        Specify key as:
            * a string - to get the column of a teradataml GeoDataFrame, i.e., a ColumnExpression.
            * list of strings - to select columns of a teradataml GeoDataFrame.
            * filter expression - to filter the data from teradataml GeoDataFrame using the specified expression.
        Types: str or list of strings or ColumnExpression
RETURNS:
    GeoDataFrame or ColumnExpression instance
RAISES:
    1. KeyError   - If key is not found
    2. ValueError - When columns of different GeoDataFrames are given in ColumnExpression.
EXAMPLES:
    # Load the example data.
    load_example_data("geodataframe", ["sample_shapes"])
    # Create teradataml DataFrame object.
    gdf = DataFrame('sample_shapes')
    # Filter the GeoDataFrame gdf.
    gdf[gdf.points > gdf.linestrings]
    # Import GeometryType classes.
    from teradataml import Point, LineString, Polygon
    gdf[gdf.points > Point(1, 3, 5)]
    gdf[gdf.linestrings == LineString([(1, 3), (3, 0), (0, 1)])]
    p1 = Polygon([(0.6, 0.8), (0.6, 20.8), (20.6, 20.8), (20.6, 0.8), (0.6, 0.8)])
    gdf[p1 != gdf.polygons]
    gdf[~(p1 < gdf.polygons)]
    gdf[(gdf.polygons < p1) & (gdf.geom_collections != None)]
    # Retrieve column geosequence from gdf.
    gdf["geosequence"]
    # Select columns using list of column names from a teradataml GeoDataFrame.
    gdf[["points", "linestrings", "polygons"]]
__init__
teradataml.geospatial.geodataframe.GeoDataFrame.__init__ = __init__(self, table_name=None, index=True, index_label=None, query=None, materialize=False)
Constructor for teradataml GeoDataFrame.
Note: 
    Use GeoDataFrame only when table or query result contains a Geometry data, i.e., 
    column of teradatasqlalchemy types: Geometry(ST_Geometry in SQL), MBB, MBR.
    Otherwise, an exception is raised. 
PARAMETERS:
    table_name:
        Optional Argument.
        The table name or view name in Teradata Vantage referenced by this GeoDataFrame.
        Types: str
    index:
        Optional Argument.
        True if using index column for sorting, otherwise False.
        Default Value: True
        Types: bool
    index_label:
        Optional Argument.
        Column/s used for sorting.
        Types: str OR list of Strings (str)
    query:
        Optional Argument.
        SQL query for this GeoDataFrame. Used by class method from_query.
        Types: str
    materialize:
        Optional Argument.
        Whether to materialize GeoDataFrame or not when created.
        Used by class method from_query.
        You should use materialization, when the query  passed to from_query(),
        is expected to produce non-deterministic results, when it is executed multiple
        times. Using this option will help user to have deterministic results in the
        resulting teradataml GeoDataFrame.
        Default Value: False (No materialization)
        Types: bool
RAISES:
    TeradataMlException - TDMLDF_CREATE_FAIL
EXAMPLES:
    >>> from teradataml import GeoDataFrame
    >>> load_example_data("geodataframe","sample_streets")
    >>> gdf = GeoDataFrame("sample_streets")
    >>> gdf
          street_name              street_shape
    skey
    1      Coast Blvd  LINESTRING (12 12,18 17)
    1     Main Street  LINESTRING (2 2,3 2,4 1)
    >>>
__repr__
teradataml.geospatial.geodataframe.GeoDataFrame.__repr__ = __repr__(self)
Returns the string representation for a teradataml GeoDataFrame instance.
The string contains:
    1. Column names of the GeoDataFrame.
    2. At most the first no_of_rows rows of the GeoDataFrame.
    3. A default index for row numbers.
NOTES:
  - This makes an explicit call to get rows from the database.
  - To change number of rows to be printed set the max_rows option in
    "options.display.display".
  - Default value of max_rows is 10.
EXAMPLES:
    gdf = GeoDataFrame.from_table("table1")
    print(gdf)
    gdf = GeoDataFrame.from_query("select col1, col2, col3 from table1")
    print(gdf)
alias
teradataml.geospatial.geodataframe.GeoDataFrame.alias = alias(self, alias_name)
DESCRIPTION:
    Method to create an aliased teradataml GeoDataFrame.
    Note:
        * This method is recommended to be used before performing
          self join using GeoDataFrame's join() API.
PARAMETERS:
    alias_name:
        Required Argument.
        Specifies the alias name to be assigned to a teradataml GeoDataFrame.
        Types: str
RETURNS:
    teradataml GeoDataFrame
EXAMPLES:
    >>> load_example_data("geodataframe", "sample_shapes")
    >>> gdf = GeoDataFrame("sample_shapes")
    >>> geo_df = gdf.select(["skey", "points", "linestrings"])
    >>> display.geometry_column_length = 100
    # Example 1: Create an alias of teradataml GeoDataFrame.
    >>> geo_df2 = geo_df.alias("rhs_gdf")
    # Print aliased GeDataFrame.
    >>> geo_df2
                                                                         points                                                
    skey
    1001                                                          POINT (10 20)                                                
    1007                                     MULTIPOINT (1 1,1 3,6 3,10 5,20 1)                                              MU
(10 5,20 1))
    1010  MULTIPOINT (10.345 20.32 30.6,40.234 50.23 60.24,70.234 80.56 80.234)  MULTILINESTRING ((1 3 6,3 0 6,6 0 1),
(1.35 3.6456 4.5,3.6756 0.23 6.8,0.345 1.756 8.9))
    1008       MULTIPOINT (1.65 1.76,1.23 3.76,6.23 3.78,10.76 5.9,20.32 1.231)                    MULTILINESTRING ((1 3,3 0,0 
(1.35 3.6456,3.6756 0.23,0.345 1.756))
    1003                                                  POINT (235.52 54.546)                                         LINESTR
    1002                                                            POINT (1 3)                                                
    1004                                                       POINT (10 20 30)                                                
    1009                                MULTIPOINT (10 20 30,40 50 60,70 80 80)                              MULTILINESTRING ((
(70 80 80,90 100 110))
    1005                                                          POINT (1 3 5)                                                
    1006                                           POINT (235.52 54.546 7.4564)                             LINESTRING (1.35 3.
assign
teradataml.geospatial.geodataframe.GeoDataFrame.assign = assign(self, drop_columns=False, **kwargs)
DESCRIPTION:
    Assign new columns to a teradataml GeoDataFrame.
PARAMETERS:
    drop_columns:
        Optional Argument.
        If True, drop columns that are not specified in assign.
        Note:
            When GeoDataFrame.assign() is run on GeoDataFrame.groupby(), this argument
            is ignored. In such cases, all columns are dropped and only new columns
            and grouping columns are returned.
        Default Value: False
        Types: bool
    kwargs:
        Specifies keyword-value pairs.
        - keywords are the column names.
        - values can be:
            * Column arithmetic expressions.
            * int/float/string literals.
            * GeoDataFrameColumn a.k.a. ColumnExpression Functions.
              (See GeoDataFrameColumn Functions in Function reference guide for more
               details)
            * DataFrameColumn a.k.a. ColumnExpression Functions.
              (See DataFrameColumn Functions in Function reference guide for more
               details)
            * SQLAlchemy ClauseElements.
              (See teradataml extension with SQLAlchemy in teradataml User Guide
               and Function reference guide for more details)
RETURNS:
    teradataml GeoDataFrame
    A new GeoDataFrame is returned with:
        1. New columns in addition to all the existing columns if "drop_columns" is False.
        2. Only new columns if "drop_columns" is True.
        3. New columns in addition to group by columns, i.e., columns used for grouping,
           if assign() is run on GeoDataFrame.groupby().
NOTES:
     1. The values in kwargs cannot be callable (functions).
     2. The original GeoDataFrame is not modified.
     3. Since ``kwargs`` is a dictionary, the order of your
       arguments may not be preserved. To make things predictable,
       the columns are inserted in alphabetical order, after the existing columns
       in the GeoDataFrame. Assigning multiple columns within the same ``assign`` is
       possible, but you cannot reference other columns created within the same
       ``assign`` call.
     4. The maximum number of columns that a GeoDataFrame can have is 2048.
     5. With GeoDataFrame.groupby(), only aggregate functions and literal values
       are advised to use. Other functions, such as string functions, can also be
       used, but the column used in such function must be a part of group by columns.
       See examples for teradataml extension with SQLAlchemy on using various
       functions with GeoDataFrame.assign().
RAISES:
     1. ValueError - When a callable is passed as a value, or columns from different
                     GeoDataFrames are passed as values in kwargs.
     2. TeradataMlException - When the return GeoDataFrame initialization fails, or
                              invalid argument types are passed.
EXAMPLES:
    >>> load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame
    >>> gdf = GeoDataFrame("sample_shapes")
    >>> gdf = gdf.select(["skey", "points", "polygons"])
    >>>
    # In the example here, we will run following:
    #     1. Calculate the area of polygons.
    #     2. Get the dimensions of geometries in polygons.
    #     3. Check whether geometry in column polygons is 3D or not and they are valid or not.
    #     4. Get the max values for x, y and z coordinates in geometries in column "points".
    >>> gdf.assign(drop_columns=True,
    ...            skey = gdf.skey,
    ...            pol_area = gdf.polygons.area,
    ...            pol_dim = gdf.polygons.dimensions,
    ...            is_pol_3d = gdf.polygons.is_3D,
    ...            is_pol_valid = gdf.polygons.is_valid,
    ...            point_maxx = gdf.points.max_x,
    ...            point_maxy = gdf.points.max_y,
    ...            point_maxz = gdf.points.max_z
    ...           )
          is_pol_3d  is_pol_valid  point_maxx  point_maxy  point_maxz  pol_area pol_dim
    skey
    1006          1             0     235.520      54.546      7.4564       0.0    None
    1001          0             1      10.000      20.000         NaN     400.0    None
    1002          0             1       1.000       3.000         NaN     375.0    None
    1010          1             0      70.234      80.560     80.2340       0.0    None
    1004          1             1      10.000      20.000     30.0000     187.5    None
    1003          0             1     235.520      54.546         NaN     400.0    None
    1008          0             0      20.320       5.900         NaN     800.0    None
    1005          1             0       1.000       3.000      5.0000       0.0    None
    1007          0             1      20.000       5.000         NaN      62.5    None
    1009          1             1      70.000      80.000     80.0000    2000.0    None
    >>>
concat
teradataml.geospatial.geodataframe.GeoDataFrame.concat = concat(self, other, join='OUTER', allow_duplicates=True, sort=False, ignore_index=False)
DESCRIPTION:
    Concatenates two teradataml GeoDataFrames along the index axis.
PARAMETERS:
    other:
        Required Argument.
        Specifies the other teradataml DataFrame/GeoDataFrame with which the
        concatenation is to be performed.
        Types: teradataml GeoDataFrame or teradataml DataFrame
    join:
        Optional Argument.
        Specifies how to handle indexes on columns axis.
        Supported values are:
            * 'OUTER': It instructs the function to project all columns from both
                       the GeoDataFrames. Columns not present in either GeoDataFrame
                       will have a SQL NULL value.
            * 'INNER': It instructs the function to project only the columns common
                       to both GeoDataFrames.
        Default value: 'OUTER'
        Permitted values: 'INNER', 'OUTER'
        Types: str
    allow_duplicates:
        Optional Argument.
        Specifies if the result of concatenation can have duplicate rows.
        Default value: True
        Types: bool
    sort:
        Optional Argument.
        Specifies a flag to sort the columns axis if it is not already aligned
        when the join argument is set to 'outer'.
        Default value: False
        Types: bool
    ignore_index:
        Optional argument.
        Specifies whether to ignore the index columns in resulting GeoDataFrame or not.
        If True, then index columns will be ignored in the concat operation.
        Default value: False
        Types: bool
RETURNS:
    teradataml GeoDataFrame
RAISES:
    TeradataMlException
EXAMPLES:
    >>> from teradataml import load_example_data, GeoDataFrame, DataFrame
    >>> load_example_data("geodataframe",["sample_streets", "sample_cities"])
    # Create required GeoDataFrames.
    >>> df1 = GeoDataFrame('sample_streets')
    >>> df1
          street_name              street_shape
    skey
    1      Coast Blvd  LINESTRING (12 12,18 17)
    1     Main Street  LINESTRING (2 2,3 2,4 1)
    >>>
    >>> df2 = GeoDataFrame('sample_cities')
    >>> df2
           city_name                                 city_shape
    skey
    1        Seaside  POLYGON ((10 10,10 20,20 20,20 15,10 10))
    0     Oceanville            POLYGON ((1 1,1 3,6 3,6 0,1 1))
    >>>
    # Example 1: Concat two GeoDataFrames with default options.
    >>> cdf = df1.concat(df2)
    >>> cdf
          street_name              street_shape   city_name                                 city_shape
    skey
    1            None                      None     Seaside  POLYGON ((10 10,10 20,20 20,20 15,10 10))
    0            None                      None  Oceanville            POLYGON ((1 1,1 3,6 3,6 0,1 1))
    1      Coast Blvd  LINESTRING (12 12,18 17)        None                                       None
    1     Main Street  LINESTRING (2 2,3 2,4 1)        None                                       None
    >>>
    # Example 2: Concat two GeoDataFrames with inner join.
    >>> cdf = df1.concat(df2, join='inner')
    >>> cdf
    Empty DataFrame
    Columns: []
    Index: [1, 1, 1, 0]
    >>>
    # Example 3: Concat two GeoDataFrames with by allowing duplicates, if there are any.
    # allow_duplicates = True
    >>> cdf = df1.concat(df2)
    >>> cdf
          street_name              street_shape   city_name                                 city_shape
    skey
    1            None                      None     Seaside  POLYGON ((10 10,10 20,20 20,20 15,10 10))
    0            None                      None  Oceanville            POLYGON ((1 1,1 3,6 3,6 0,1 1))
    1      Coast Blvd  LINESTRING (12 12,18 17)        None                                       None
    1     Main Street  LINESTRING (2 2,3 2,4 1)        None                                       None
    >>> cdf = cdf.concat(df2)
    >>> cdf
          street_name              street_shape   city_name                                 city_shape
    skey
    1     Main Street  LINESTRING (2 2,3 2,4 1)        None                                       None
    1            None                      None     Seaside  POLYGON ((10 10,10 20,20 20,20 15,10 10))
    1            None                      None     Seaside  POLYGON ((10 10,10 20,20 20,20 15,10 10))
    1      Coast Blvd  LINESTRING (12 12,18 17)        None                                       None
    0            None                      None  Oceanville            POLYGON ((1 1,1 3,6 3,6 0,1 1))
    0            None                      None  Oceanville            POLYGON ((1 1,1 3,6 3,6 0,1 1))
    >>>
    # Example 4: Concat two GeoDataFrames but do not allow duplicates, if there are any.
    # allow_duplicates = False
    >>> cdf = cdf.concat(df2, allow_duplicates=False)
    >>> cdf
          street_name              street_shape   city_name                                 city_shape
    skey
    1      Coast Blvd  LINESTRING (12 12,18 17)        None                                       None
    1     Main Street  LINESTRING (2 2,3 2,4 1)        None                                       None
    1            None                      None     Seaside  POLYGON ((10 10,10 20,20 20,20 15,10 10))
    0            None                      None  Oceanville            POLYGON ((1 1,1 3,6 3,6 0,1 1))
    >>>
    # Example 5: Concat two GeoDataFrames with sort set to True.
    >>> cdf = df1.concat(df2, sort=True)
    >>> cdf
           city_name                                 city_shape  street_name              street_shape
    skey
    1        Seaside  POLYGON ((10 10,10 20,20 20,20 15,10 10))         None                      None
    0     Oceanville            POLYGON ((1 1,1 3,6 3,6 0,1 1))         None                      None
    1           None                                       None   Coast Blvd  LINESTRING (12 12,18 17)
    1           None                                       None  Main Street  LINESTRING (2 2,3 2,4 1)
    >>>
    # Example 6: Concat two GeoDataFrames with ignore_index set to True.
    >>> cdf = df1.concat(df2, ignore_index=True)
    >>> cdf
       street_name              street_shape   city_name                                 city_shape
    0         None                      None     Seaside  POLYGON ((10 10,10 20,20 20,20 15,10 10))
    1         None                      None  Oceanville            POLYGON ((1 1,1 3,6 3,6 0,1 1))
    2   Coast Blvd  LINESTRING (12 12,18 17)        None                                       None
    3  Main Street  LINESTRING (2 2,3 2,4 1)        None                                       None
    >>>
    # Example 7: Concat a GeoDataFrame with teradataml DataFrame.
    >>> load_example_data("dataframe", "admissions_train")
    >>> tdf = DataFrame("admissions_train")
    >>> cdf = df1.concat(tdf, ignore_index=True)
    >>> cdf
      street_name street_shape masters   gpa     stats programming  admitted
    0        None         None     yes  2.65  Advanced    Beginner         1
    1        None         None      no  3.44    Novice      Novice         0
    2        None         None      no  1.87  Advanced      Novice         1
    3        None         None      no  3.83  Advanced    Advanced         1
    4        None         None      no  4.00  Advanced      Novice         1
    5        None         None     yes  3.46  Advanced    Beginner         0
    6        None         None      no  3.13  Advanced    Advanced         1
    7        None         None      no  3.82  Advanced    Advanced         1
    8        None         None     yes  3.85  Advanced    Beginner         0
    9        None         None     yes  3.57  Advanced    Advanced         1
    >>>
count
teradataml.geospatial.geodataframe.GeoDataFrame.count = count(self, distinct=False)
DESCRIPTION:
    Returns column-wise count of the GeoDataFrame.
PARAMETERS:
    distinct:
        Optional Argument.
        Specifies whether to exclude duplicate values while calculating the count.
        Default Value: False
        Types: bool
RETURNS:
    teradataml GeoDataFrame object with count() operation
    performed.
RAISES:
    TeradataMLException
    1. EXECUTION_FAILED - If count() operation fails to
        generate the column-wise count of the GeoDataFrame.
        Possible error message:
        Failed to perform 'count'. (Followed by error message).
    2. TDMLDF_AGGREGATE_COMBINED_ERR - If the count() operation
        doesn't support all the columns in the GeoDataFrame.
        Possible error message:
        No results. Below is/are the error message(s):
        All selected columns [(col2 -  PERIOD_TIME), (col3 -
        BLOB)] is/are unsupported for 'count' operation.
EXAMPLES :
    # Load the data to run the example.
    from teradataml.data.load_example_data import load_example_data
    load_example_data("GeoDataFrame", ["sample_shapes"])
    # Create teradataml GeoDataFrame.
    df1 = GeoDataFrame("sample_shapes")
    print(df1)
    # Prints count of the values in all the selected columns
    # (excluding None types).
    df1.count()
drop
teradataml.geospatial.geodataframe.GeoDataFrame.drop = drop(self, labels=None, axis=0, columns=None)
DESCRIPTION:
    Drop specified labels from rows or columns.
    Remove rows or columns by specifying label names and corresponding
    axis, or by specifying the index or column names directly.
PARAMETERS:
    labels:
        Optional Argument. Required when "columns" is not provided.
        Single label or list-like. Can be Index or column labels to drop depending on axis.
        Types: str OR list of Strings (str)
    axis:
        Optional Argument.
        0 or 'index' for index labels
        1 or 'columns' for column labels
        Default Values: 0
        Permitted Values: 0, 1, 'index', 'columns'
        Types: int OR str
    columns:
        Optional Argument. Required when labels is not provided.
        Single label or list-like. This is an alternative to specifying axis=1 with labels.
        Cannot specify both labels and columns.
        Types: str OR list of Strings (str)
RETURNS:
    teradataml GeoDataFrame
RAISE:
    TeradataMlException
EXAMPLES:
    >>> load_example_data("geodataframe","sample_streets")
    >>> df = GeoDataFrame('sample_streets')
    >>> df
          street_name              street_shape
    skey
    1      Coast Blvd  LINESTRING (12 12,18 17)
    1     Main Street  LINESTRING (2 2,3 2,4 1)
    # Drop columns
    >>> df.drop(['street_name', 'skey'], axis=1)
                   street_shape
    0  LINESTRING (12 12,18 17)
    1  LINESTRING (2 2,3 2,4 1)
    >>> df.drop(columns=['street_shape'])
          street_name
    skey
    1      Coast Blvd
    1     Main Street
    >>>
dropna
teradataml.geospatial.geodataframe.GeoDataFrame.dropna = dropna(self, how='any', thresh=None, subset=None)
DESCRIPTION:
    Removes rows with null values.
PARAMETERS:
    how:
        Optional Argument.
        Specifies how rows are removed.
        'any' removes rows with at least one null value.
        'all' removes rows with all null values.
        Default Value: 'any'
        Permitted Values: 'any' or 'all'
        Types: str
    thresh:
        Optional Argument.
        Specifies the minimum number of non null values in a row to include.
        Types: int
    subset:
        Optional Argument.
        Specifies list of column names to include, in array-like format.
        Types: str OR list of Strings (str)
RETURNS:
    teradataml GeoDataFrame
RAISE:
    TeradataMlException
EXAMPLES:
    >>> load_example_data("geodataframe","sample_shapes")
    >>> df = GeoDataFrame('sample_shapes').select(["skey", "geosequence"])
    >>> df
    skey
    1006                                                                                                                       
    1001                           GEOSEQUENCE((10 20,30 40,50 60),(2007-08-22 12:05:23.560000,2007-08-
22 12:08:25.140000,2007-08-22 12:11:41.520000),(1,2,3),(2,10,12,11,18,21,19))
    1002  GEOSEQUENCE((10 10,15 15,-2 0),(2007-03-14 01:35:00.000000,2007-03-14 01:35:05.000000,2007-03-14 01:35:08.000000),
(1222,1223,1224),(2,12.1,3.14159,2.78128,-10,-11,100.1))
    1010                                                                                                                       
    1004                                                                                                                       
    1003                                                                                                                       
    1008                                                                                                                       
    1005                                                                                                                       
    1007                                                                                                                       
    1009                                                                                                                       
    >>>
    # Drop the rows where at least one element is null.
    >>> df.dropna()
    skey
    1002  GEOSEQUENCE((10 10,15 15,-2 0),(2007-03-14 01:35:00.000000,2007-03-14 01:35:05.000000,2007-03-14 01:35:08.000000),
(1222,1223,1224),(2,12.1,3.14159,2.78128,-10,-11,100.1))
    1001                           GEOSEQUENCE((10 20,30 40,50 60),(2007-08-22 12:05:23.560000,2007-08-
22 12:08:25.140000,2007-08-22 12:11:41.520000),(1,2,3),(2,10,12,11,18,21,19))
    >>>
    # Drop the rows where all elements are nulls for columns 'geosequence' and 'skey'.
    >>> df.dropna(how='all', subset=['skey','geosequence'])
    skey
    1008                                                                                                                       
    1003                                                                                                                       
    1009                                                                                                                       
    1007                                                                                                                       
    1005                                                                                                                       
    1001                           GEOSEQUENCE((10 20,30 40,50 60),(2007-08-22 12:05:23.560000,2007-08-
22 12:08:25.140000,2007-08-22 12:11:41.520000),(1,2,3),(2,10,12,11,18,21,19))
    1006                                                                                                                       
    1004                                                                                                                       
    1010                                                                                                                       
    1002  GEOSEQUENCE((10 10,15 15,-2 0),(2007-03-14 01:35:00.000000,2007-03-14 01:35:05.000000,2007-03-14 01:35:08.000000),
(1222,1223,1224),(2,12.1,3.14159,2.78128,-10,-11,100.1))
    >>>
    # Keep only the rows with at least 2 non null values.
    >>> df.dropna(thresh=2)
    skey
    1001                           GEOSEQUENCE((10 20,30 40,50 60),(2007-08-22 12:05:23.560000,2007-08-
22 12:08:25.140000,2007-08-22 12:11:41.520000),(1,2,3),(2,10,12,11,18,21,19))
    1002  GEOSEQUENCE((10 10,15 15,-2 0),(2007-03-14 01:35:00.000000,2007-03-14 01:35:05.000000,2007-03-14 01:35:08.000000),
(1222,1223,1224),(2,12.1,3.14159,2.78128,-10,-11,100.1))
    >>>
ﬁlter
teradataml.geospatial.geodataframe.GeoDataFrame.ﬁlter = ﬁlter(self, items=None, like=None, regex=None, axis=1, **kw)
DESCRIPTION:
    Filter rows or columns of GeoDataFrame according to labels in the specified index.
    The filter is applied to the columns of the index when axis is set to 'rows'.
    Must use one of the parameters 'items', 'like', and 'regex' only.
PARAMETERS:
    axis:
        Optional Argument.
        Specifies the axis to filter on.
        1 denotes column axis (default). Alternatively, 'columns' can be specified.
        0 denotes row axis. Alternatively, 'rows' can be specified.
        Default Values: 1
        Permitted Values: 0, 1, 'rows', 'columns'
        Types: int OR str
    items:
        Optional Argument.
        List of values that the info axis should be restricted to.
        When axis is 1, items are a list of column names.
        When axis is 0, items are a list of literal values.
        Types: list of Strings (str) or literals
    like:
        Optional Argument.
        Specifies the substring pattern.
        When axis is 1, substring pattern for matching column names.
        When axis is 0, substring pattern for checking index values
        with REGEXP_SUBSTR.
        Types: str
    regex:
        Optional Argument.
        Specifies a regular expression pattern.
        When axis is 1, regex pattern for re.search(regex, column_name).
        When axis is 0, regex pattern for checking index values
        with REGEXP_SUBSTR.
        Types: str
    **kw: optional keyword arguments
        varchar_size:
            Specifies an integer to specify the size of varchar-casted index.
            Used when axis=0 or axis='rows' and index must be char-like in "like"
            and "regex" filtering.
            Default Value: configure.default_varchar_size
            Types: int
        match_arg: string
            Specifies argument to pass if axis is 0/'rows' and regex is used.
            Valid values for match_arg are:
            - 'i': case-insensitive matching.
            - 'c': case sensitive matching.
            - 'n': the period character (match any character) can match
                    the newline character.
            - 'm': index value is treated as multiple lines instead of as a single
                    line. With this option, the '^' and '$' characters apply to each
                    line in source_string instead of the entire index value.
            - 'l': if index value exceeds the current maximum allowed size
                    (currently 16 MB), a NULL is returned instead of an error.
                    This is useful for long-running queries where you do not want
                    long strings causing an error that would make the query fail.
            - 'x': ignore whitespace.
    The 'match_arg' argument may contain more than one character.
    If a character in 'match_arg' is not valid, then that character is ignored.
    See Teradata® Database SQL Functions, Operators, Expressions, and Predicates,
    for more information on specifying arguments for REGEXP_SUBSTR.
    NOTES:
        - Using 'regex' or 'like' with axis equal to 0 will attempt to cast the
          values in the index to a VARCHAR.
          Note that conversion between BYTE data and other types is not supported.
          Also, LOBs are not allowed to be compared.
        - When using 'like' or 'regex', datatypes are casted into VARCHAR.
          This may alter the format of the value in the column(s)
          and thus whether there is a match or not. The size of the VARCHAR may also
          play a role since the casted value is truncated if the size is not big enough.
          See varchar_size under **kw: optional keyword arguments.
RETURNS:
    teradataml GeoDataFrame
RAISES:
    ValueError if more than one parameter: 'items', 'like', or 'regex' is used.
    TeradataMlException if invalid argument values are given.
EXAMPLES:
    >>> load_example_data("geodataframe","sample_streets")
    >>> df = GeoDataFrame('sample_streets')
    >>> df
          street_name              street_shape
    skey
    1      Coast Blvd  LINESTRING (12 12,18 17)
    1     Main Street  LINESTRING (2 2,3 2,4 1)
    >>>
    # Retrieve non-geometry columns in df
    >>> df.filter(items = ['skey', 'street_name'])
          street_name
    skey
    1      Coast Blvd
    1     Main Street
    >>>
    # Retrieve geometry column and 'skey' in df
    >>> df.filter(items = ['skey', 'street_shape'])
                      street_shape
    skey
    1     LINESTRING (12 12,18 17)
    1     LINESTRING (2 2,3 2,4 1)
    >>>
    # Retrieve rows where index matches 1
    >>> df.filter(items = ['1'], axis = 0)
          street_name              street_shape
    skey
    1      Coast Blvd  LINESTRING (12 12,18 17)
    1     Main Street  LINESTRING (2 2,3 2,4 1)
    >>>
from_query
teradataml.geospatial.geodataframe.GeoDataFrame.from_query = from_query(query, index=True, index_label=None, materialize=False) method of builtins.type instance
Class method for creating a GeoDataFrame using a query.
Note: 
    Use GeoDataFrame only when query result contains a Geometry data, i.e., 
    column of teradatasqlalchemy types: Geometry(ST_Geometry in SQL), MBB, MBR.
    Otherwise, an exception is raised.
PARAMETERS:
    query:
        Required Argument.
        Specifies the Teradata Vantage SQL query referenced by this GeoDataFrame.
        Only "SELECT" queries are supported. Exception will be
        raised, if any other type of query is passed.
        Unsupported queries include:
            1. DDL Queries like CREATE, DROP, ALTER etc.
            2. DML Queries like INSERT, UPDATE, DELETE etc. except SELECT.
            3. SELECT query with ORDER BY clause.
        Types: str
    index:
        Optional Argument.
        True if using index column for sorting otherwise False.
        Default Value: True
        Types: bool
    index_label:
        Optional Argument.
        Column/s used for sorting.
        Types: str
    materialize:
        Optional Argument.
        Whether to materialize GeoDataFrame or not when created.
        You should use materialization, when the query  passed to from_query(),
        is expected to produce non-deterministic results, when it is executed multiple
        times. Using this option will help user to have deterministic results in the
        resulting teradataml GeoDataFrame.
        Default Value: False (No materialization)
        Types: bool
RETURNS:
    GeoDataFrame
RAISES:
    TeradataMlException - TDMLDF_CREATE_FAIL
EXAMPLES:
    >>> from teradataml import GeoDataFrame
    >>> load_example_data("geodataframe","sample_streets")
    >>> gdf = GeoDataFrame.from_query("select skey, street_shape from sample_streets")
    >>> gdf
                      street_shape
    skey
    1     LINESTRING (12 12,18 17)
    1     LINESTRING (2 2,3 2,4 1)
    >>> gdf = GeoDataFrame.from_query("select skey, street_shape from sample_streets", True, "skey")
    >>> gdf
                      street_shape
    skey
    1     LINESTRING (12 12,18 17)
    1     LINESTRING (2 2,3 2,4 1)
    >>>
from_table
teradataml.geospatial.geodataframe.GeoDataFrame.from_table = from_table(table_name, index=True, index_label=None) method of builtins.type instance
Class method for creating a GeoDataFrame from a table or a view.
Note: 
    Use GeoDataFrame only when table or view contains a Geometry data, i.e., 
    column of teradatasqlalchemy types: Geometry(ST_Geometry in SQL), MBB, MBR.
    Otherwise, an exception is raised.
PARAMETERS:
    table_name:
        Required Argument.
        The table name in Teradata Vantage referenced by this GeoDataFrame.
        Types: str
    index:
        Optional Argument.
        True if using index column for sorting otherwise False.
        Default Value: True
        Types: bool
    index_label:
        Optional
        Column/s used for sorting.
        Types: str
RETURNS:
    GeoDataFrame
RAISES:
    TeradataMlException - TDMLDF_CREATE_FAIL
EXAMPLES:
    >>> from teradataml import GeoDataFrame
    >>> load_example_data("geodataframe","sample_streets")
    >>> gdf = GeoDataFrame.from_table('sample_streets')
    >>> gdf
          street_name              street_shape
    skey
    1      Coast Blvd  LINESTRING (12 12,18 17)
    1     Main Street  LINESTRING (2 2,3 2,4 1)
    >>> gdf = GeoDataFrame.from_table("sample_streets", False)
    >>> gdf
          street_name              street_shape
    skey
    1      Coast Blvd  LINESTRING (12 12,18 17)
    1     Main Street  LINESTRING (2 2,3 2,4 1)
    >>> gdf = GeoDataFrame.from_table("sample_streets", True, "skey")
    >>> gdf
          street_name              street_shape
    skey
    1      Coast Blvd  LINESTRING (12 12,18 17)
    1     Main Street  LINESTRING (2 2,3 2,4 1)
    >>>
get
teradataml.geospatial.geodataframe.GeoDataFrame.get = get(self, key)
DESCRIPTION:
    Retrieve required columns from GeoDataFrame using column name(s) as key.
    Returns a new teradataml GeoDataFrame with requested columns only.
PARAMETERS:
    key:
        Required Argument.
        Specifies column(s) to retrieve from the teradataml GeoDataFrame.
        Types: str OR List of Strings (str)
    teradataml supports the following formats (only) for the "get" method:
    1] Single Column String: df.get("col1")
    2] Single Column List: df.get(["col1"])
    3] Multi-Column List: df.get(['col1', 'col2', 'col3'])
    4] Multi-Column List of List: df.get([["col1", "col2", "col3"]])
    Note: Multi-Column retrieval of the same column such as df.get(['col1', 'col1']) is not supported.
RETURNS:
    teradataml GeoDataFrame
RAISES:
    TeradataMlException
EXAMPLES:
    >>> load_example_data("geodataframe","sample_streets")
    >>> df = GeoDataFrame('sample_streets')
    >>> df
          street_name              street_shape
    skey
    1      Coast Blvd  LINESTRING (12 12,18 17)
    1     Main Street  LINESTRING (2 2,3 2,4 1)
    >>>
    # Example 1: Column passed as single String Column
    >>> df.get("skey")
    Empty DataFrame
    Columns: []
    Index: [1, 1]
    >>>
    # Example 2: Column passed as single Column List
    >>> df.get(["street_shape"])
                   street_shape
    0  LINESTRING (12 12,18 17)
    1  LINESTRING (2 2,3 2,4 1)
    >>>
    # Example 3: Columns passed as multi-Column List
    >>> df.get(["skey", "street_shape"])
                      street_shape
    skey
    1     LINESTRING (12 12,18 17)
    1     LINESTRING (2 2,3 2,4 1)
    >>>
    # Example 4: Column passed as multi-Column List of List
    >>> df.get([["skey", "street_shape"]])
                      street_shape
    skey
    1     LINESTRING (12 12,18 17)
    1     LINESTRING (2 2,3 2,4 1)
    >>>
get_values
teradataml.geospatial.geodataframe.GeoDataFrame.get_values = get_values(self, num_rows=99999)
DESCRIPTION:
    Retrieves all values (only) present in a teradataml GeoDataFrame.
    Values are retrieved as per a numpy.ndarray representation of a teradataml GeoDataFrame.
    This format is equivalent to the get_values() representation of a Pandas GeoDataFrame.
PARAMETERS:
    num_rows:
        Optional Argument.
        Specifies the number of rows to retrieve values for from a teradataml GeoDataFrame.
        The num_rows parameter specified needs to be an integer value.
        Default Value: 99999
        Types: int
RETURNS:
    Numpy.ndarray representation of a teradataml GeoDataFrame
RAISES:
    TeradataMlException
EXAMPLES:
    >>> load_example_data("geodataframe","sample_streets")
    >>> df1 = GeoDataFrame.from_table('sample_streets')
    >>> df1
          street_name              street_shape
    skey
    1      Coast Blvd  LINESTRING (12 12,18 17)
    1     Main Street  LINESTRING (2 2,3 2,4 1)
    >>>
    # Retrieve all values from the teradataml GeoDataFrame
    >>> vals = df1.get_values()
    >>> vals
    array([['Main Street', 'LINESTRING (2 2,3 2,4 1)'],
           ['Coast Blvd', 'LINESTRING (12 12,18 17)']], dtype=object)
    >>>
    # Retrieve values for a given number of rows from the teradataml GeoDataFrame
    >>> vals = df1.get_values(num_rows = 1)
    >>> vals
    array([['Coast Blvd', 'LINESTRING (12 12,18 17)']], dtype=object)
    >>>
groupby
teradataml.geospatial.geodataframe.GeoDataFrame.groupby = groupby(self, columns_expr)
DESCRIPTION:
    Applies GroupBy to one or more columns of a teradataml GeoDataFrame.
    The result will always behaves like calling groupby with as_index=False
    in pandas.
PARAMETERS:
    columns_expr:
        Required Argument.
        Specifies the column name(s) to group by.
        Types: str OR list of Strings (str)
NOTES:
    1. Users can still apply teradataml GeoDataFrame methods (filters/sort/etc) on top of the result.
    2. Consecutive operations of grouping, i.e., groupby_time(), resample() and groupby() are not permitted.
       An exception will be raised. Following are some cases where exception will be raised as
       "Invalid operation applied, check documentation for correct usage."
            a. df.resample().groupby()
            b. df.resample().resample()
            c. df.resample().groupby_time()
RETURNS:
    teradataml GeoDataFrameGroupBy Object
RAISES:
    TeradataMlException
EXAMPLES:
    >>> load_example_data("geodataframe","sample_streets")
    >>> df = GeoDataFrame("sample_streets")
    >>> df1 = df.groupby(["street_shape"])
    >>> df1.count()
                   street_shape  count_skey  count_street_name
    0  LINESTRING (2 2,3 2,4 1)           1                  1
    1  LINESTRING (12 12,18 17)           1                  1
    >>>
head
teradataml.geospatial.geodataframe.GeoDataFrame.head = head(self, n=10)
DESCRIPTION:
    Print the first n rows of the sorted teradataml GeoDataFrame.
    Note:
        The GeoDataFrame is sorted on the index column or the first column if
        there is no index column. The column type must support sorting.
        Unsupported types: ['BLOB', 'CLOB', 'ARRAY', 'VARRAY']
PARAMETERS:
    n:
        Optional Argument.
        Specifies the number of rows to select.
        Default Value: 10.
        Types: int
RETURNS:
    teradataml GeoDataFrame
RAISES:
    TeradataMlException
EXAMPLES:
    >>> load_example_data("geodataframe", "sample_shapes")
    >>> df = GeoDataFrame("sample_shapes").select(["skey", "points"])
    >>> df
    skey                                                    points
    1003                                                  POINT (235.52 54.546)
    1010  MULTIPOINT (10.345 20.32 30.6,40.234 50.23 60.24,70.234 80.56 80.234)
    1008       MULTIPOINT (1.65 1.76,1.23 3.76,6.23 3.78,10.76 5.9,20.32 1.231)
    1006                                           POINT (235.52 54.546 7.4564)
    1007                                     MULTIPOINT (1 1,1 3,6 3,10 5,20 1)
    1005                                                          POINT (1 3 5)
    1009                                MULTIPOINT (10 20 30,40 50 60,70 80 80)
    1001                                                          POINT (10 20)
    1002                                                            POINT (1 3)
    1004                                                       POINT (10 20 30)
    >>> df.head()
    skey                                                    points
    1003                                                  POINT (235.52 54.546)
    1005                                                          POINT (1 3 5)
    1006                                           POINT (235.52 54.546 7.4564)
    1007                                     MULTIPOINT (1 1,1 3,6 3,10 5,20 1)
    1009                                MULTIPOINT (10 20 30,40 50 60,70 80 80)
    1010  MULTIPOINT (10.345 20.32 30.6,40.234 50.23 60.24,70.234 80.56 80.234)
    1008       MULTIPOINT (1.65 1.76,1.23 3.76,6.23 3.78,10.76 5.9,20.32 1.231)
    1004                                                       POINT (10 20 30)
    1002                                                            POINT (1 3)
    1001                                                          POINT (10 20)
    >>> df.head(5)
    skey        points
    1003  POINT (235.52 54.546)
    1005          POINT (1 3 5)
    1004       POINT (10 20 30)
    1002            POINT (1 3)
    1001          POINT (10 20)
info
teradataml.geospatial.geodataframe.GeoDataFrame.info = info(self, verbose=True, buf=None, max_cols=None, null_counts=False)
DESCRIPTION:
    Print a summary of the GeoDataFrame.
PARAMETERS:
    verbose:
        Optional Argument.
        Print full summary if True. Print short summary if False.
        Default Value: True
        Types: bool
    buf:
        Optional Argument.
        Speifies the writable buffer to send the output to.
        If argument is not specified, by default, the output is
        sent to sys.stdout.
    max_cols:
        Optional Argument.
        Specifies the maximum number of columns allowed for printing
        the full summary.
        Types: int
    null_counts:
        Optional Argument.
        Specifies whether to show the non-null counts.
        Display the counts if True, otherwise do not display the counts.
        Default Value: False
        Types: bool
RETURNS:
RAISES:
    TeradataMlException
EXAMPLES:
    >>> load_example_data("geodataframe","sample_streets")
    >>> df = GeoDataFrame('sample_streets')
    >>> df.info()
    <class 'teradataml.geospatial.geodataframe.GeoDataFrame'>
    Data columns (total 3 columns):
    skey            int
    street_name     str
    street_shape    str
    dtypes: str(2), int(1)
    >>>
    >>> df.info(null_counts=True)
    <class 'teradataml.geospatial.geodataframe.GeoDataFrame'>
    Data columns (total 3 columns):
    skey            2 non-null int
    street_name     2 non-null str
    street_shape    2 non-null str
    dtypes: str(2), int(1)
    >>>
    >>> df.info(verbose=False)
    <class 'teradataml.geospatial.geodataframe.GeoDataFrame'>
    Data columns (total 3 columns):
    dtypes: str(2), int(1)
    >>>
itertuples
teradataml.geospatial.geodataframe.GeoDataFrame.itertuples = itertuples(self, name='Row', num_rows=None)
DESCRIPTION:
    Method to Iterate over teradataml DataFrame rows as namedtuples.
PARAMETERS:
    name:
        Optional Argument.
        Specifies the name of the returned namedtuples. When set to None,
        the method returns the row in a list instead of namedtuple.
        Default Value: Row
        Types: str
    num_rows:
        Optional Argument.
        Specifies the number of rows to retrieve from the DataFrame.
        When set to None, the method retrieves all the records.
        Types: int OR NoneType
RETURNS:
    iterator, an object to iterate over namedtuples for each row in the DataFrame.
RAISES:
    None
EXAMPLES:
    # Load the data.
    >>> load_example_data("teradataml", "titanic")
    # Example 1: Retrieve all the records from the teradataml DataFrame.
    >>> df = DataFrame("titanic")
    >>> rows = df.itertuples()
    >>> next(rows)
    Row(passenger=78, survived=0, pclass=3, name='Moutal Mr. Rahamin Haim', sex='male', age=None, sibsp=0, parch=0, ticket='374
    >>> next(rows)
    Row(passenger=93, survived=0, pclass=1, name='Chaffee Mr. Herbert Fuller', sex='male', age=46, sibsp=1, parch=0, ticket='W.
    >>> next(rows)
    Row(passenger=5, survived=0, pclass=3, name='Allen Mr. William Henry', sex='male', age=35, sibsp=0, parch=0, ticket='373450
    >>>
    # Example 2: Retrieve the first 20 records from the teradataml DataFrame in a list,
    # when DataFrame is ordered by column "passenger".
    >>> df = DataFrame("titanic")
    >>> rows = df.sort("passenger").itertuples(name=None, num_rows=20)
    >>> next(rows)
    [1, 0, 3, 'Braund, Mr. Owen Harris', 'male', 22, 1, 0, 'A/5 21171', 7.25, None, 'S']
    >>> next(rows)
    [2, 1, 1, 'Cumings, Mrs. John Bradley (Florence Briggs Thayer)', 'female', 38, 1, 0, 'PC 17599', 71.2833, 'C85', 'C']
    >>> next(rows)
    [3, 1, 3, 'Heikkinen, Miss. Laina', 'female', 26, 0, 0, 'STON/O2. 3101282', 7.925, None, 'S']
join
teradataml.geospatial.geodataframe.GeoDataFrame.join = join(self, other, on=None, how='left', lsufﬁx=None, rsufﬁx=None)
DESCRIPTION:
    Joins two different teradataml GeoDataFrames together based on column comparisons
    specified in argument 'on' and type of join is specified in the argument 'how'.
    Supported join operations are:
    • Inner join: Returns only matching rows, non matching rows are eliminated.
    • Left outer join: Returns all matching rows plus non matching rows from the left table.
    • Right outer join: Returns all matching rows plus non matching rows from the right table.
    • Full outer join: Returns all rows from both tables, including non matching rows.
    • Cross join: Returns all rows from both tables where each row from the first table
                  is joined with each row from the second table. The result of the join
                  is a cartesian cross product.
                  Note: For a cross join, the 'on' argument is ignored.
    Supported join operators are =, ==, <, <=, >, >=, <> and != (= and <> operators are
    not supported when using GeoDataFrame columns as operands).
    Notes:
        1.  When multiple join conditions are given as a list string/ColumnExpression,
            they are joined using AND operator.
        2.  Two or more on conditions can be combined using & and | operators
            and can be passed as single ColumnExpression.
            You can use (df1.a == df1.b) & (df1.c == df1.d) in place of
            [df1.a == df1.b, df1.c == df1.d].
        3.  Two or more on conditions can not be combined using pythonic 'and'
            and 'or'.
            You can use (df1.a == df1.b) & (df1.c == df1.d) in place of
            [df1.a == df1.b and df1.c == df1.d].
        4.  Performing self join using same GeoDataFrame object in 'other'
            argument is not supported. In order to perform self join,
            first create aliased GeoDataFrame using alias() API and pass it
            for 'other' argument. Refer to Example 4 in EXAMPLES section.
PARAMETERS:
    other:
        Required Argument.
        Specifies the right teradataml DataFrame/GeoDataFrame on which join is to
        be performed.
        Types: teradataml GeoDataFrame or teradataml DataFrame
    on:
        Optional argument when "how" is "cross", otherwise required.
        If specified when "how" is "cross", it is ignored.
        Specifies list of conditions that indicate the columns to be join keys.
        It can take the following forms:
        • String comparisons, in the form of "col1 <= col2", where col1 is
          the column of left GeoDataFrame df1 and col2 is the column of right
          GeoDataFrame df2.
          Examples:
            1. ["a","b"] indicates df1.a = df2.a and df1.b = df2.b.
            2. ["a = b", "c == d"] indicates df1.a = df2.b and df1.c = df2.d.
            3. ["a <= b", "c > d"] indicates df1.a <= df2.b and df1.c > df2.d.
            4. ["a < b", "c >= d"] indicates df1.a < df2.b and df1.c >= df2.d.
            5. ["a <> b"] indicates df1.a != df2.b. Same is the case for ["a != b"].
        • Column comparisons, in the form of df1.col1 <= df2.col2, where col1
          is the column of left GeoDataFrame df1 and col2 is the column of right
          GeoDataFrame df2.
          Examples:
            1. [df1.a == df2.a, df1.b == df2.b] indicates df1.a = df2.a AND df1.b = df2.b.
            2. [df1.a == df2.b, df1.c == df2.d] indicates df1.a = df2.b AND df1.c = df2.d.
            3. [df1.a <= df2.b & df1.c > df2.d] indicates df1.a <= df2.b AND df1.c > df2.d.
            4. [df1.a < df2.b | df1.c >= df2.d] indicates df1.a < df2.b OR df1.c >= df2.d.
            5. df1.a != df2.b indicates df1.a != df2.b.
        • The combination of both string comparisons and comparisons as column expressions.
          Examples:
            1. ["a", df1.b == df2.b] indicates df1.a = df2.a AND df1.b = df2.b.
            2. [df1.a <= df2.b, "c > d"] indicates df1.a <= df2.b AND df1.c > df2.d.
        • ColumnExpressions containing FunctionExpressions which represent SQL functions
          invoked on GeoDataFrame Columns.
          Examples:
            1. gdf1.points_col.within(gdf2.lines_col) == 1
            2. (gdf2d.lines_col.is_closed == 1) & (gdf2d_alias.poly_col.perimeter > 0)
        Types: str (or) ColumnExpression (or) List of strings(str) or ColumnExpressions
    how:
        Optional Argument.
        Specifies the type of join to perform.
        Default value is "left".
        Permitted Values : "inner", "left", "right", "full" and "cross"
        Types: str
    lsuffix:
        Optional Argument.
        Specifies the suffix to be added to the left table columns.
        Default Value: None.
        Types: str
    rsuffix:
        Optional Argument.
        Specifies the suffix to be added to the right table columns.
        Default Value: None.
        Types: str
    lprefix:
        Optional Argument.
        Specifies the prefix to be added to the left table columns.
        Default Value: None.
        Types: str
    rprefix:
        Optional Argument.
        Specifies the prefix to be added to the right table columns.
        Default Value: None.
        Types: str
RETURNS:
    teradataml GeoDataFrame
RAISES:
    TeradataMlException
EXAMPLES:
    # Load the data to run the example.
    >>> from teradataml import load_example_data, GeoDataFrame
    >>> load_example_data("geodataframe", ["sample_cities", "sample_streets"])
    >>> df1 = GeoDataFrame("sample_cities")
    >>> df2 = GeoDataFrame("sample_streets")
    >>>
    # Print GeoDataFrame.
    >>> df1
           city_name                                 city_shape
    skey
    0     Oceanville            POLYGON ((1 1,1 3,6 3,6 0,1 1))
    1        Seaside  POLYGON ((10 10,10 20,20 20,20 15,10 10))
    # Print GeoDataFrame.
    >>> df2
          street_name              street_shape
    skey
    1      Coast Blvd  LINESTRING (12 12,18 17)
    1     Main Street  LINESTRING (2 2,3 2,4 1)
    # Example 1: Specifying on condition as string for common column.
    >>> df1.join(other = df2, on = ["skey"], how = "inner", lsuffix = "t1", rsuffix = "t2")
       t1_skey  t2_skey city_name                                 city_shape  street_name              street_shape
    0        1        1   Seaside  POLYGON ((10 10,10 20,20 20,20 15,10 10))   Coast Blvd  LINESTRING (12 12,18 17)
    1        1        1   Seaside  POLYGON ((10 10,10 20,20 20,20 15,10 10))  Main Street  LINESTRING (2 2,3 2,4 1)
    >>>
    # Example 2: Specifying on condition as ColumnExpression with left outer join.
    >>> df1.join(df2, on = [df1.city_name == df2.street_name], how = "left", lsuffix = "t1", rsuffix = "t2")
       t1_skey t2_skey   city_name                                 city_shape street_name street_shape
    0        0    None  Oceanville            POLYGON ((1 1,1 3,6 3,6 0,1 1))        None         None
    1        1    None     Seaside  POLYGON ((10 10,10 20,20 20,20 15,10 10))        None         None
    >>>
    # Example 3: Cross join "sample_cities" with "admissions_train", i.e.,
    #            joining teradataml GeoDataFrame with teradataml DataFrame.
    >>> load_example_data("dataframe", "admissions_train")
    >>> tdf = DataFrame("admissions_train")
    >>> df3 = df1.join(other=tdf, how="cross", lsuffix="l", rsuffix="r")
    >>> df3
           city_name                                 city_shape  id masters   gpa     stats programming  admitted
    skey
    1        Seaside  POLYGON ((10 10,10 20,20 20,20 15,10 10))   5      no  3.44    Novice      Novice         0
    1        Seaside  POLYGON ((10 10,10 20,20 20,20 15,10 10))   3      no  3.70    Novice    Beginner         1
    1        Seaside  POLYGON ((10 10,10 20,20 20,20 15,10 10))   1     yes  3.95  Beginner    Beginner         0
    0     Oceanville            POLYGON ((1 1,1 3,6 3,6 0,1 1))  38     yes  2.65  Advanced    Beginner         1
    0     Oceanville            POLYGON ((1 1,1 3,6 3,6 0,1 1))   5      no  3.44    Novice      Novice         0
    0     Oceanville            POLYGON ((1 1,1 3,6 3,6 0,1 1))  24      no  1.87  Advanced      Novice         1
    0     Oceanville            POLYGON ((1 1,1 3,6 3,6 0,1 1))   3      no  3.70    Novice    Beginner         1
    0     Oceanville            POLYGON ((1 1,1 3,6 3,6 0,1 1))   1     yes  3.95  Beginner    Beginner         0
    0     Oceanville            POLYGON ((1 1,1 3,6 3,6 0,1 1))  26     yes  3.57  Advanced    Advanced         1
    1        Seaside  POLYGON ((10 10,10 20,20 20,20 15,10 10))  24      no  1.87  Advanced      Novice         1
    >>>
    # Example 4: Perform join on GeoDataFrames with 'on' condition
    #            having FunctionExpression.
    >>> load_example_data("geodataframe", "sample_shapes")
    >>> gdf = GeoDataFrame("sample_shapes")
    >>> geo_df = gdf.select(["skey", "points", "linestrings"])
    >>> geo_df2 = geo_df.alias("rhs_gdf")
    >>> joined_geo = geo_df.join(geo_df2, geo_df.points.within(geo_df2.linestrings) == 1,
    >>>                          how="inner", lprefix="l")
        l_skey  skey                                   l_points                                points                          
    0     1002  1007                                POINT (1 3)    MULTIPOINT (1 1,1 3,6 3,10 5,20 1)                          
(10 5,20 1))
    1     1009  1004    MULTIPOINT (10 20 30,40 50 60,70 80 80)                      POINT (10 20 30)   MULTILINESTRING ((10 20
(70 80 80,90 100 110))            LINESTRING (10 20 30,40 50 60,70 80 80)
    2     1007  1007         MULTIPOINT (1 1,1 3,6 3,10 5,20 1)    MULTIPOINT (1 1,1 3,6 3,10 5,20 1)                   MULTILI
(10 5,20 1))        MULTILINESTRING ((1 1,1 3,6 3),(10 5,20 1))
    3     1005  1007                              POINT (1 3 5)    MULTIPOINT (1 1,1 3,6 3,10 5,20 1)                          
(10 5,20 1))
keys
teradataml.geospatial.geodataframe.GeoDataFrame.keys = keys(self)
DESCRIPTION:
    Get the column names of a GeoDataFrame.
RETURNS:
    List containing the column names
EXAMPLES:
    >>> load_example_data("geodataframe","sample_streets")
    >>> df = GeoDataFrame.from_table('sample_streets')
    >>> df.keys()
    ['skey', 'street_name', 'street_shape']
    >>>
merge
teradataml.geospatial.geodataframe.GeoDataFrame.merge = merge(self, right, on=None, how='inner', left_on=None, right_on=None, use_index=False, lsufﬁx=None,
rsufﬁx=None)
DESCRIPTION:
    Merges two teradataml GeoDataFrames together.
    Supported merge operations are:
        - inner: Returns only matching rows, non-matching rows are eliminated.
        - left: Returns all matching rows plus non-matching rows from the left teradataml GeoDataFrame.
        - right: Returns all matching rows plus non-matching rows from the right teradataml GeoDataFrame.
        - full: Returns all rows from both teradataml GeoDataFrames, including non matching rows.
PARAMETERS:
    right:
        Required Argument.
        Specifies right teradataml DataFrame/GeoDataFrame on which merge is to be performed.
        Types: teradataml GeoDataFrame or teradataml DataFrame
    on:
        Optional Argument.
        Specifies list of conditions that indicate the columns used for the merge.
        When no arguments are provided for this condition, the merge is performed using the indexes
        of the teradataml GeoDataFrames. Both teradataml GeoDataFrames are required to have index labels to
        perform a merge operation when no arguments are provided for this condition.
        When either teradataml GeoDataFrame does not have a valid index label in the above case,
        an exception is thrown.
        • String comparisons, in the form of "col1 <= col2", where col1 is
          the column of left GeoDataFrame df1 and col2 is the column of right
          GeoDataFrame df2.
          Examples:
            1. ["a","b"] indicates df1.a = df2.a and df1.b = df2.b.
            2. ["a = b", "c = d"] indicates df1.a = df2.b and df1.c = df2.d
            3. ["a <= b", "c > d"] indicates df1.a <= df2.b and df1.c > df2.d.
            4. ["a < b", "c >= d"] indicates df1.a < df2.b and df1.c >= df2.d.
            5. ["a <> b"] indicates df1.a != df2.b. Same is the case for ["a != b"].
        • Column comparisons, in the form of df1.col1 <= df2.col2, where col1
          is the column of left GeoDataFrame df1 and col2 is the column of right
          GeoDataFrame df2.
          Examples:
            1. [df1.a == df2.a, df1.b == df2.b] indicates df1.a = df2.a and df1.b = df2.b.
            2. [df1.a == df2.b, df1.c == df2.d] indicates df1.a = df2.b and df1.c = df2.d.
            3. [df1.a <= df2.b and df1.c > df2.d] indicates df1.a <= df2.b and df1.c > df2.d.
            4. [df1.a < df2.b and df1.c >= df2.d] indicates df1.a < df2.b and df1.c >= df2.d.
            5. df1.a != df2.b indicates df1.a != df2.b.
        • The combination of both string comparisons and comparisons as column expressions.
          Examples:
            1. ["a", df1.b == df2.b] indicates df1.a = df2.a and df1.b = df2.b.
            2. [df1.a <= df2.b, "c > d"] indicates df1.a <= df2.b and df1.c > df2.d.
        Default Value: None
        Types: str or ColumnExpression or List of strings(str) or ColumnExpressions
    how:
        Optional Argument.
        Specifies the type of merge to perform. Supports inner, left, right, full and cross merge operations.
        When how is "cross", the arguments on, left_on, right_on and use_index are ignored.
        Default Value: "inner".
        Types: str
    left_on:
        Optional Argument.
        Specifies column to merge on, in the left teradataml GeoDataFrame.
        When both the 'on' and 'left_on' parameters are unspecified, the index columns
        of the teradataml GeoDataFrames are used to perform the merge operation.
        Default Value: None.
        Types: str or ColumnExpression or List of strings(str) or ColumnExpressions
    right_on:
        Optional Argument.
        Specifies column to merge on, in the right teradataml GeoDataFrame.
        When both the 'on' and 'right_on' parameters are unspecified, the index columns
        of the teradataml GeoDataFrames are used to perform the merge operation.
        Default Value: None.
        Types: str or ColumnExpression or List of strings(str) or ColumnExpressions
    use_index:
        Optional Argument.
        Specifies whether (or not) to use index from the teradataml GeoDataFrames as the merge key(s).
        When False, and 'on', 'left_on', and 'right_on' are all unspecified, the index columns
        of the teradataml GeoDataFrames are used to perform the merge operation.
        Default value: False.
        Types: bool
    lsuffix:
        Optional Argument.
        Specifies suffix to be added to the left table columns.
        Default Value: None.
        Types: str
        Note: A suffix is required if teradataml GeoDataFrames being merged have columns
              with the same name.
    rsuffix:
        Optional Argument.
        Specifies suffix to be added to the right table columns.
        Default Value: None.
        Types: str
        Note: A suffix is required if teradataml GeoDataFrames being merged have columns
              with the same name.
RAISES:
    TeradataMlException
RETURNS:
    teradataml GeoDataFrame
EXAMPLES:
    # Load the data to run the example.
    >>> from teradataml import load_example_data, GeoDataFrame
    >>> load_example_data("geodataframe", ["sample_cities", "sample_streets"])
    >>>
    >>> df1 = GeoDataFrame("sample_cities")
    >>> df2 = GeoDataFrame("sample_streets")
    >>>
    # Print GeoDataFrame.
    >>> df1
           city_name                                 city_shape
    skey
    0     Oceanville            POLYGON ((1 1,1 3,6 3,6 0,1 1))
    1        Seaside  POLYGON ((10 10,10 20,20 20,20 15,10 10))
    # Print GeoDataFrame.
    >>> df2
          street_name              street_shape
    skey
    1      Coast Blvd  LINESTRING (12 12,18 17)
    1     Main Street  LINESTRING (2 2,3 2,4 1)
    >>>
    # Example 1: Specifying on condition as string for common column.
    >>> df1.merge(right = df2, on = ["skey"], how = "inner", lsuffix = "t1", rsuffix = "t2")
       t1_skey  t2_skey city_name                                 city_shape  street_name              street_shape
    0        1        1   Seaside  POLYGON ((10 10,10 20,20 20,20 15,10 10))   Coast Blvd  LINESTRING (12 12,18 17)
    1        1        1   Seaside  POLYGON ((10 10,10 20,20 20,20 15,10 10))  Main Street  LINESTRING (2 2,3 2,4 1)
    >>>
    # Example 2: Specifying on condition as ColumnExpression with left outer join.
    >>> df1.merge(df2, on = [df1.city_name == df2.street_name], how = "left", lsuffix = "t1", rsuffix = "t2")
       t1_skey t2_skey   city_name                                 city_shape street_name street_shape
    0        0    None  Oceanville            POLYGON ((1 1,1 3,6 3,6 0,1 1))        None         None
    1        1    None     Seaside  POLYGON ((10 10,10 20,20 20,20 15,10 10))        None         None
    >>>
    # Example 3: Cross join "sample_cities" with "admissions_train", i.e.,
    #            joining teradataml GeoDataFrame with teradataml DataFrame.
    >>> load_example_data("dataframe", "admissions_train")
    >>> tdf = DataFrame("admissions_train")
    >>> df3 = df1.merge(right=tdf, how="cross", lsuffix="l", rsuffix="r")
    >>> df3
           city_name                                 city_shape  id masters   gpa     stats programming  admitted
    skey
    0     Oceanville            POLYGON ((1 1,1 3,6 3,6 0,1 1))  13      no  4.00  Advanced      Novice         1
    0     Oceanville            POLYGON ((1 1,1 3,6 3,6 0,1 1))  11      no  3.13  Advanced    Advanced         1
    0     Oceanville            POLYGON ((1 1,1 3,6 3,6 0,1 1))   9      no  3.82  Advanced    Advanced         1
    1        Seaside  POLYGON ((10 10,10 20,20 20,20 15,10 10))  17      no  3.83  Advanced    Advanced         1
    1        Seaside  POLYGON ((10 10,10 20,20 20,20 15,10 10))  13      no  4.00  Advanced      Novice         1
    1        Seaside  POLYGON ((10 10,10 20,20 20,20 15,10 10))  32     yes  3.46  Advanced    Beginner         0
    1        Seaside  POLYGON ((10 10,10 20,20 20,20 15,10 10))  11      no  3.13  Advanced    Advanced         1
    1        Seaside  POLYGON ((10 10,10 20,20 20,20 15,10 10))   9      no  3.82  Advanced    Advanced         1
    1        Seaside  POLYGON ((10 10,10 20,20 20,20 15,10 10))  34     yes  3.85  Advanced    Beginner         0
    0     Oceanville            POLYGON ((1 1,1 3,6 3,6 0,1 1))  32     yes  3.46  Advanced    Beginner         0
    >>>
plot
teradataml.geospatial.geodataframe.GeoDataFrame.plot = plot(self, x=None, y=None, kind='geometry', **kwargs)
DESCRIPTION:
    Generate plots on teradataml GeoDataFrame. Following type of plots
    are supported, which can be specified using argument "kind":
        * geometry plot
        * bar plot
        * corr plot
        * line plot
        * mesh plot
        * scatter plot
        * wiggle plot
    Notes:
        * Geometry plot is generated based on geometry column in teradataml GeoDataFrame.
        * Only the columns with ST_GEOMETRY type are allowed for generating geometry plot.
        * The maximum size for ST_GEOMETRY must be less than or equal to 64000.
        * The ST_GEOMETRY shape can be POINT, LINESTRING etc. It is POLGYON that allows
          filling of different colors.
PARAMETERS:
    x:
        Optional Argument.
        Specifies a GeoDataFrame column to use for the x-axis data.
        Note:
            "x" is not significant for geometry plots. For other plots
            it is mandatory argument.
        Types: teradataml GeoDataFrame Column
    y:
        Required Argument.
        Specifies GeoDataFrame column(s) to use for the y-axis data.
        Notes:
             * Geometry plot always requires geometry column and corresponding 'weight'
               column. 'weight' column represents the weight of a shape mentioned in
               geometry column.
             * If user does not specify geometry column, the default geometry column
               is considered for plotting.
        Types: teradataml GeoDataFrame Column OR tuple of GeoDataFrame Column OR list of teradataml GeoDataFrame Columns.
    scale:
        Optional Argument.
        Specifies GeoDataFrame column to use for scale data to
        wiggle and mesh plots.
        Note:
            "scale" is significant for wiggle and mesh plots. Ignored for other
            type of plots.
        Types: teradataml GeoDataFrame Column.
    kind:
        Optional Argument.
        Specifies the kind of plot.
        Permitted Values:
            * 'geometry'
            * 'line'
            * 'bar'
            * 'scatter'
            * 'corr'
            * 'wiggle'
            * 'mesh'
        Default Value: geometry
        Types: str
    ax:
        Optional Argument.
        Specifies the axis for the plot.
        Types: Axis
    cmap:
        Optional Argument.
        Specifies the name of the colormap to be used for plotting.
        Notes:
             * Significant only when corresponding type of plot is mesh or geometry.
             * Ignored for other type of plots.
        Permitted Values:
            * All the colormaps mentioned in below URLs are supported.
                * https://matplotlib.org/stable/tutorials/colors/colormaps.html
                * https://matplotlib.org/cmocean/
        Types: str
    color:
        Optional Argument.
        Specifies the color for the plot.
        Note:
            Hexadecimal color codes are not supported.
        Permitted Values:
            * 'blue'
            * 'orange'
            * 'green'
            * 'red'
            * 'purple'
            * 'brown'
            * 'pink'
            * 'gray'
            * 'olive'
            * 'cyan'
            * Apart from above mentioned colors, the colors mentioned in
              https://xkcd.com/color/rgb are also supported.
        Default Value: 'blue'
        Types: str OR list of str
    figure:
        Optional Argument.
        Specifies the figure for the plot.
        Types: Figure
    figsize:
        Optional Argument.
        Specifies the size of the figure in a tuple of 2 elements. First
        element represents width of plot image in pixels and second
        element represents height of plot image in pixels.
        Default Value: (640, 480)
        Types: tuple
    figtype:
        Optional Argument.
        Specifies the type of the image to generate.
        Permitted Values:
            * 'png'
            * 'jpg'
            * 'svg'
        Default Value: png
        Types: str
    figdpi:
        Optional Argument.
        Specifies the dots per inch for the plot image.
        Note:
            * Valid range for "dpi" is: 72 <= width <= 300.
        Default Value: 100 for PNG and JPG Type image.
        Types: int
    grid_color:
        Optional Argument.
        Specifies the color of the grid.
        Note:
            Hexadecimal color codes are not supported.
        Permitted Values:
            * 'blue'
            * 'orange'
            * 'green'
            * 'red'
            * 'purple'
            * 'brown'
            * 'pink'
            * 'gray'
            * 'olive'
            * 'cyan'
            * Apart from above mentioned colors, the colors mentioned in
              https://xkcd.com/color/rgb are also supported.
        Default Value: gray
        Types: str
    grid_format:
        Optional Argument.
        Specifies the format for the grid.
        Types: str
    grid_linestyle:
        Optional Argument.
        Specifies the line style of the grid.
        Permitted Values:
            * -
            * --
            * -.
        Default Value: -
        Types: str
    grid_linewidth:
        Optional Argument.
        Specifies the line width of the grid.
        Note:
            Valid range for "grid_linewidth" is: 0.5 <= grid_linewidth <= 10.
        Default Value: 0.8
        Types: int OR float
    heading:
        Optional Argument.
        Specifies the heading for the plot.
        Types: str
    legend:
        Optional Argument.
        Specifies the legend(s) for the Plot.
        Types: str OR list of str
    legend_style:
        Optional Argument.
        Specifies the location for legend to display on Plot image. By default,
        legend is displayed at upper right corner.
        Permitted Values:
            * 'upper right'
            * 'upper left'
            * 'lower right'
            * 'lower left'
            * 'right'
            * 'center left'
            * 'center right'
            * 'lower center'
            * 'upper center'
            * 'center'
        Default Value: 'upper right'
        Types: str
    linestyle:
        Optional Argument.
        Specifies the line style for the plot.
        Permitted Values:
            * -
            * --
            * -.
            * :
        Default Value: -
        Types: str OR list of str
    linewidth:
        Optional Argument.
        Specifies the line width for the plot.
        Note:
            Valid range for "linewidth" is: 0.5 <= linewidth <= 10.
        Default Value: 0.8
        Types: int OR float OR list of int OR list of float
    marker:
        Optional Argument.
        Specifies the type of the marker to be used.
        Permitted Values:
            All the markers mentioned in https://matplotlib.org/stable/api/markers_api.html
            are supported.
        Types: str OR list of str
    markersize:
        Optional Argument.
        Specifies the size of the marker.
        Note:
            Valid range for "markersize" is: 1 <= markersize <= 20.
        Default Value: 6
        Types: int OR float OR list of int OR list of float
    position:
        Optional Argument.
        Specifies the position of the axis in the figure. Accepts a tuple
        of two elements where first element represents the row and second
        element represents column.
        Default Value: (1, 1)
        Types: tuple
    span:
        Optional Argument.
        Specifies the span of the axis in the figure. Accepts a tuple
        of two elements where first element represents the row and second
        element represents column.
        For Example,
            Span of (2, 1) specifies the Axis occupies 2 rows and 1 column
            in Figure.
        Default Value: (1, 1)
        Types: tuple
    reverse_xaxis:
        Optional Argument.
        Specifies whether to reverse tick values on x-axis or not.
        Default Value: False
        Types: bool
    reverse_yaxis:
        Optional Argument.
        Specifies whether to reverse tick values on y-axis or not.
        Default Value: False
        Types: bool
    series_identifier:
        Optional Argument.
        Specifies the teradataml GeoDataFrame Column which represents the
        identifier for the data. As many plots as distinct "series_identifier"
        are generated in a single Axis.
        For example:
            consider the below data in teradataml GeoDataFrame.
                   ID   x   y
                0  1    1   1
                1  1    2   2
                2  2   10  10
                3  2   20  20
            If "series_identifier" is not specified, simple plot is
            generated where every 'y' is plotted against 'x' in a
            single plot. However, specifying "series_identifier" as 'ID'
            generates two plots in a single axis. One plot is for ID 1
            and another plot is for ID 2.
        Types: teradataml GeoDataFrame Column.
    title:
        Optional Argument.
        Specifies the title for the Axis.
        Types: str
    xlabel:
        Optional Argument.
        Specifies the label for x-axis.
        Notes:
             * When set to empty string, label is not displayed for x-axis.
             * When set to None, name of the x-axis column is displayed as
               label.
        Types: str
    xlim:
        Optional Argument.
        Specifies the range for xtick values.
        Types: tuple
    xtick_format:
        Optional Argument.
        Specifies whether to format tick values for x-axis or not.
        Types: str
    ylabel:
        Optional Argument.
        Specifies the label for y-axis.
        Notes:
             * When set to empty string, label is not displayed for y-axis.
             * When set to None, name of the y-axis column(s) is displayed as
               label.
        Types: str
    ylim:
        Optional Argument.
        Specifies the range for ytick values.
        Types: tuple
    ytick_format:
        Optional Argument.
        Specifies whether to format tick values for y-axis or not.
        Types: str
    vmin:
        Optional Argument.
        Specifies the lower range of the color map. By default, the range
        is derived from data and color codes are assigned accordingly.
        Note:
            "vmin" Significant only for Geometry Plot.
        Types: int OR float
    vmax:
        Optional Argument.
        Specifies the upper range of the color map. By default, the range is
        derived from data and color codes are assigned accordingly.
        Note:
            "vmax" Significant only for Geometry Plot.
        For example:
            Assuming user wants to use colormap 'matter' and derive the colors for
            values which are in between 1 and 100.
            Note:
                colormap 'matter' starts with Pale Yellow and ends with Violet.
            * If "colormap_range" is not specified, then range is derived from
              existing values. Thus, colors are represented as below in the whole range:
              * 1 as Pale Yellow.
              * 100 as Violet.
            * If "colormap_range" is specified as -100 and 100, the value 1 is at middle of
              the specified range. Thus, colors are represented as below in the whole range:
              * -100 as Pale Yellow.
              * 1 as Orange.
              * 100 as Violet.
        Types: int OR float
    wiggle_fill:
        Optional Argument.
        Specifies whether to fill the wiggle area or not. By default, the right
        positive half of the wiggle is not filled. If specified as True, wiggle
        area is filled.
        Note:
            Applicable only for the wiggle plot.
        Default Value: False
        Types: bool
    wiggle_scale:
        Optional Argument.
        Specifies the scale of the wiggle. By default, the amplitude of wiggle is scaled
        relative to RMS of the first payload.  In certain cases, it can lead to excessively
        large wiggles. Use "wiggle_scale" to adjust the relative size of the wiggle.
        Note:
            Applicable only for the wiggle and mesh plots.
        Types: int OR float
    ignore_nulls:
        Optional Argument.
        Specifies whether to delete rows with null values or not present in 'x', 'y' and
        'scale' params.
        Default Value: False
        Types: bool
RAISES:
    TeradataMlException
EXAMPLES:
    >>> load_example_data("geodataframe", ["sample_shapes"])
    >>> shapes_df = GeoDataFrame("sample_shapes")
    >>> shapes_df
                                      points                     linestrings                        polygons                geo
        skey
        1006    POINT (235.52 54.546 7.4564)  LINESTRING (1.35 3.6456 4.5,3.  POLYGON ((0 0 0,0 0 20,0 20 0,                   
        1007  MULTIPOINT (1 1,1 3,6 3,10 5,2  MULTILINESTRING ((1 1,1 3,6 3)  MULTIPOLYGON (((1 1,1 3,6 3,6                    
        1005                   POINT (1 3 5)  LINESTRING (1 3 6,3 0 6,6 0 1)  POLYGON ((0 0 0,0 0 20.435,0.0  GEOMETRYCOLLECTIO
        1004                POINT (10 20 30)  LINESTRING (10 20 30,40 50 60,  POLYGON ((0 0 0,0 10 20,20 20   GEOMETRYCOLLECTIO
        1003           POINT (235.52 54.546)  LINESTRING (1.35 3.6456,3.6756  POLYGON ((0.6 0.8,0.6 20.8,20.                   
        1001                   POINT (10 20)    LINESTRING (1 1,2 2,3 3,4 4)  POLYGON ((0 0,0 20,20 20,20 0,  GEOMETRYCOLLECTIO
        1002                     POINT (1 3)        LINESTRING (1 3,3 0,0 1)  POLYGON ((0 0,0 20,20 20,20 0,                   
        1009  MULTIPOINT (10 20 30,40 50 60,  MULTILINESTRING ((10 20 30,40   MULTIPOLYGON (((0 0 0,0 20 20,                   
        1008  MULTIPOINT (1.65 1.76,1.23 3.7  MULTILINESTRING ((1 3,3 0,0 1)  MULTIPOLYGON (((0 0,0 20,20 20                   
        1010  MULTIPOINT (10.345 20.32 30.6,  MULTILINESTRING ((1 3 6,3 0 6,  MULTIPOLYGON (((0 0 0,0 0 20,0                   
    >>>
    >>> load_example_data("geodataframe", ["us_population", "us_states_shapes"])
    >>> us_population
               location_type  population_year  population
    state_name
    Georgia            State             1930   2908506.0
    Georgia            State             1950   3444578.0
    Georgia            State             1960   3943116.0
    Georgia            State             1970   4589575.0
    Georgia            State             1990   6478216.0
    Georgia            State             2000   8186453.0
    Georgia            State             1980   5463105.0
    Georgia            State             1940   3123723.0
    Georgia            State             1920   2895832.0
    Georgia            State             1910   2609121.0
    >>> us_states_shapes = GeoDataFrame("us_states_shapes")
    >>> us_states_shapes
           state_name                     state_shape
    id
    NM     New Mexico  POLYGON ((472.45213 324.75551,
    VA       Virginia  POLYGON ((908.75086 270.98255,
    ND   North Dakota  POLYGON ((556.50879 73.847349,
    OK       Oklahoma  POLYGON ((609.50526 322.91131,
    WI      Wisconsin  POLYGON ((705.79187 134.80299,
    RI   Rhode Island  POLYGON ((946.50841 152.08022,
    HI         Hawaii  POLYGON ((416.34965 514.99923,
    KY       Kentucky  POLYGON ((693.17367 317.18459,
    WV  West Virginia  POLYGON ((836.73002 223.71281,
    NJ     New Jersey  POLYGON ((916.80709 207.30914,
    >>>
    >>> # Join shapes with population and filter only 1990 data.
    >>> population_data = us_states_shapes.join(us_population,
    ...                                         on=us_population.state_name == us_states_shapes.state_name,
    ...                                         lsuffix="us",
    ...                                         rsuffix="t2")
    >>> population_data = population_data.select(["us_state_name", "state_shape", "population_year", "population"])
    >>> type(population_data)
    teradataml.geospatial.geodataframe.GeoDataFrame
    >>>
    # Example 1: Generate the geometry plot to show the density of population
    #            across the US states in year 1990.
    >>> population_data_1990 = population_data[population_data.population_year == 1990]
    >>> population_data_1990
       us_state_name                     state_shape  population_year  population
    0     New Mexico  POLYGON ((472.45213 324.75551,             1990   1515069.0
    1         Hawaii  POLYGON ((416.34965 514.99923,             1990   1108229.0
    2       Kentucky  POLYGON ((693.17367 317.18459,             1990   3685296.0
    3     New Jersey  POLYGON ((916.80709 207.30914,             1990   7730188.0
    4   North Dakota  POLYGON ((556.50879 73.847349,             1990    638800.0
    5       Oklahoma  POLYGON ((609.50526 322.91131,             1990   3145585.0
    6  West Virginia  POLYGON ((836.73002 223.71281,             1990   1793477.0
    7      Wisconsin  POLYGON ((705.79187 134.80299,             1990   4891769.0
    8       Virginia  POLYGON ((908.75086 270.98255,             1990   6187358.0
    9   Rhode Island  POLYGON ((946.50841 152.08022,             1990   1003464.0
    >>>
    >>> # Define Figure.
    >>> from teradataml import Figure
    >>> figure = Figure(width=1500, height=862, heading="Geometry Plot")
    >>> figure.heading = "Geometry Plot"
    >>>
    >>> plot_1990 = population_data_1990.plot(y=population_data_1990.population,
    ...                                       cmap='rainbow',
    ...                                       figure=figure,
    ...                                       reverse_yaxis=True,
    ...                                       title="US 1990 Population",
    ...                                       xlabel="",
    ...                                       ylabel="")
    >>>
    >>> plot_1990.show()
    # Example 2: Plot a geometry plot for a single polygon to visualize the shape.
    # Note: X-axis is not significant in geometry plot. Y-axis can be a tuple,
    #       first element represents weight of geometry shape and second element
    #       represents the geometry column. Since color of geometry shape is generated
    #       based on first column and since the example is to plot a single polygon,
    #       the first element in tuple is not significant.
    >>> # Generate GeoDataFrame which has single Polygon.
    >>> single_polygon_df = shapes_df[shapes_df.skey==1004]
    >>> single_polygon_df.plot(y=(single_polygon_df.skey, single_polygon_df.polygons))
    # Example 3: Generate a bar plot on a GeoDataFrame.
    #     Note: The below example shows how the population of the United States
    #           changed from 1910 to 2020.
    >>> population_data.plot(x=population_data.population_year, y=population_data.population, kind="bar")
    # Example 4: Generate a subplot on a GeoDataFrame to show the rate of population increase over 4 decades.
    # Create DataFrames for population in the year 2020, 2010, 2000, 1990.
    >>> df_2020 = population_data[population_data.population_year == 2020]
    >>> df_2010 = population_data[population_data.population_year == 2010]
    >>> df_2000 = population_data[population_data.population_year == 2000]
    >>> df_1990 = population_data[population_data.population_year == 1990]
    # Define subplot.
    >>> fig, axes = subplots(nrows=2, ncols=2)
    >>> plot_population = df_1990.plot(y=(df_1990.population, df_1990.state_shape),
    ...                                   cmap='rainbow',
    ...                                   figure=fig,
    ...                                   ax=axis[0],
    ...                                   reverse_yaxis=True,
    ...                                   vmin=55036.0,
    ...                                   vmax=39538223.0,
    ...                                   heading="US Population growth over 4 decades",
    ...                                   title="US 1990 Population",
    ...                                   xlabel="",
    ...                                   yylabel="")
    >>> plot_population = df_2000.plot(y=(df_2000.population, df_2000.state_shape),
    ...                                   cmap='rainbow',
    ...                                   figure=fig,
    ...                                   ax=axis[1],
    ...                                   reverse_yaxis=True,
    ...                                   vmin=55036.0,
    ...                                   vmax=39538223.0,
    ...                                   heading="US Population growth over 4 decades",
    ...                                   title="US 2000 Population",
    ...                                   xlabel="",
    ...                                   ylabel="")
    >>> plot_population = df_2010.plot(x=df_2010.population_year,
    ...                                y=(df_2010.population, df_2010.state_shape),
    ...                                cmap='rainbow',
    ...                                figure=fig,
    ...                                ax=axis[2],
    ...                                reverse_yaxis=True,
    ...                                vmin=55036.0,
    ...                                vmax=39538223.0,
    ...                                heading="US Population growth over 4 decades",
    ...                                title="US 2010 Population",
    ...                                xlabel="",
    ...                                ylabel="",
    ...                                xtick_values_format="")
    >>> plot_population = df_2020.plot(x=df_2020.population_year,
    ...                                y=(df_2020.population, df_2020.state_shape),
    ...                                cmap='rainbow',
    ...                                figure=fig,
    ...                                ax=axis[3],
    ...                                reverse_yaxis=True,
    ...                                vmin=55036.0,
    ...                                vmax=39538223.0,
    ...                                heading="US Population growth over 4 decades",
    ...                                title="US 2020 Population",
    ...                                xlabel="",
    ...                                ylabel="",
    ...                                xtick_values_format="")
    >>> # Show the plot.
    >>> plot_population.show()
sample
teradataml.geospatial.geodataframe.GeoDataFrame.sample = sample(self, n=None, frac=None, replace=False, randomize=False, case_when_then=None,
case_else=None)
DESCRIPTION:
    Allows to sample few rows from GeoDataFrame directly or based on conditions.
    Creates a new column 'sampleid' which has a unique id for each sample
    sampled, it helps to uniquely identify each sample.
PARAMETERS:
    n:
        Required Argument, if neither of 'frac' and 'case_when_then' are specified.
        Specifies a set of positive integer constants that specifies the number of 
        rows to be sampled from the teradataml GeoDataFrame.
        Example:
            n = 10 or n = [10] or n = [10, 20, 30, 40]
        Default Value: None
        Types: int or list of ints.
        Note:
            1. You should use only one of the following arguments: 'n', 'frac' and 'case_when_then'.
            2. No more than 16 samples can be requested per count description.
    frac:
        Required Argument, if neither of 'n' and 'case_when_then' are specified.
        Specifies any set of unsigned floating point constant numbers in the half
        opened interval (0,1] that means greater than 0 and less than or equal to 1.
        It specifies the percentage of rows to be sampled from the teradataml GeoDataFrame.
        Example:
            frac = 0.4 or frac = [0.4] or frac = [0.2, 0.5]
        Default Value: None
        Types: float or list of floats.
        Note:
            1. You should use only one of the following arguments: 'n', 'frac' and 'case_when_then'.
            2. No more than 16 samples can be requested per count description.
            3. Sum of elements in list should not be greater than 1 as total percentage cannot be
               more than 100% and should not be less than or equal to 0.
    replace:
        Optional Argument.
        Specifies if sampling should be done with replacement or not.
        Default Value: False
        Types: bool
    randomize:
        Optional Argument.
        Specifies if sampling should be done across AMPs in Teradata or per AMP.
        Default Value: False
        Types: bool
    case_when_then :
        Required Argument, if neither of 'frac' and 'n' are specified.
        Specifies condition and number of samples to be sampled as key value pairs.
        Keys should be of type ColumnExpressions.
        Values should be either of type int, float, list of ints or list of floats.
        The following usage of key is not allowed:
            case_when_then = {"gpa" > 2 : 2}
        The following operators are supported:
              comparison: ==, !=, <, <=, >, >=
              boolean: & (and), | (or), ~ (not), ^ (xor)
        Example :
              case_when_then = {df.gpa > 2 : 2}
              case_when_then = {df.gpa > 2 & df.stats == 'Novice' : [0.2, 0.3],
                               df.programming == 'Advanced' : [10,20,30]}
        Default Value: None
        Types: dictionary
        Note:
            1. You should use only one of the following arguments: 'n', 'frac' and 'case_when_then'.
            2. No more than 16 samples can be requested per fraction description or count description.
            3. If any value in dictionary is specified as list of floats then
               sum of elements in list should not be greater than 1 as total percentage cannot be
               more than 100% and should not be less than or equal to 0.
    case_else :
        Optional Argument.
        Specifies number of samples to be sampled from rows where none of the conditions in
        'case_when_then' are met.
        Example :
            case_else = 10
            case_else = [10,20]
            case_else = [0.5]
            case_else = [0.2,0.4]
        Default Value: None
        Types: int or float or list of ints or list of floats
        Note:
            1. This argument can only be used with 'case_when_then'. 
               If used otherwise, below error will raised.
                   'case_else' can only be used when 'case_when_then' is specified.
            2. No more than 16 samples can be requested per fraction description 
               or count description.
            3. If case_else is list of floats then sum of elements in list should not be 
               greater than 1 as total percentage cannot be more than 100% and should not 
               be less than or equal to 0.
RETURNS:
    teradataml GeoDataFrame
RAISES:
    1. ValueError - When columns of different GeoDataFrames are given in ColumnExpression.
                     or
                    When columns are given in string format and not ColumnExpression.
    2. TeradataMlException - If types of input parameters are mismatched.
    3. TypeError
Examples:
    >>> from teradataml import load_example_data, GeoDataFrame
    >>> load_example_data("geodataframe","sample_shapes")
    >>> df = GeoDataFrame("sample_shapes").select(["skey", "points"])
    >>>
    # Print GeoDataFrame.
    >>> df
                                                                         points
    skey
    1006                                           POINT (235.52 54.546 7.4564)
    1001                                                          POINT (10 20)
    1002                                                            POINT (1 3)
    1010  MULTIPOINT (10.345 20.32 30.6,40.234 50.23 60.24,70.234 80.56 80.234)
    1004                                                       POINT (10 20 30)
    1003                                                  POINT (235.52 54.546)
    1008       MULTIPOINT (1.65 1.76,1.23 3.76,6.23 3.78,10.76 5.9,20.32 1.231)
    1005                                                          POINT (1 3 5)
    1007                                     MULTIPOINT (1 1,1 3,6 3,10 5,20 1)
    1009                                MULTIPOINT (10 20 30,40 50 60,70 80 80)
    >>>
    # Example 1: Sample with only n argument.
    #            Randomly samples 2 rows from the teradataml GeoDataFrame.
    #            As there is only 1 sample 'sampleid' is 1.
    >>> df.sample(n = 2)
                                                                    points  sampleid
    skey
    1008  MULTIPOINT (1.65 1.76,1.23 3.76,6.23 3.78,10.76 5.9,20.32 1.231)         1
    1001                                                     POINT (10 20)         1
    >>>
    # Example 2: Sample with multiple sample values for n.
    #            Creates 2 samples with 2 and 1 rows each respectively.
    #            There are 2 values(1,2) for 'sampleid' each for one sample.
    >>> df.sample(n = [2, 1])
                                                                    points  sampleid
    skey
    1003                                             POINT (235.52 54.546)         1
    1008  MULTIPOINT (1.65 1.76,1.23 3.76,6.23 3.78,10.76 5.9,20.32 1.231)         2
    1001                                                     POINT (10 20)         1
    >>>
    # Example 3: Sample with only frac parameter.
    #            Randomly samples 20% of total rows present in teradataml GeoDataFrame.
    >>> df.sample(frac = 0.2)
                    points  sampleid
    skey
    1004  POINT (10 20 30)         1
    1001     POINT (10 20)         1
    >>>
    # Example 4: Sample with multiple sample values for frac.
    #            Creates 2 samples each with 40% and 20% of total rows in teradataml GeoDataFrame.
    >>> df.sample(frac = [0.4, 0.2])
                                                                    points  sampleid
    skey
    1001                                                     POINT (10 20)         1
    1004                                                  POINT (10 20 30)         1
    1003                                             POINT (235.52 54.546)         2
    1008  MULTIPOINT (1.65 1.76,1.23 3.76,6.23 3.78,10.76 5.9,20.32 1.231)         1
    1006                                      POINT (235.52 54.546 7.4564)         2
    1009                           MULTIPOINT (10 20 30,40 50 60,70 80 80)         1
    >>>
    # Example 5: Sample with n and replace and randomization.
    #            Creates 2 samples with 2 and 1 rows respectively with possible redundant
    #            sampling as replace is True and also selects rows from different AMPS as
    #            randomize is True.
    >>> df.sample(n = [2, 1], replace = True, randomize = True)
                                           points  sampleid
    skey
    1005                            POINT (1 3 5)         2
    1009  MULTIPOINT (10 20 30,40 50 60,70 80 80)         1
    1009  MULTIPOINT (10 20 30,40 50 60,70 80 80)         1
    >>>
    # Example 6: Sample with frac and replace and randomization.
    #            Creates 2 samples with 40% and 20% of total rows in teradataml GeoDataFrame
    #            respectively with possible redundant sampling and also selects rows from different AMPS.
    >>> df.sample(frac = [0.4, 0.2], replace = True, randomize = True)
                                      points  sampleid
    skey
    1002                         POINT (1 3)         1
    1004                    POINT (10 20 30)         1
    1004                    POINT (10 20 30)         2
    1002                         POINT (1 3)         1
    1005                       POINT (1 3 5)         2
    1007  MULTIPOINT (1 1,1 3,6 3,10 5,20 1)         1
    >>>
    # Example 7: Sample with case_when_then.
    #            Creates 2 samples with 1, 2 rows respectively from rows which satisfy df.skey < 1004
    #            and 25% of rows from rows which satisfy df.skey >= 1004.
    >>> df.sample(case_when_then={df.skey < 1004 : [1, 2], df.skey >= 1004 : 0.25})
                                                                         points  sampleid
    skey
    1010  MULTIPOINT (10.345 20.32 30.6,40.234 50.23 60.24,70.234 80.56 80.234)         3
    1003                                                  POINT (235.52 54.546)         1
    1002                                                            POINT (1 3)         1
    1001                                                          POINT (10 20)         1
    1009                                MULTIPOINT (10 20 30,40 50 60,70 80 80)         3
    >>>
    # Example 8: Sample with case_when_then and replace, randomize.
    #            Creates 2 samples with 1, 2 rows respectively from rows which satisfy df.skey < 1004
    #            and 25% of rows from rows which satisfy df.skey >= 1004 and selects rows
    #            from different AMPs with replacement.
    >>> df.sample(replace = True, randomize = True, case_when_then={df.skey < 1004 : [1, 2],
    ...                                                             df.skey >= 1004 : 0.25})
                                      points  sampleid
    skey
    1001                       POINT (10 20)         1
    1001                       POINT (10 20)         2
    1001                       POINT (10 20)         2
    1001                       POINT (10 20)         2
    1002                         POINT (1 3)         1
    1002                         POINT (1 3)         2
    1002                         POINT (1 3)         2
    1001                       POINT (10 20)         2
    1001                       POINT (10 20)         1
    1007  MULTIPOINT (1 1,1 3,6 3,10 5,20 1)         3
    >>>
    # Example 9: Sample with case_when_then and case_else
    #            Creates 4 samples 2 with 1, 3 rows from rows which satisfy df.skey < 1004.
    #            2 samples with 25%, 50% of rows from all the rows which does not
    #            meet condition df.skey < 1004.
    >>> df.sample(case_when_then = {df.skey < 1004 : [1, 3]}, case_else = [0.25, 0.5])
                                                                         points  sampleid
    skey
    1005                                                          POINT (1 3 5)         3
    1010  MULTIPOINT (10.345 20.32 30.6,40.234 50.23 60.24,70.234 80.56 80.234)         4
    1008       MULTIPOINT (1.65 1.76,1.23 3.76,6.23 3.78,10.76 5.9,20.32 1.231)         3
    1004                                                       POINT (10 20 30)         4
    1003                                                  POINT (235.52 54.546)         1
    1002                                                            POINT (1 3)         1
    1001                                                          POINT (10 20)         1
    1006                                           POINT (235.52 54.546 7.4564)         4
    1007                                     MULTIPOINT (1 1,1 3,6 3,10 5,20 1)         4
    >>>
    # Example 10: Sample with case_when_then, case_else, replace, randomize
    #             Creates 4 samples 2 with 1, 3 rows from rows which satisfy df.skey < 1004 and
    #             2 samples with 25%, 50% of rows from all the rows which does not
    #             meet condition df.skey < 1004  with possible redundant replacement
    #             and also selects rows from different AMPs
    >>> df.sample(case_when_then = {df.skey < 1004 : [1, 3]}, replace = True,
    ...           randomize = True, case_else = [0.25, 0.5])
                                                                         points  sampleid
    skey
    1009                                MULTIPOINT (10 20 30,40 50 60,70 80 80)         4
    1010  MULTIPOINT (10.345 20.32 30.6,40.234 50.23 60.24,70.234 80.56 80.234)         4
    1008       MULTIPOINT (1.65 1.76,1.23 3.76,6.23 3.78,10.76 5.9,20.32 1.231)         4
    1002                                                            POINT (1 3)         1
    1002                                                            POINT (1 3)         1
    1002                                                            POINT (1 3)         2
    1002                                                            POINT (1 3)         2
    1002                                                            POINT (1 3)         2
    1002                                                            POINT (1 3)         2
    1002                                                            POINT (1 3)         1
    >>>
select
teradataml.geospatial.geodataframe.GeoDataFrame.select = select(self, select_expression)
DESCRIPTION:
    Select required columns from GeoDataFrame using an expression.
    Returns a new teradataml GeoDataFrame with selected columns only.
PARAMETERS:
    select_expression:
        Required Argument.
        String or List representing columns to select.
        Types: str OR List of Strings (str)
        The following formats (only) are supported for select_expression:
        A] Single Column String: df.select("col1")
        B] Single Column List: df.select(["col1"])
        C] Multi-Column List: df.select(['col1', 'col2', 'col3'])
        D] Multi-Column List of List: df.select([["col1", "col2", "col3"]])
        Column Names ("col1", "col2"..) are Strings representing Teradata Vantage table Columns.
        All Standard Teradata data types for columns supported: INTEGER, VARCHAR(5), FLOAT.
        Note: Multi-Column selection of the same column such as df.select(['col1', 'col1']) is not supported.
RETURNS:
    teradataml GeoDataFrame
RAISES:
    TeradataMlException (TDMLDF_SELECT_INVALID_COLUMN, TDMLDF_SELECT_INVALID_FORMAT,
                         TDMLDF_SELECT_DF_FAIL, TDMLDF_SELECT_EXPR_UNSPECIFIED,
                         TDMLDF_SELECT_NONE_OR_EMPTY)
EXAMPLES:
    >>> load_example_data("geodataframe","sample_streets")
    >>> df = GeoDataFrame('sample_streets')
    >>> df
          street_name              street_shape
    skey
    1      Coast Blvd  LINESTRING (12 12,18 17)
    1     Main Street  LINESTRING (2 2,3 2,4 1)
    >>>
    # Example 1: Column passed as single String Column
    >>> df.select("skey")
    Empty DataFrame
    Columns: []
    Index: [1, 1]
    >>>
    # Example 2: Column passed as single Column List
    >>> df.select(["street_shape"])
                   street_shape
    0  LINESTRING (12 12,18 17)
    1  LINESTRING (2 2,3 2,4 1)
    >>>
    # Example 3: Columns passed as multi-Column List
    >>> df.select(["skey", "street_shape"])
                      street_shape
    skey
    1     LINESTRING (12 12,18 17)
    1     LINESTRING (2 2,3 2,4 1)
    >>>
    # Example 4: Column passed as multi-Column List of List
    >>> df.select([["skey", "street_shape"]])
                      street_shape
    skey
    1     LINESTRING (12 12,18 17)
    1     LINESTRING (2 2,3 2,4 1)
    >>>
set_index
teradataml.geospatial.geodataframe.GeoDataFrame.set_index = set_index(self, keys, drop=True, append=False)
    DESCRIPTION:
        Assigns one or more existing columns as the new index to a teradataml GeoDataFrame.
    PARAMETERS:
        keys:
            Required Argument.
            Specifies the column name or a list of column names to use as the GeoDataFrame index.
            Types: str OR list of Strings (str)
        drop:
            Optional Argument.
            Specifies whether or not to display the column(s) being set as index as
            teradataml GeoDataFrame columns anymore.
            When drop is True, columns are set as index and not displayed as columns.
            When drop is False, columns are set as index; but also displayed as columns.
            Note: When the drop argument is set to True, the column being set as index does not cease to
                  be a part of the underlying table upon which the teradataml GeoDataFrame is based off.
                  A column that is dropped while being set as an index is merely not used for display
                  purposes anymore as a column of the teradataml GeoDataFrame.
            Default Value: True
            Types: bool
        append:
            Optional Argument.
            Specifies whether or not to append requested columns to the existing index.
`           When append is False, replaces existing index.
            When append is True, retains both existing & currently appended index.
            Default Value: False
            Types: bool
    RETURNS:
        teradataml GeoDataFrame
    RAISES:
        TeradataMlException
    EXAMPLES:
        >>> load_example_data("geodataframe","sample_cities")
        >>> df = GeoDataFrame("sample_cities")
        >>> df.sort('skey')
               city_name                                 city_shape
        skey
        0     Oceanville            POLYGON ((1 1,1 3,6 3,6 0,1 1))
        1        Seaside  POLYGON ((10 10,10 20,20 20,20 15,10 10))
        >>>
        # Set new index.
        >>> df.set_index('city_shape').sort('skey')
                                                   skey   city_name
        city_shape
        POLYGON ((1 1,1 3,6 3,6 0,1 1))               0  Oceanville
        POLYGON ((10 10,10 20,20 20,20 15,10 10))     1     Seaside
        >>>
        # Set multiple indexes using list of columns
        >>> df.set_index(['city_name', 'skey']).sort('skey')
                                                        city_shape
        city_name  skey
        Oceanville 0               POLYGON ((1 1,1 3,6 3,6 0,1 1))
        Seaside    1     POLYGON ((10 10,10 20,20 20,20 15,10 10))
        >>>
        # Append to new index to the existing set of index.
        >>> df.set_index(['city_name', 'skey'], drop = False, append = True).sort('skey')
                                                        city_shape
        city_name  skey
        Oceanville 0               POLYGON ((1 1,1 3,6 3,6 0,1 1))
        Seaside    1     POLYGON ((10 10,10 20,20 20,20 15,10 10))
        >>>
show_query
teradataml.geospatial.geodataframe.GeoDataFrame.show_query = show_query(self, full_query=False)
DESCRIPTION:
    Function returns underlying SQL for the teradataml GeoDataFrame. It is the same 
    SQL that is used to view the data for a teradataml GeoDataFrame.
PARAMETERS:
    full_query:
        Optional Argument.
        Specifies if the complete query for the GeoDataFrame should be returned.
        When this parameter is set to True, query for the GeoDataFrame is returned
        with respect to the base GeoDataFrame's table (from_table() or from_query()) or from the
        output tables of analytical functions (if there are any in the workflow).
        This query may or may not be directly used to retrieve data for the GeoDataFrame upon
        which the function is called.
        When this parameter is not used, string returned is the query already used
        or will be used to retrieve data for the teradataml GeoDataFrame.
        Default Value: False
        Types: bool
RETURNS:
    String representing the underlying SQL query for the teradataml GeoDataFrame.
EXAMPLES:
    >>> load_example_data("geodataframe","sample_streets")
    >>> df = GeoDataFrame.from_table("sample_streets")
    >>>
    # Example 1: Show query on base (from_table) GeoDataFrame, with default option
    >>> df.show_query()
    'select * from "sample_streets"'
    >>>
    # Example 2: Show query on base (from_query) GeoDataFrame, with default option
    >>> df_from_query = GeoDataFrame.from_query("select skey, street_shape from sample_streets")
    >>> df_from_query.show_query()
    'select skey, street_shape from sample_streets'
    >>>
    # Example 3: Show query on base (from_table) GeoDataFrame, with full_query option
    #            This will return same query as with default option because workflow
    #            only has one GeoDataFrame.
    >>> df.show_query(full_query = True)
    'select * from "sample_streets"'
    >>>
    # Example 4: Show query on base (from_query) GeoDataFrame, with full_query option
    #            This will return same query as with default option because workflow
    #            only has one GeoDataFrame.
    >>> df_from_query = GeoDataFrame.from_query("select skey, street_shape from sample_streets")
    >>> df_from_query.show_query(full_query = True)
    'select skey, street_shape from sample_streets'
    >>>
    # Example 5: Show query used in a workflow demonstrating default and full_query options.
    >>>
    # Workflow Step-1: Assign operation on base GeoDataFrame
    >>> df1 = df.assign(temp_column=df.skey + df.skey)
    >>>
    # Workflow Step-2: Selecting columns from assign's result
    >>> df2 = df1.select(["skey", "street_shape"])
    >>>
    # Workflow Step-3: Filtering on top of select's result
    >>> df3 = df2[df2.street_shape.is_valid == 1]
    >>>
    # Show query with full_query option on df3.
    # This will give full query upto base GeoDataFrame(df)
    >>> df3.show_query(full_query = True)
    'select * from (select skey,street_shape from (select skey AS skey, street_name AS street_name, street_shape AS street_shap
    >>>
    # Show query with default option on df3. This will give same query as give in above case.
    >>> df3.show_query()
    'select * from (select skey,street_shape from (select skey AS skey, street_name AS street_name, street_shape AS street_shap
    >>>
    # Executing intermediate GeoDataFrame df2
    >>> df2
                      street_shape
    skey
    1     LINESTRING (12 12,18 17)
    1     LINESTRING (2 2,3 2,4 1)
    >>>
    # Show query with default option on df3. This will give query with respect
    # to view/table created by the latest executed GeoDataFrame in the workflow (df2 in this scenario).
    # This is the query teradataml internally uses to retrieve data for GeoDataFrame df4, if executed
    # at this point.
    >>> df3.show_query()
    'select * from "ALICE"."ml__select__1646760722889302" where street_shape.ST_IsValid() = 1'
    >>>
    # Show query with full_query option on df3. This will still give the same full
    # query upto base GeoDataFrame(df)
    >>> df3.show_query(full_query = True)
    'select * from (select skey,street_shape from (select skey AS skey, street_name AS street_name, street_shape AS street_shap
    >>>
sort
teradataml.geospatial.geodataframe.GeoDataFrame.sort = sort(self, columns, ascending=True)
DESCRIPTION:
    Get sorted data by one or more columns in either ascending or descending order
    for a GeoDataFrame.
    Unsupported column types for sorting: ['BLOB', 'CLOB', 'ARRAY', 'VARRAY']
PARAMETERS:
    columns:
        Required Argument.
        Column names as a string or a list of strings to sort on.
        Types: str OR list of Strings (str)
    ascending:
        Optional Argument.
        Order ASC or DESC to be applied for each column.
        True for ascending order and False for descending order.
        Default value: True
        Types: bool
RETURNS:
    teradataml GeoDataFrame
RAISES:
    TeradataMlException
EXAMPLES:
    >>> load_example_data("geodataframe","sample_streets")
    >>> df = GeoDataFrame('sample_streets')
    >>> df
          street_name              street_shape
    skey
    1      Coast Blvd  LINESTRING (12 12,18 17)
    1     Main Street  LINESTRING (2 2,3 2,4 1)
    >>> df.sort("street_shape")
          street_name              street_shape
    skey
    1     Main Street  LINESTRING (2 2,3 2,4 1)
    1      Coast Blvd  LINESTRING (12 12,18 17)
    >>>
sort_index
teradataml.geospatial.geodataframe.GeoDataFrame.sort_index = sort_index(self, axis=0, ascending=True, kind='quicksort')
DESCRIPTION:
    Gets sorted object by labels (along an axis) in either ascending or
    descending order for a teradataml GeoDataFrame.
PARAMETERS:
    axis:
        Optional Argument.
        Specifies the value to direct sorting on index or columns. 
        Values can be either 0 ('rows') OR 1 ('columns'), value as 0
        will sort on index (if no index is present then parent
        GeoDataFrame will be returned) and value as 1 will sort on
        columns names (if no index is present then parent GeoDataFrame
        will be returned with sorted columns) for the GeoDataFrame.
        Default value: 0
        Types: int
    ascending:
        Optional Argument.
        Specifies a flag to sort columns in either ascending (True)
        or descending (False).
        Default value: True
        Types: bool
    kind:
        Optional Argument.
        Specifies a desired algorithm to be used.
        Permitted values: 'quicksort', 'mergesort' or 'heapsort'
        Default value: 'quicksort'
        Types: str
RETURNS:
    teradataml GeoDataFrame
RAISES:
    TeradataMlException
EXAMPLES:
    >>> load_example_data("GeoDataFrame","sample_cities")
    >>> df = GeoDataFrame.from_table('sample_cities')
    >>> df
           city_name                                 city_shape
    skey
    1        Seaside  POLYGON ((10 10,10 20,20 20,20 15,10 10))
    0     Oceanville            POLYGON ((1 1,1 3,6 3,6 0,1 1))
    >>>
    >>> df.sort_index()
           city_name                                 city_shape
    skey
    0     Oceanville            POLYGON ((1 1,1 3,6 3,6 0,1 1))
    1        Seaside  POLYGON ((10 10,10 20,20 20,20 15,10 10))
    >>>
    >>> df.sort_index(0)
           city_name                                 city_shape
    skey
    0     Oceanville            POLYGON ((1 1,1 3,6 3,6 0,1 1))
    1        Seaside  POLYGON ((10 10,10 20,20 20,20 15,10 10))
    >>>
    >>> df.sort_index(1, False) # here 'False' means DESCENDING for respective axis
                                         city_shape   city_name
    skey
    1     POLYGON ((10 10,10 20,20 20,20 15,10 10))     Seaside
    0               POLYGON ((1 1,1 3,6 3,6 0,1 1))  Oceanville
    >>>
    >>> df.sort_index(1, True, 'mergesort')
           city_name                                 city_shape
    skey
    1        Seaside  POLYGON ((10 10,10 20,20 20,20 15,10 10))
    0     Oceanville            POLYGON ((1 1,1 3,6 3,6 0,1 1))
    >>>
squeeze
teradataml.geospatial.geodataframe.GeoDataFrame.squeeze = squeeze(self, axis=None)
DESCRIPTION:
    Squeeze one-dimensional axis objects into scalars.
    teradataml GeoDataFrames with a single element are squeezed to a scalar.
    teradataml GeoDataFrames with a single column are squeezed to a Series.
    Otherwise the object is unchanged.
    Note: Currently only '1' and 'None' are supported for axis.
          For now with axis = 0, the teradataml GeoDataFrame is returned.
PARAMETERS:
    axis:
        Optional Argument.
        A specific axis to squeeze. By default, all axes with
        length equals one are squeezed.
        Permitted Values: 0 or 'index', 1 or 'columns', None
        Default: None
RETURNS:
    teradataml GeoDataFrame, teradataml Series, or scalar,
    the projection after squeezing 'axis' or all the axes.
RAISES:
    TeradataMlException
EXAMPLES:
    >>> load_example_data("geodataframe","sample_streets")
    >>> gdf = GeoDataFrame("sample_streets")
    >>> gdf
          street_name              street_shape
    skey
    1      Coast Blvd  LINESTRING (12 12,18 17)
    1     Main Street  LINESTRING (2 2,3 2,4 1)
    >>>
    # Example 1: Squeeze the GeoDataFrame.
    >>> shape = gdf.select(["street_shape"])
    >>> shape.squeeze()
    0    LINESTRING (12 12,18 17)
    1    LINESTRING (2 2,3 2,4 1)
    Name: street_shape, dtype: object
    >>>
    >>> shape.squeeze(axis = 1)
    0    LINESTRING (12 12,18 17)
    1    LINESTRING (2 2,3 2,4 1)
    Name: street_shape, dtype: object
    >>>
    >>> shape.squeeze(axis = 0)
                   street_shape
    0  LINESTRING (12 12,18 17)
    1  LINESTRING (2 2,3 2,4 1)
    >>>
    # Example 2: Squeeze the GeoDataFrame created from a query.
    >>> df = GeoDataFrame.from_query('select skey, street_shape from sample_streets')
    >>> s = df.squeeze()
    >>> s
                      street_shape
    skey
    1     LINESTRING (12 12,18 17)
    1     LINESTRING (2 2,3 2,4 1)
    >>>
    # Example 3: Squeeze the GeoDataFrame containing only one value.
    >>> gdf_single = gdf[gdf.street_name == "Coast Blvd"].select("street_shape")
    >>> gdf_single
                   street_shape
    0  LINESTRING (12 12,18 17)
    >>> gdf_single.squeeze()
    'LINESTRING (12 12,18 17)'
    >>> gdf_single.squeeze(axis = 1)
    0    LINESTRING (12 12,18 17)
    Name: street_shape, dtype: object
    >>> gdf_single.squeeze(axis = 0)
                   street_shape
    0  LINESTRING (12 12,18 17)
    >>>
tail
teradataml.geospatial.geodataframe.GeoDataFrame.tail = tail(self, n=10)
DESCRIPTION:
    Print the last n rows of the sorted teradataml GeoDataFrame.
    Note:
        The GeoDataframe is sorted on the index column or the first column if
        there is no index column. The column type must support sorting.
        Unsupported types: ['BLOB', 'CLOB', 'ARRAY', 'VARRAY']
PARAMETERS:
    n:
        Optional Argument.
        Specifies the number of rows to select.
        Default Value: 10.
        Types: int
RETURNS:
    teradataml GeoDataFrame
RAISES:
    TeradataMlException
EXAMPLES:
>>> load_example_data("geodataframe", "sample_shapes")
>>> df = GeoDataFrame("sample_shapes").select(["skey", "points"])
>>> df
skey                                                    points
1003                                                  POINT (235.52 54.546)
1010  MULTIPOINT (10.345 20.32 30.6,40.234 50.23 60.24,70.234 80.56 80.234)
1008       MULTIPOINT (1.65 1.76,1.23 3.76,6.23 3.78,10.76 5.9,20.32 1.231)
1006                                           POINT (235.52 54.546 7.4564)
1007                                     MULTIPOINT (1 1,1 3,6 3,10 5,20 1)
1005                                                          POINT (1 3 5)
1009                                MULTIPOINT (10 20 30,40 50 60,70 80 80)
1001                                                          POINT (10 20)
1002                                                            POINT (1 3)
1004                                                       POINT (10 20 30)
>>> df.tail()
skey                                  points
1008       MULTIPOINT (1.65 1.76,1.23 3.76,6.23 3.78,10.76 5.9,20.32 1.231)
1006                                           POINT (235.52 54.546 7.4564)
1005                                                          POINT (1 3 5)
1004                                                       POINT (10 20 30)
1002                                                            POINT (1 3)
1001                                                          POINT (10 20)
1003                                                  POINT (235.52 54.546)
1007                                     MULTIPOINT (1 1,1 3,6 3,10 5,20 1)
1009                                MULTIPOINT (10 20 30,40 50 60,70 80 80)
1010  MULTIPOINT (10.345 20.32 30.6,40.234 50.23 60.24,70.234 80.56 80.234)
>>> df.tail(5)
skey                                points
1008       MULTIPOINT (1.65 1.76,1.23 3.76,6.23 3.78,10.76 5.9,20.32 1.231)
1006                                           POINT (235.52 54.546 7.4564)
1007                                     MULTIPOINT (1 1,1 3,6 3,10 5,20 1)
1009                                MULTIPOINT (10 20 30,40 50 60,70 80 80)
1010  MULTIPOINT (10.345 20.32 30.6,40.234 50.23 60.24,70.234 80.56 80.234)
>>>
to_csv
teradataml.geospatial.geodataframe.GeoDataFrame.to_csv = to_csv(self, csv_ﬁle, num_rows=99999, all_rows=False, fastexport=False, sep=',', quotechar='"',
catch_errors_warnings=False, **kwargs)
DESCRIPTION:
    Export data to CSV from teradataml DataFrame with or
    without FastExport protocol.
PARAMETERS:
    csv_file:
        Required Argument.
        Specifies the name of CSV file to export the data into.
        Types: str
    num_rows:
        Optional Argument.
        Specifies the number of rows to export.
        Note:
            This argument is ignored if "all_rows" is set to True.
        Default Value: 99999
        Types: int
    all_rows:
        Optional Argument.
        Specifies whether all rows should be exported to CSV or not.
        Default Value: False
        Types: bool
    fastexport:
        Optional Argument.
        Specifies whether FastExport protocol should be used or not while
        exporting data. When set to True, data is exported using FastExport
        protocol, otherwise FastExport protocol is not used, which is default.
        When set to None, the approach is decided based on the number of rows
        to be exported.
        Notes:
            1. Teradata recommends to use FastExport when number of rows
               in teradataml DataFrame are atleast 100,000. To extract
               lesser rows ignore this option and go with regular
               approach. FastExport opens multiple data transfer connections
               to the database.
            2. FastExport does not support all Teradata Database data types.
               For example, tables with BLOB and CLOB type columns cannot
               be extracted.
            3. FastExport cannot be used to extract data from a
               volatile or temporary table.
            4. For best efficiency, do not use DataFrame.groupby() and
               DataFrame.sort() with FastExport.
        For additional information about FastExport protocol through
        teradatasql driver, please refer to FASTEXPORT section of
        https://pypi.org/project/teradatasql/#FastExport driver documentation.
        Default Value: False
        Types: bool
    sep:
        Optional Argument.
        Specifies a single character string used to separate fields in a CSV file.
        Default Value: ","
        Notes:
            1. "sep" cannot be line feed ('\n') or carriage return ('\r').
            2. "sep" should not be same as "quotechar".
            3. Length of "sep" argument should be 1.
        Types: String
    quotechar:
        Optional Argument.
        Specifies a single character string used to quote fields in a CSV file.
        Default Value: """
        Notes:
            1. "quotechar" cannot be line feed ('\n') or carriage return ('\r').
            2. "quotechar" should not be same as "sep".
            3. Length of "quotechar" argument should be 1.
        Types: String
    catch_errors_warnings:
        Optional Argument.
        Specifies whether to catch errors/warnings (if any) raised by
        FastExport protocol while exporting data.
        Note:
            This argument is ignored if "fastexport" is set to False.
        Default Value: False
        Types: bool
    kwargs:
        Optional Argument.
        Specifies keyword arguments. Argument "open_sessions" can be
        passed as keyword arguments.
            * "open_sessions" specifies the number of Teradata data transfer
              sessions to be opened for fastexport. This argument is only
              applicable in fastexport mode.
        Note:
            If "open_sessions" argument is not provided, the default value
               is the smaller of 8 or the number of AMPs avaialble.
               For additional information about number of Teradata data-transfer
               sessions opened during fastexport, please refer to:
               https://pypi.org/project/teradatasql/#FastExport
RETURNS:
    When FastExport protocol is used and "catch_errors_warnings" is set to True,
    then the function returns a tuple containing:
        a. Errors, if any, thrown by fastexport in a list of strings.
        b. Warnings, if any, thrown by fastexport in a list of strings.
RAISES:
    TeradataMlException
EXAMPLES:
    # Create a teradataml DataFrame.
    >>> load_example_data("dataframe","admissions_train")
    >>> df = DataFrame("admissions_train")
    >>> df
       masters   gpa     stats programming admitted
    id
    22     yes  3.46    Novice    Beginner        0
    37      no  3.52    Novice      Novice        1
    35      no  3.68    Novice    Beginner        1
    12      no  3.65    Novice      Novice        1
    4      yes  3.50  Beginner      Novice        1
    38     yes  2.65  Advanced    Beginner        1
    27     yes  3.96  Advanced    Advanced        0
    39     yes  3.75  Advanced    Beginner        0
    7      yes  2.33    Novice      Novice        1
    40     yes  3.95    Novice    Beginner        0
    ...
    # Example 1: Export data from teradataml DataFrame into CSV,
    #            with only required argument.
    >>> df.to_csv("export_to_csv_1.csv")
       Data is successfully exported into export_to_csv_1.csv
    # Example 2: Export all rows from teradataml DataFrame into CSV
    #            using FastExport protocol.
    >>> df.to_csv("export_to_csv_2.csv", all_rows=True, fastexport=True)
       Data is successfully exported into export_to_csv_2.csv
    # Example 3: Export 20 rows from teradataml DataFrame into CSV.
    >>> df.to_csv("export_to_csv_3.csv", num_rows=20)
       Data is successfully exported into export_to_csv_3.csv
    # Example 4: Export data from teradataml DataFrame into CSV using
    #            FastExport protocol by opening one Teradata data
    #            transfer session. Save errors and warnings
    #            thrown by fastexport.
    >>> err, warn = df.to_csv("export_to_csv_4.csv", fastexport=True,
                              catch_errors_warnings=True, open_sessions=1 )
       Data is successfully exported into export_to_csv_4.csv
    >>>err
       []
    >>>warn
       []
    # Example 5: Export data from teradataml DataFrame into CSV
    #            file with '|' as field separator and single quote(')
    #            as field quote character.
    >>> df.to_csv("export_to_csv_5.csv", sep="|", quotechar="'" )
       Data is successfully exported into export_to_csv_5.csv
to_pandas
teradataml.geospatial.geodataframe.GeoDataFrame.to_pandas = to_pandas(self, index_column=None, num_rows=99999, all_rows=False, fastexport=False,
catch_errors_warnings=False, **kwargs)
DESCRIPTION:
    Returns a Pandas GeoDataFrame for the corresponding teradataml
    GeoDataFrame Object.
PARAMETERS:
    index_column:
        Optional Argument.
        Specifies column(s) to be used as Pandas index.
        When the argument is provided, the specified column is used as
        the Pandas index. Otherwise, the teradataml GeoDataFrame's index
        (if exists) is used as the Pandas index or the primary index of
        the table on Vantage is used as the Pandas index. The default
        integer index is used if none of the above indexes exists.
        Default Value: Integer index
        Types: str OR list of Strings (str)
    num_rows:
        Optional Argument.
        The number of rows to retrieve from GeoDataFrame while creating
        Pandas GeoDataFrame.
        Default Value: 99999
        Types: int
        Note:
            This argument is ignored if "all_rows" is set to True.
    all_rows:
        Optional Argument.
        Specifies whether all rows from teradataml GeoDataFrame should be
        retrieved while creating Pandas GeoDataFrame.
        Default Value: False
        Types: bool
    fastexport:
        Optional Argument.
        Specifies whether fastexport protocol should be used while
        converting teradataml GeoDataFrame to a Pandas GeoDataFrame. If the
        argument is set to True, fastexport wire protocol is used
        internally for data transfer. By default, fastexport protocol will not be
        used while converting teradataml GeoDataFrame to a Pandas GeoDataFrame.
        When set to None, the approach is decided based on the number of rows
        requested by the user for extraction.
        If requested number of rows are greater than or equal to 100000,
        then fastexport is used, otherwise regular mode is used for data
        extraction.
        Note:
            1. Teradata recommends to use FastExport when number of rows
               in teradataml GeoDataFrame are atleast 100,000. To extract
               lesser rows ignore this option and go with regular
               approach. FastExport opens multiple data transfer connections
               to the database.
            2. FastExport does not support all Teradata Database data types.
               For example, tables with BLOB and CLOB type columns cannot
               be extracted.
            3. FastExport cannot be used to extract data from a
               volatile or temporary table.
            4. For best efficiency, do not use GeoDataFrame.groupby() and
               GeoDataFrame.sort() with FastExport.
        For additional information about FastExport protocol through
        teradatasql driver, please refer to FASTEXPORT section of
        https://pypi.org/project/teradatasql/#FastExport driver documentation.
        Default Value: False
        Types: bool
    catch_errors_warnings:
        Optional Argument.
        Specifies whether to catch errors/warnings(if any) raised by
        fastexport protocol while converting teradataml GeoDataFrame to
        Pandas GeoDataFrame. When this is set to True and fastexport is used,
        to_pandas() returns a tuple containing:
            a. Pandas GeoDataFrame.
            b. Errors(if any) in a list thrown by fastexport.
            c. Warnings(if any) in a list thrown by fastexport.
        When set to False and fastexport is used, prints the fastexport
        errors/warnings to the standard output, if there are any.
        Note:
            This argument is ignored if "fastexport" is set to False.
        Default Value: False
        Types: bool
    kwargs:
        Optional Argument.
        Specifies keyword arguments. Arguments "coerce_float" and
        "parse_dates" can be passed as keyword arguments.
            * "coerce_float" specifies a boolean to for attempting to
              convert non-string, non-numeric objects to floating point.
            * "parse_dates" specifies columns to parse as dates.
        Note:
            For additional information about "coerce_float" and
            "parse_date" arguments please refer to:
            https://pandas.pydata.org/docs/reference/api/pandas.read_sql.html
RETURNS:
    When "catch_errors_warnings" is set to True and if protocol used for
    data transfer is fastexport, then the function returns a tuple
    containing:
        a. Pandas GeoDataFrame.
        b. Errors, if any, thrown by fastexport in a list of strings.
        c. Warnings, if any, thrown by fastexport in a list of strings.
    Only Pandas GeoDataFrame otherwise.
    Note:
        Column types of the resulting Pandas GeoDataFrame depends on
        pandas.read_sql_query().
RAISES:
    TeradataMlException
EXAMPLES:
    >>> load_example_data("geodataframe","sample_streets")
    >>> df = GeoDataFrame("sample_streets")
    >>> df
          street_name              street_shape
    skey
    1      Coast Blvd  LINESTRING (12 12,18 17)
    1     Main Street  LINESTRING (2 2,3 2,4 1)
    >>>
    >>> pandas_df = df.to_pandas()
    >>> pandas_df
          street_name              street_shape
    skey
    1     Main Street  LINESTRING (2 2,3 2,4 1)
    1      Coast Blvd  LINESTRING (12 12,18 17)
    >>>
    >>> pandas_df = df.to_pandas(index_column = 'skey')
    >>> pandas_df
          street_name              street_shape
    skey
    1     Main Street  LINESTRING (2 2,3 2,4 1)
    1      Coast Blvd  LINESTRING (12 12,18 17)
    >>>
    >>> pandas_df = df.to_pandas(index_column = 'street_shape')
    >>> pandas_df
                              skey  street_name
    street_shape
    LINESTRING (2 2,3 2,4 1)     1  Main Street
    LINESTRING (12 12,18 17)     1   Coast Blvd
    >>>
    >>> pandas_df = df.to_pandas(index_column = ['skey', 'street_name'])
    >>> pandas_df
                                  street_shape
    skey street_name
    1    Main Street  LINESTRING (2 2,3 2,4 1)
         Coast Blvd   LINESTRING (12 12,18 17)
    >>>
    >>> pandas_df = df.to_pandas(index_column = 'skey', num_rows = 1)
    >>> pandas_df
         street_name              street_shape
    skey
    1     Coast Blvd  LINESTRING (12 12,18 17)
    >>>
    >>> pandas_df = df.to_pandas(all_rows = True)
    >>> pandas_df
          street_name              street_shape
    skey
    1     Main Street  LINESTRING (2 2,3 2,4 1)
    1      Coast Blvd  LINESTRING (12 12,18 17)
    >>>
to_sql
teradataml.geospatial.geodataframe.GeoDataFrame.to_sql = to_sql(self, table_name, if_exists='fail', primary_index=None, temporary=False, schema_name=None,
types=None, primary_time_index_name=None, timecode_column=None, timebucket_duration=None, timezero_date=None, columns_list=None, sequence_column=None,
seq_max=None, set_table=False)
DESCRIPTION:
    Writes records stored in a teradataml GeoDataFrame to Teradata Vantage.
PARAMETERS:
    table_name:
        Required Argument.
        Specifies the name of the table to be created in Teradata Vantage.
        Types: str
    schema_name:
        Optional Argument.
        Specifies the name of the SQL schema in Teradata Vantage to write to.
        Default Value: None (Use default Teradata Vantage schema).
        Types: str
        Note: schema_name will be ignored when temporary=True.
    if_exists:
        Optional Argument.
        Specifies the action to take when table already exists in Teradata Vantage.
        Default Value: 'fail'
        Permitted Values: 'fail', 'replace', 'append'
            - fail: If table exists, do nothing.
            - replace: If table exists, drop it, recreate it, and insert data.
            - append: If table exists, insert data. Create table, if does not exist.
        Types: str
        Note: Replacing a table with the contents of a teradataml GeoDataFrame based on
              the same underlying table is not supported.
    primary_index:
        Optional Argument.
        Creates Teradata table(s) with primary index column(s) when specified.
        When None, No primary index Teradata tables are created.
        Default Value: None
        Types: str or List of Strings (str)
            Example:
                primary_index = 'my_primary_index'
                primary_index = ['my_primary_index1', 'my_primary_index2', 'my_primary_index3']
    temporary:
        Optional Argument.
        Creates Teradata SQL tables as permanent or volatile.
        When True,
            1. volatile tables are created, and
            2. schema_name is ignored.
        When False, permanent tables are created.
        Default Value: False
        Types: boolean
    types:
        Optional Argument.
        Specifies required data types for requested columns to be saved in Vantage.
        Types: Python dictionary ({column_name1: type_value1, ... column_nameN: type_valueN})
        Default: None
        Note:
            1. This argument accepts a dictionary of columns names and their required teradatasqlalchemy types
               as key-value pairs, allowing to specify a subset of the columns of a specific type.
               When only a subset of all columns are provided, the column types for the rest are retained.
               When types argument is not provided, the column types are retained.
            2. This argument does not have any effect when the table specified using table_name and schema_name
               exists and if_exists = 'append'.
    primary_time_index_name:
        Optional Argument.
        Specifies a name for the Primary Time Index (PTI) when the table
        to be created must be a PTI table.
        Types: String
        Note: This argument is not required or used when the table to be created
              is not a PTI table. It will be ignored if specified without the timecode_column.
    timecode_column:
        Optional Argument.
        Required when the GeoDataFrame must be saved as a PTI table.
        Specifies the column in the GeoDataFrame that reflects the form
        of the timestamp data in the time series.
        This column will be the TD_TIMECODE column in the table created.
        It should be of SQL type TIMESTAMP(n), TIMESTAMP(n) WITH TIMEZONE, or DATE,
        corresponding to Python types datetime.datetime or datetime.date.
        Types: String
        Note: When you specify this parameter, an attempt to create a PTI table
              will be made. This argument is not required when the table to be created
              is not a PTI table. If this argument is specified, primary_index will be ignored.
    timezero_date:
        Optional Argument.
        Used when the GeoDataFrame must be saved as a PTI table.
        Specifies the earliest time series data that the PTI table will accept;
        a date that precedes the earliest date in the time series data.
        Value specified must be of the following format: DATE 'YYYY-MM-DD'
        Default Value: DATE '1970-01-01'.
        Types: String
        Note: This argument is not required or used when the table to be created
              is not a PTI table. It will be ignored if specified without the timecode_column.
    timebucket_duration:
        Optional Argument.
        Required if columns_list is not specified or is None.
        Used when the GeoDataFrame must be saved as a PTI table.
        Specifies a duration that serves to break up the time continuum in
        the time series data into discrete groups or buckets.
        Specified using the formal form time_unit(n), where n is a positive
        integer, and time_unit can be any of the following:
        CAL_YEARS, CAL_MONTHS, CAL_DAYS, WEEKS, DAYS, HOURS, MINUTES,
        SECONDS, MILLISECONDS, or MICROSECONDS.
        Types:  String
        Note: This argument is not required or used when the table to be created
              is not a PTI table. It will be ignored if specified without the timecode_column.
    columns_list:
        Optional Argument.
        Required if timebucket_duration is not specified.
        Used when the GeoDataFrame must be saved as a PTI table.
        Specifies a list of one or more PTI table column names.
        Types: String or list of Strings
        Note: This argument is not required or used when the table to be created
              is not a PTI table. It will be ignored if specified without the timecode_column.
    sequence_column:
        Optional Argument.
        Used when the GeoDataFrame must be saved as a PTI table.
        Specifies the column of type Integer containing the unique identifier for
        time series data readings when they are not unique in time.
        * When specified, implies SEQUENCED, meaning more than one reading from the same
          sensor may have the same timestamp.
          This column will be the TD_SEQNO column in the table created.
        * When not specified, implies NONSEQUENCED, meaning there is only one sensor reading
          per timestamp.
          This is the default.
        Types: str
        Note: This argument is not required or used when the table to be created
              is not a PTI table. It will be ignored if specified without the timecode_column.
    seq_max:
        Optional Argument.
        Used when the GeoDataFrame must be saved as a PTI table.
        Specifies the maximum number of sensor data rows that can have the
        same timestamp. Can be used when 'sequenced' is True.
        Accepted range:  1 - 2147483647.
        Default Value: 20000.
        Types: int
        Note: This argument is not required or used when the table to be created
              is not a PTI table. It will be ignored if specified without the timecode_column.
    set_table:
        Optional Argument.
        Specifies a flag to determine whether to create a SET or a MULTISET table.
        When True, a SET table is created.
        When False, a MULTISET table is created.
        Default value: False
        Types: boolean
        Note: 1. Specifying set_table=True also requires specifying primary_index or timecode_column.
              2. Creating SET table (set_table=True) may result in loss of duplicate rows.
              3. This argument has no effect if the table already exists and if_exists='append'.
RETURNS:
    None
RAISES:
    TeradataMlException
EXAMPLES:
    >>> load_example_data("geodataframe","sample_streets")
    >>> df = GeoDataFrame("sample_streets")
    >>> df2 = df[(df.skey == 1)]
    >>> df2.to_sql('to_sql_example', primary_index='skey')
    >>> df3 = GeoDataFrame('to_sql_example')
    >>> df3
          street_name              street_shape
    skey
    1     Main Street  LINESTRING (2 2,3 2,4 1)
    1      Coast Blvd  LINESTRING (12 12,18 17)
    >>>
Geospatial Speciﬁc Methods
Methods for All Types of Geometries
buffer
teradataml.geospatial.geodataframe.GeoDataFrame.buffer = buffer(self, distance)
DESCRIPTION:
    Returns all points whose distance from a Geometry value is less than or
    equal to a specified distance.
    Note:
        *  The "distance" argument is measured in the linear units of measure in
          the spatial reference system of the Geometry value.
PARAMETERS:
    distance:
        Required Argument.
        Specifies the distance from the geometry value to the buffer.
        Types: float, str, ColumnExpression
SUPPORTED GEOMETRY TYPES:
    All Geometry types
    Note:
        This method can be called on 3D geometries (those that include Z
        coordinates). However, the Z coordinate is dropped, and only x and y
        coordinates are returned.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame with result column containing Geometry values
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only skey and points columns from GeoDataFrame.
    points = geodf.select(["skey", "points"])
    # Example 1: Get all the points with distance less than or equal to 2.8 from the geometries
    #            specified in column 'points'
    points.buffer(2.8)
contains
teradataml.geospatial.geodataframe.GeoDataFrame.contains = contains(self, geom_column)
DESCRIPTION:
    Tests if a Geometry value spatially contains another Geometry value.
    Note:
        *  Using a geospatial index can greatly improve the performance of
          queries that use this method.
PARAMETERS:
    geom_column:
        Required Argument.
        Specifies the other Geometry value.
        Types: str, ColumnExpression, GeometryType
SUPPORTED GEOMETRY TYPES:
    All Geometry types except geometry collections.
    Note:
        This method can be called on 3D geometries (those that include Z
        coordinates). However, the Z coordinate is ignored in method
        calculations.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
    Resultant column contains:
        * 1, if the input geometry contains other geometry
        * 0, if the input geometry does not contain other geometry
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only few columns from GeoDataFrame.
    points_lines = geodf.select(["skey", "points", "linestrings"])
    # Example 1: Check if the ST_Linestring/ST_MultiLinestring type geometries in column
    #            'linestrings' contains the geometries in column 'points' of type ST_Point/ST_MultiPoint.
    # First, set the geometry property of GeoDataFrame to point to 'linestrings' column.
    points_lines.geometry = points_lines.linestrings
    # Pass the GeoDataFrameColumn object of 'points' columns as input to "geom_column" argument.
    points_lines.contains(geom_column=points_lines.points)
    # Example 2: Check if the ST_Linestring/ST_MultiLinestring type geometries in column
    #            'linestrings' contains the geometry specified by a GeometryType object - Point.
    # First, set the geometry property of GeoDataFrame to point to 'points' column.
    points_lines.geometry = points_lines.points
    # Create a Point GeometryType object with co-ordinates 1 and 3.
    point1 = Point(1, 3)
    # Pass the Point() GeometryType object as input to "geom_column" argument.
    points_lines.contains(geom_column=point1)
crosses
teradataml.geospatial.geodataframe.GeoDataFrame.crosses = crosses(self, geom_column)
DESCRIPTION:
    Tests if a Geometry value spatially crosses another Geometry value.
    Note:
        *  Using a geospatial index can greatly improve the performance of
          queries that use this method.
PARAMETERS:
    geom_column:
        Required Argument.
        Specifies the other Geometry value.
        Types: str, ColumnExpression, GeometryType
SUPPORTED GEOMETRY TYPES:
    All Geometry types except geometry collections.
    Note:
        This method can be called on 3D geometries (those that include Z
        coordinates). However, the Z coordinate is ignored in method
        calculations.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
    Resultant column contains:
        * 1, if the geometries cross
        * 0, if the geometries do not cross
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only few columns from GeoDataFrame.
    points_lines = geodf.select(["skey", "points", "linestrings"])
    # Example 1: Check if the ST_Linestring/ST_MultiLinestring type geometries in column
    #            'linestrings' crosses the geometries in column 'points' of type ST_Point/ST_MultiPoint.
    # Set the geometry property of GeoDataFrame to point to 'linestrings' column.
    points_lines.geometry = points_lines.linestrings
    # Pass the GeoDataFrameColumn object of 'points' columns as input to "geom_column" argument.
    points_lines.crosses(geom_column=points_lines.points)
    # Example 2: Check if the ST_Linestring/ST_MultiLinestring type geometries in column
    #            'linestrings' crosses the geometry specified by a GeometryType object - LineString.
    # Create an object of Linestring GeometryType.
    line1 = LineString([(3, 1), (0,2)])
    # Pass the Linestring() GeometryType object as input to "geom_column" argument.
    points_lines.crosses(geom_column=line1)
difference
teradataml.geospatial.geodataframe.GeoDataFrame.difference = difference(self, geom_column)
DESCRIPTION:
    Returns a Geometry value that represents the point set difference of
    two Geometry values.
PARAMETERS:
    geom_column:
        Required Argument.
        Specifies the other Geometry value.
        Types: str, ColumnExpression, GeometryType
SUPPORTED GEOMETRY TYPES:
    All Geometry types except geometry collections.
    Note:
        This method can be called on 3D geometries (those that include Z
        coordinates). However, the Z coordinate is ignored in method
        calculations. Consequently, any Z coordinates returned by this
        method should be ignored. Teradata recommends using the make_2D()
        method to strip out the Z coordinates of the return value.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
    Notes:
        Returns a Geometry type where the representation is one of the
        possible set of types in the following table, depending on the
        parameter types.
        +==============+=====+=====+==============+======+======+=======+=======+
        | a - b        | Ø   | Pnt | Line, GeoSeq | Poly | MPnt | MLine | MPoly |
        +==============+=====+=====+==============+======+======+=======+=======+
        | Ø            | Ø   | Ø   | Ø            | Ø    | Ø    | Ø     | Ø     |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        | Pnt          | R01 | R09 | R09          | R09  | R09  | R09   | R09   |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        | Line, GeoSeq | R02 | R02 | R08          | R08  | R02  | R08   | R08   |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        | Poly         | R03 | R03 | R03          | R14  | R14  | R14   | R14   |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        | MPnt         | R04 | R13 | R13          | R13  | R13  | R13   | R13   |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        | MLine        | R05 | R05 | R08          | R08  | R05  | R08   | R08   |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        | MPoly        | R06 | R06 | R06          | R14  | R06  | R05   | R06   |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        Where:
            Pnt, R01   = ST_Point
            Line, R02  = ST_LineString
            Poly, R03  = ST_Polygon
            MPnt, R04  = ST_MultiPoint
            MLine, R05 = ST_MultiLineString
            MPoly, R06 = ST_MultiPolygon
            GeoSeq     = GeoSequence
            R08        = Ø, ST_LineString, ST_MultiLineString
            R09        = Ø, ST_Point
            R13        = Ø, ST_Point, ST_MultiPoint
            R14        = Ø, ST_Polygon, ST_MultiPolygon
        Vantage converts GeoSequence types that are involved in the difference()
        method to ST_LineString values. Therefore, difference() never returns a GeoSequence
        type.
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only few columns from GeoDataFrame.
    points_lines = geodf.select(["skey", "points", "linestrings"])
    # Example 1: Get the difference between ST_Linestring/ST_MultiLinestring geometries in column
    #            'linestrings' and ST_Point/ST_MultiPoint geometries in column 'points'.
    # Set the geometry property of GeoDataFrame to point to 'linestrings' column.
    points_lines.geometry = points_lines.linestrings
    points_lines.difference(geom_column=points_lines.points)
    # Example 2: Get the difference between ST_Linestring/ST_MultiLinestring geometries in column
    #            'linestrings' and the geometry specified by a GeometryType object - LineString.
    # Create an object of Linestring GeometryType.
    l1 = LineString([(3, 1), (0,2)])
    # Pass the LineString() GeometryType object as input to "geom_column" argument.
    points_lines.difference(geom_column=l1)
disjoint
teradataml.geospatial.geodataframe.GeoDataFrame.disjoint = disjoint(self, geom_column)
DESCRIPTION:
    Tests if a Geometry value is spatially disjoint from another Geometry
    value.
PARAMETERS:
    geom_column:
        Required Argument.
        Specifies the other Geometry value.
        Types: str, ColumnExpression, GeometryType
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
    Note:
        This method can be called on 3D geometries (those that include Z
        coordinates). However, the Z coordinate is ignored in method
        calculations.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
    Resultant column contains:
        * 1, if the geometries are spatially disjoint
        * 0, if the geometries are not spatially disjoint
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only few columns from GeoDataFrame.
    points_lines = geodf.select(["skey", "points", "linestrings"])
    # Example 1: Check whether the ST_Linestring/ST_MultiLinestring geometries in column
    #            'linestrings' and ST_Point/ST_MultiPoint geometries in column 'points'
    #            are disjoint or not.
    # Set the geometry property of GeoDataFrame to point to 'linestrings' column.
    points_lines.geometry = points_lines.linestrings
    points_lines.disjoint(geom_column=points_lines.points)
    # Example 2: Check whether the ST_Linestring/ST_MultiLinestring geometries in column
    #            'linestrings' and the geometry specified by a GeometryType object - LineString,
    #            are disjoint or not.
    # Create an object of Linestring GeometryType.
    l1 = LineString([(3, 1), (0,2)])
    # Pass the LineString() GeometryType object as input to "geom_column" argument.
    points_lines.disjoint(geom_column=l1)
distance
teradataml.geospatial.geodataframe.GeoDataFrame.distance = distance(self, geom_column)
DESCRIPTION:
    Returns the distance between two Geometry values.
    Notes:
        *  distance() returns NULL if the geometry value passed to it is the
          empty set.
        *  Both geometries must use the same spatial reference system (have the
          same SRS ID).
        *  If a geospatial index is defined on the geospatial data column to
          which this method is applied, the index will not be used. If user wants
          index to be applied, then use the method on GeoDataFrame column with
          slice filtering such that the distance between the geometries is
          less than or equal to a constant.
PARAMETERS:
    geom_column:
        Required Argument.
        Specifies the other Geometry value.
        Types: str, ColumnExpression, GeometryType
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only few columns from GeoDataFrame.
    points_lines = geodf.select(["skey", "points", "linestrings"])
    # Example 1: Calculate the distance between (ST_Linestring/ST_MultiLinestring) geometries in column
    #            'linestrings' and (ST_Point/ST_MultiPoint) geometries in column 'points'.
    points_lines.distance(geom_column=points_lines.linestrings)
    # Example 2: Calculate the distance between (ST_Linestring/ST_MultiLinestring) geometries in column
    #            'linestrings' and the geometry specified by a GeometryType object - Point.
    # Create an object of Point GeometryType.
    p1 = Point(3, 1)
    # Pass the Point() GeometryType object as input to "geom_column" argument.
    points_lines.distance(geom_column=p1)
distance_3D
teradataml.geospatial.geodataframe.GeoDataFrame.distance_3D = distance_3D(self, geom_column)
DESCRIPTION:
    Returns the distance between two Geometry values. Function
    considers Z coordinate values, if they exist in the geometries passed to
    it.
    Notes:
        *  distance() returns NULL if the geometry value passed to it is the
          empty set.
        *  Both geometries must use the same spatial reference system (have the
          same SRS ID).
        *  If a geospatial index is defined on the geospatial data column to
          which this method is applied, the index will not be used. If user wants
          index to be applied, then use the method on GeoDataFrame column with
          slice filtering such that the distance between the geometries is
          less than or equal to a constant.
PARAMETERS:
    geom_column:
        Required Argument.
        Specifies the other Geometry value.
        Types: str, ColumnExpression, GeometryType
SUPPORTED GEOMETRY TYPES:
    ST_Point, ST_MultiPoint, ST_LineString, and ST_MultiLineString
    To be valid with distance_3D, these shapes must include Z coordinates.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
    Resultant column contains:
        * a NULL, if the Geometry is an empty set.
        * 0, if the two geometries intersect.
        * a float in all other cases. The distance units are those of
          the two input geometries.
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Function works with only 3D geometries, hence let's select only few columns from GeoDataFrame
    # and filter the rows which contains 3D geometries.
    points_lines = geodf.select(["skey", "points", "linestrings"])[geodf.points.is_3D == 1]
    # Example 1: Calculate the distance between 3D geometries in column 'linestrings' and 3D
    #            geometries in column 'points'.
    points_lines.distance_3D(geom_column=points_lines.linestrings)
    # Example 2: Calculate the distance between 3D geometries in column 'linestrings' and 3D Point
    #            GeometryType object.
    # Create a 3D Point GeometryType object.
    p1 = Point(3, 1, 0)
    # Pass the Point() GeometryType object as input to "geom_column" argument.
    points_lines.distance_3D(geom_column=p1)
envelope
teradataml.geospatial.geodataframe.GeoDataFrame.envelope = envelope(self)
DESCRIPTION:
    Returns the bounding rectangle for the Geometry value.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
    Note:
        This method can be called on 3D geometries (those that include Z
        coordinates). However, the Z coordinate is dropped, and only x and y
        coordinates are returned.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame with result column containing Geometry values
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Get the bounding rectangle for (ST_Point/ST_MultiPoint) geometries in column
    #            'points'.
    points_lines = geodf.select(["skey", "points", "linestrings"])
    points_lines.envelope()
    # Example 2: Get the bounding rectangle for (ST_Linestring/ST_MultiLinestring) geometries in column
    #            'linestrings'.
    # Set the geometry property of GeoDataFrame to point to 'linestrings' column.
    points_lines.geometry = points_lines.linestrings
    points_lines.envelope()
geom_equals
teradataml.geospatial.geodataframe.GeoDataFrame.geom_equals = geom_equals(self, geom_column)
DESCRIPTION:
    Tests if a Geometry value is spatially equal to another Geometry value.
    Note:
        *  Using a geospatial index can greatly improve the performance of
          queries that use this method.
PARAMETERS:
    geom_column:
        Required Argument.
        Specifies the other Geometry value.
        Types: str, ColumnExpression, GeometryType
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
    Resultant column contains:
        * 1, if the geometries are spatially equal
        * 0, if the geometries are not spatially equal
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only few columns from GeoDataFrame.
    points_lines = geodf.select(["skey", "points", "linestrings"])
    # Example 1: Check whether the ST_Point/ST_MultiPoint type geometries in column
    #            'points' equals geometry specified by a GeometryType object - Point.
    # Create an object of Point GeometryType.
    p1 = Point(10, 20)
    # Pass the Point() GeometryType object as input to "geom_column" argument.
    points_lines.geom_equals(geom_column=p1)
    # Example 2: Check whether the ST_Linestring/ST_MultiLinestring type geometries in column
    #            'linestrings' equals geometry specified by a GeometryType object - LineString.
    # Set the geometry property of GeoDataFrame to point to 'linestrings' column.
    points_lines.geometry = points_lines.linestrings
    # Create an object of Linestring GeometryType.
    l1 = LineString([(1,3), (3,0), (0,1)])
    # Pass the LineString() GeometryType object as input to "geom_column" argument.
    points_lines.geom_equals(geom_column=l1)
intersection
teradataml.geospatial.geodataframe.GeoDataFrame.intersection = intersection(self, geom_column)
DESCRIPTION:
    Returns a Geometry type where the value represents the point set
    intersection of two Geometry values.
PARAMETERS:
    geom_column:
        Required Argument.
        Specifies the other Geometry value.
        Types: str, ColumnExpression, GeometryType
SUPPORTED GEOMETRY TYPES:
    All Geometry types, including GeoSequence, but not the ST_GeomCollection type.
    Note:
        This method can be called on 3D geometries (those that include Z
        coordinates). However, the Z coordinate is ignored in method
        calculations. Consequently, any Z coordinates returned by this
        method should be ignored. Teradata recommends using the make_2D
        method to strip out the Z coordinates of the return value.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
    Notes:
        Returns a Geometry type where the representation is one of the
        possible set of types in the following table, depending on the
        parameter types.
        +==============+=====+=====+==============+======+======+=======+=======+
        | a n b        | Ø   | Pnt | Line, GeoSeq | Poly | MPnt | MLine | MPoly |
        +==============+=====+=====+==============+======+======+=======+=======+
        | Ø            | Ø   | Ø   | Ø            | Ø    | Ø    | Ø     | Ø     |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        | Pnt          | Ø   | R09 | R09          | R09  | R09  | R09   | R09   |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        | Line, GeoSeq | Ø   | R09 | R11          | R11  | R13  | R11   | R11   |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        | Poly         | Ø   | R09 | R11          | R12  | R13  | R11   | R12   |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        | MPnt         | Ø   | R09 | R13          | R13  | R13  | R13   | R13   |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        | MLine        | Ø   | R09 | R11          | R11  | R13  | R11   | R11   |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        | MPoly        | Ø   | R09 | R11          | R12  | R13  | R11   | R12   |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        Where:
            Pnt     = ST_Point
            Line    = ST_LineString
            Poly    = ST_Polygon
            MPnt    = ST_MultiPoint
            MLine   = ST_MultiLineString
            MPoly   = ST_MultiPolygon
            GeoSeq  = GeoSequence
            R09     = Ø, ST_Point
            R11     = Ø, ST_Point, ST_LineString, ST_MultiPoint, ST_MultiLineString
            R12     = Ø, ST_Point, ST_LineString, ST_Polygon, ST_MultiPoint,
                      ST_MultiLineString, ST_MultiPolygon
            R13     = Ø, ST_Point, ST_MultiPoint
        Vantage converts GeoSequence types that are involved in the intersection()
        method to ST_LineString values. Therefore, intersection() never returns a
        GeoSequence type.
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only few columns from GeoDataFrame.
    points_lines = geodf.select(["skey", "points", "linestrings"])
    # Example 1: Get the point set interection geometry value between the geometries in columns
    #            'points' and 'linestrings'.
    points_lines.intersection(geom_column=points_lines.linestrings)
    # Example 2: Get the point set interection geometry value between the geometries in column
    #            'linestrings' and geometry type specified by Linestring.
    # Set the geometry property of GeoDataFrame to point to 'linestrings' column.
    points_lines.geometry = points_lines.linestrings
    # Create an object of Linestring GeometryType.
    l1 = LineString([(1,3), (3,0), (0,1)])
    # Pass the LineString() GeometryType object as input to "geom_column" argument.
    points_lines.intersection(l1)
intersects
teradataml.geospatial.geodataframe.GeoDataFrame.intersects = intersects(self, geom_column)
DESCRIPTION:
    Tests if a Geometry value spatially intersects another Geometry value.
    Note:
        *  Using a geospatial index can greatly improve the performance of
          queries that use this method.
PARAMETERS:
    geom_column:
        Required Argument.
        Specifies the other Geometry value.
        Types: str, ColumnExpression, GeometryType
SUPPORTED GEOMETRY TYPES:
    All Geometry types except geometry collections.
    Note:
        This method can be called on 3D geometries (those that include Z
        coordinates). However, the Z coordinate is ignored in method
        calculations.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
    Resultant column contains:
        * 1, if the geometries intersect
        * 0, if the geometries do not intersect
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only few columns from GeoDataFrame.
    points_lines = geodf.select(["skey", "points", "linestrings"])
    # Example 1: Check whether the ST_Point/ST_MultiPoint type geometries in column
    #            'points' intersects with ST_Linestring/ST_MultiLinestring geometries
    #            in column 'linestrings'.
    points_lines.intersects(geom_column=points_lines.linestrings)
    # Example 2: Check whether the ST_Linestring/ST_MultiLinestring type geometries in column
    #            'linestrings' intersects with GeometryType object of Linestring.
    # Set the geometry property of GeoDataFrame to point to 'linestrings' column.
    points_lines.geometry = points_lines.linestrings
    # Create an object of Linestring GeometryType.
    l1 = LineString([(1,3), (3,0), (0,1)])
    # Pass the LineString() GeometryType object as input to "geom_column" argument.
    points_lines.intersects(l1)
make_2D
teradataml.geospatial.geodataframe.GeoDataFrame.make_2D = make_2D(self, validate=False)
DESCRIPTION:
    Converts a 3D Geometry value to a 2D Geometry value, and optionally
    validates that the 2D geometry is well-formed. This method strips the z
    coordinate from 3D geometries.
    Notes:
        *  The is_valid method can be used to validate converted 2D
          geometries that were not validated by make_2D().
        *  If the input geometry is already a 2D geometry value, this value is
          returned unchanged by this method.
        *  A well-formed 3D geometry value can become invalid during a 3D to 2D
          conversion. For example, if a 3D polygon lies in a plane parallel to
          the y axis, its line segments will self-intersect. make_2D() does not
          attempt to change the geometry type of the converted geometry from
          polygon to linestring to account for this, and an invalid 2D
          geometry can result.
PARAMETERS:
    validate:
        Optional Argument.
        Specifies a value that determines whether the converted 2D geometry
        is tested to ensure it is well-formed. When "validate" is set to True,
        the 2D geometry is tested. If it is not well-formed, Vantage
        returns an error. If validate is set to False or is not specified,
        the 2D geometry is not tested.
        Default Value: False
        Types: bool, str, ColumnExpression
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Convert 3D ST_Point/ST_MultiPoint geometries in column 'points' to 2D.
    # Let's select only few columns from GeoDataFrame and get only rows which contains 3D geometries.
    points = geodf.select(["skey", "points"])[geodf.points.is_3D == 1]
    points.make_2D()
mbb
teradataml.geospatial.geodataframe.GeoDataFrame.mbb = mbb(self)
DESCRIPTION:
    Returns the 3D minimum bounding box (MBB) that encloses a 3D Geometry.
    Note:
        *  mbb() can return a two-dimensional MBB, if appropriate. Such an MBB
          has Z coordinates that are all zero. If the 3D input geometry is
          empty, all the coordinates of the resulting MBB will be zero.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    All 3D Geometry types.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only few columns from GeoDataFrame and select only geometries that are 3D.
    pos = geodf.select(["skey", "points", "polygons"])[geodf.points.is_3D == 1]
    # Example 1: Get the 3D minimum bounding box for 3D ST_Point/ST_MultiPoint geometries
    #            in column 'points'.
    pos.mbb()
    # Example 2: Get the 3D minimum bounding box for 3D ST_Polygon/ST_MultiPolygon geometries
    #            in column 'polygons'.
    # Set the geometry property of GeoDataFrame to point to 'polygons' column.
    pos.geometry = pos.polygons
    pos.mbb()
mbr
teradataml.geospatial.geodataframe.GeoDataFrame.mbr = mbr(self)
DESCRIPTION:
    Returns the minimum bounding rectangle (MBR) of a Geometry value.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only few columns from GeoDataFrame.
    pos = geodf.select(["skey", "points", "polygons"])
    # Example 1: Get the minimum bounding rectangle for ST_Point/ST_MultiPoint geometries
    #            in column 'points'.
    pos.mbr()
    # Example 2: Get the minimum bounding rectangle for ST_Polygon/ST_MultiPolygon geometries
    #            in column 'polygons'.
    # Set the geometry property of GeoDataFrame to point to 'polygons' column.
    pos.geometry = pos.polygons
    pos.mbr()
overlaps
teradataml.geospatial.geodataframe.GeoDataFrame.overlaps = overlaps(self, geom_column)
DESCRIPTION:
    Tests if a Geometry value spatially overlaps another Geometry value.
    Note:
        *  Using a geospatial index can greatly improve the performance of
          queries that use this method.
PARAMETERS:
    geom_column:
        Required Argument.
        Specifies the other Geometry value.
        Types: str, ColumnExpression, GeometryType
SUPPORTED GEOMETRY TYPES:
    Valid on all Geometry types, except geometry collections.
    Note:
        This method can be called on 3D geometries (those that include Z
        coordinates). However, the Z coordinate is ignored in method
        calculations.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
    Resultant column contains:
        * 1, if the geometries overlap
        * 0, if the geometries do not overlap
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only few columns from GeoDataFrame.
    pols_lines = geodf.select(["skey", "polygons", "linestrings"])
    # Example 1: Check whether geometries in column 'polygons' and 'lienstrings'
    #            overlaps or not.
    # Set the geometry property of GeoDataFrame to point to 'polygons' column.
    pols_lines.geometry = pols_lines.polygons
    pols_lines.overlaps(geom_column=pols_lines.linestrings)
    # Example 2: Check whether geometries in column 'lienstrings' columns and
    #            a Linestring GeometryType object overlaps or not.
    # Set the geometry property of GeoDataFrame to point to 'linestrings' column.
    pols_lines.geometry = pols_lines.linestrings
    # Create an object of Linestring GeometryType.
    l1 = LineString([(1,3,6), (3,0,6), (6,0,1)])
    # Pass the LineString() GeometryType object as input to "geom_column" argument.
    pols_lines.overlaps(l1)
relates
teradataml.geospatial.geodataframe.GeoDataFrame.relates = relates(self, geom_column, amatrix)
DESCRIPTION:
    Tests if a Geometry value is spatially related to another Geometry
    value.
    Note:
        *  Using a geospatial index can greatly improve the performance of
          queries that use this method.
PARAMETERS:
    geom_column:
        Required Argument.
        Specifies the other Geometry value.
        Types: str, ColumnExpression, GeometryType
    amatrix:
        Required Argument.
        Specifies a value where each character corresponds to a cell in the
        DE-9IM pattern matrix.
        Types: str
        Notes:
            The mapping for amatrix, as defined by SQL/MM Spatial, is as follows:
            +==========+=====================================================+
            | Position | DE-9IM Cell                                         |
            +==========+=====================================================+
            | 1        | (Interior(SELF) ∩ Interior(ageometry)).dimension    |
            +----------+-----------------------------------------------------+
            | 2        | (Interior(SELF) ∩ Boundary(ageometry)).dimension    |
            +----------+-----------------------------------------------------+
            | 3        | (Interior(SELF) ∩ Exterior(ageometry)).dimension    |
            +----------+-----------------------------------------------------+
            | 4        | (Boundary(SELF) ∩ Interior(ageometry)).dimension    |
            +----------+-----------------------------------------------------+
            | 5        | (Boundary(SELF) ∩ Boundary(ageometry)).dimension    |
            +----------+-----------------------------------------------------+
            | 6        | (Boundary(SELF) ∩ Exterior(ageometry)).dimension    |
            +----------+-----------------------------------------------------+
            | 7        | (Exterior(SELF) ∩ Interior(ageometry)).dimension    |
            +----------+-----------------------------------------------------+
            | 8        | (Exterior(SELF) ∩ Boundary(ageometry)).dimension    |
            +----------+-----------------------------------------------------+
            | 9        | (Exterior(SELF) ∩ Exterior(ageometry)).dimension    |
            +==========+=====================================================+
            Valid values for each character are: 'T', 'F', '0', '1', '2', and '*'.
            The value specifies the set of acceptable values for an intersection
            at that given cell. The meaning is as follows:
            +============+==========================+
            | Cell Value | Intersection Set Results |
            +============+==========================+
            | 'T'        | { 0, 1, 2 }              |
            +------------+--------------------------+
            | 'F'        | { -1 }                   |
            +------------+--------------------------+
            | '0'        | { 0 }                    |
            +------------+--------------------------+
            | '1'        | { 1 }                    |
            +------------+--------------------------+
            | '2'        | { 2 }                    |
            +------------+--------------------------+
            | '*'        | { -1, 0, 1, 2 }          |
            +============+==========================+
SUPPORTED GEOMETRY TYPES:
    All Geometry types except geometry collections.
    Note:
        This method can be called on 3D geometries (those that include Z
        coordinates). However, the Z coordinate is ignored in method
        calculations.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
    Resultant column contains:
        * 1, if the spatial relationship between the geometries corresponds
          to one of the acceptable values represented by "amatrix"
        * 0, if the spatial relationship between the geometries does not
          correspond to one of the acceptable values represented by "amatrix"
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Check if geometry values in column 'points' are spatially related to
    #            geometry values in column 'linestrings'.
    # Let's select only few columns from GeoDataFrame.
    point_lines = geodf.select(["skey", "points", "linestrings"])
    point_lines.relates(geom_column=point_lines.linestrings, amatrix="0********")
set_exterior
teradataml.geospatial.geodataframe.GeoDataFrame.set_exterior = set_exterior(self)
DESCRIPTION:
    Set the exterior ring of a Geometry type that represents an
    ST_Polygon value.
    Note:
        *  set_exterior() returns an error if the geometry value passed to it
          is the empty set.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    ST_Polygon
    Note:
        This method can be called on 3D geometries (those that include Z
        coordinates). However, the Z coordinate is ignored in method
        calculations. Consequently, any Z coordinates returned by this
        method should be ignored. Teradata recommends using the make_2D()
        method to strip out the Z coordinates of the return value.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
EXAMPLES:
    TODO
set_srid
teradataml.geospatial.geodataframe.GeoDataFrame.set_srid = set_srid(self, srid)
DESCRIPTION:
    Set the spatial reference system identifier of the Geometry
    value.
PARAMETERS:
    srid:
        Required Argument.
        Specifies a value for the spatial reference system identifier.
        Types: int, str, ColumnExpression
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
    Resultant column contains the Geometry with the spatial reference system identifier set
    to the specified spatial reference system.
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Set the spatial reference system identifier for the geometries in column 'geosequence'.
    # Let's select only few columns from GeoDataFrame.
    geoseq = geodf.select(["skey", "geosequence"])[geodf.skey.isin([1001, 1002])]
    geoseq.set_srid(srid=20)
simplify
teradataml.geospatial.geodataframe.GeoDataFrame.simplify = simplify(self, tolerance)
DESCRIPTION:
    Simplifies a geometry by removing points that would fall within a
    specified distance tolerance.
    Notes:
        *  The simplification always returns a valid geometry.
        *  Simplified geometries require less storage space and fewer spatial
          operations during geospatial manipulations. Consequently, operations
          on simplified geometries generally perform faster.
        *  Smaller tolerance values result in a geometry closer to the input
          geometry, but will remove fewer vertices. Larger tolerance values
          will remove more vertices, but the resulting simplified geometry
          will be less similar to the original input.
PARAMETERS:
    tolerance:
        Required Argument.
        Specifies the distance tolerance for the simplification. The
        simplified line will never be farther away from the original line
        than the tolerance value.
        Types: float, str, ColumnExpression
SUPPORTED GEOMETRY TYPES:
    Valid on the following 2D geometries:
        LineString, MultiLineString, Polygon, and MultiPolygon.
    It is valid on these types within Geometry Collections. If method is called on other 2D
    geometries, those geometries are returned unchanged.
    This method is not valid on 3D geometries.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame with result column containing Geometry values
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only few columns from GeoDataFrame.
    pols_lines = geodf.select(["skey", "polygons", "linestrings"])[geodf.skey.isin([1001, 1002, 1003])]
    # Example 1: Simplify the geometry in column 'polygons' within a specified distance tolerance of 2.8.
    # Set the geometry property of GeoDataFrame to point to 'polygons' column.
    pols_lines.geometry = pols_lines.polygons
    pols_lines.simplify(2.8)
    # Example 2: Simplify the geometry in column 'linestrings' within a specified distance tolerance of 2.8.
    # Set the geometry property of GeoDataFrame to point to 'linestrings' column.
    pols_lines.geometry = pols_lines.linestrings
    pols_lines.simplify(tolerance=2.8)
sym_difference
teradataml.geospatial.geodataframe.GeoDataFrame.sym_difference = sym_difference(self, geom_column)
DESCRIPTION:
    Returns a Geometry value that represents the point set symmetric
    difference of two Geometry values.
PARAMETERS:
    geom_column:
        Required Argument.
        Specifies the other Geometry value.
        Types: str, ColumnExpression, GeometryType
SUPPORTED GEOMETRY TYPES:
    All Geometry types except geometry collections.
    Note:
        This method can be called on 3D geometries (those that include Z
        coordinates). However, the Z coordinate is ignored in method
        calculations. Consequently, any Z coordinates returned by this
        method should be ignored. Teradata recommends using the make_2D()
        method to strip out the Z coordinates of the return value.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
    Notes:
        Returns a Geometry type where the representation is one of the
        possible set of types in the following table, depending on the
        parameter types.
        +==============+=====+=====+==============+======+======+=======+=======+
        | u (a - b)    | Ø   | Pnt | Line, GeoSeq | Poly | MPnt | MLine | MPoly |
        +==============+=====+=====+==============+======+======+=======+=======+
        | Ø            | Ø   | R01 | R02          | R03  | R04  | R05   | R06   |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        | Pnt          | R01 | R10 | R15          | R22  | R10  | R17   | R19   |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        | Line, GeoSeq | R02 | R15 | R08          | R21  | R15  | R08   | R18   |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        | Poly         | R03 | R22 | R21          | R14  | R22  | R21   | R14   |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        | MPnt         | R04 | R10 | R15          | R22  | R10  | R17   | R19   |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        | MLine        | R05 | R17 | R08          | R21  | R17  | R08   | R18   |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        | MPoly        | R06 | R19 | R18          | R14  | R19  | R18   | R14   |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        Where:
            Pnt, R01    = ST_Point
            Line, R02   = ST_LineString
            Poly, R03   = ST_Polygon
            MPnt, R04   = ST_MultiPoint
            MLine, R05  = ST_MultiLineString
            MPoly, R06  = ST_MultiPolygon
            GeoSeq      = GeoSequence
            R08         = Ø, ST_LineString, ST_MultiLineString
            R10         = Ø, ST_MultiPoint
            R14         = Ø, ST_Polygon, ST_MultiPolygon
            R15         = ST_LineString,
                          ST_GeomCollection of ST_Point and ST_LineString values
            R17         = ST_MultiLineString,
                          ST_GeomCollection of ST_Point and ST_LineString values
            R18         = ST_MultiPolygon,
                          ST_GeomCollection of ST_LineString and ST_Polygon values
            R19         = ST_MultiPolygon,
                          ST_GeomCollection of ST_Point and ST_Polygon values
            R21         = ST_Polygon,
                          ST_GeomCollection of ST_LineString and ST_Polygon values
            R22         = ST_Polygon,
                          ST_GeomCollection of ST_Point and ST_Polygon values
        Vantage converts GeoSequence types that are involved in the sym_difference()
        method to ST_LineString values. Therefore, sym_difference() never returns a GeoSequence
        type.
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only few columns from GeoDataFrame.
    points_lines = geodf.select(["skey", "points", "linestrings"])
    # Example 1: Get the point set symmetric difference between geometries in columns
    #            'points' and 'linestrings'.
    points_lines.sym_difference(geom_column=points_lines.linestrings)
    # Example 2: Get the point set symmetric difference between geometries in columns
    #            'linestrings' and a GeometryType object - Linestring.
    # Set the geometry property of GeoDataFrame to point to 'linestrings' column.
    points_lines.geometry = points_lines.linestrings
    # Create an object of Linestring GeometryType.
    l1 = LineString([(1,3), (3,0), (0,1)])
    # Pass the LineString() GeometryType object as input to "geom_column" argument.
    points_lines.sym_difference(l1)
to_binary
teradataml.geospatial.geodataframe.GeoDataFrame.to_binary = to_binary(self)
DESCRIPTION:
    Returns the well-known binary (WKB) representation of a Geometry value.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame with result column containing BLOB value
    The maximum size of the return value is 16 MB (the exact maximum is 16 MB - 1 KB).
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Get the well-known binary (WKB) representation of geometry values
    #            in column 'points'.
    # Let's select only few columns from GeoDataFrame.
    points_lines = geodf.select(["skey", "points", "linestrings"])
    points_lines.to_binary()
to_text
teradataml.geospatial.geodataframe.GeoDataFrame.to_text = to_text(self)
DESCRIPTION:
    Returns the well-known text (WKT) representation of a Geometry value.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame with result column containing CLOB value
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Get the well-known text (WKT) representation of geometry values
    #            in column 'linestrings'.
    # Let's select only few columns from GeoDataFrame.
    points_lines = geodf.select(["skey", "points", "linestrings"])
    # Set the geometry property of GeoDataFrame to point to 'linestrings' column.
    points_lines.geometry = points_lines.linestrings
    points_lines.to_text()
touches
teradataml.geospatial.geodataframe.GeoDataFrame.touches = touches(self, geom_column)
DESCRIPTION:
    Tests if a Geometry value spatially touches another Geometry value.
    Note:
        *  Using a geospatial index can greatly improve the performance of
          queries that use this method.
PARAMETERS:
    geom_column:
        Required Argument.
        Specifies the other Geometry value.
        Types: str, ColumnExpression, GeometryType
SUPPORTED GEOMETRY TYPES:
    All Geometry types except geometry collections.
    Note:
        This method can be called on 3D geometries (those that include Z
        coordinates). However, the Z coordinate is ignored in method
        calculations.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
    Resultant column contains:
        * 1, if the geometries touch
        * 0, if the geometries do not touch
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only few columns from GeoDataFrame.
    pols_lines = geodf.select(["skey", "polygons", "linestrings"])
    # Example 1: Check whether geometries in column 'polygons' and 'lienstrings'
    #            touches or not.
    # Set the geometry property of GeoDataFrame to point to 'linestrings' column.
    pols_lines.geometry = pols_lines.linestrings
    pols_lines.touches(geom_column=pols_lines.polygons)
    # Example 2: Check whether geometries in column 'polygons' columns and
    #            a Polygon GeometryType object touches or not.
    # Set the geometry property of GeoDataFrame to point to 'polygons' column.
    pols_lines.geometry = pols_lines.polygons
    # Create an object of Polygon GeometryType.
    po1 = Polygon([(0, 0), (0, 20), (20, 20), (20, 0), (0, 0)])
    # Pass the Polygon() GeometryType object as input to "geom_column" argument.
    pols_lines.touches(po1)
transform
teradataml.geospatial.geodataframe.GeoDataFrame.transform = transform(self, to_wkt_srs, from_wkt_srs, to_srsid=-12345)
DESCRIPTION:
    Returns a Geometry value transformed to the specified spatial reference
    system.
    Note:
        *  The SRTEXT column of the SYSSPATIAL.SPATIAL_REF_SYS metadata table
          contains WKT representations of spatial reference systems.
PARAMETERS:
    to_wkt_srs:
        Required Argument.
        Specifies a value that is the identifier of the spatial reference
        system returned by the method.
        Types: str, ColumnExpression
    from_wkt_srs:
        Required Argument.
        Specifies a value for the WKT representation of the spatial
        reference system to transform to.
        Types: str, ColumnExpression
    to_srsid:
        Optional Argument.
        Specifies a value for the WKT representation of the spatial
        reference system to assign to the geometry value (without any
        transformation) before transforming it to the spatial reference
        system specified by "to_wkt_srs".
        Default Value: -12345
        Types: int, str, ColumnExpression
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
    Note:
        This method can be called on 3D geometries (those that include Z
        coordinates). However, the Z coordinate is ignored in method
        calculations. Consequently, any Z coordinates returned by this
        method should be ignored. Teradata recommends using the make_2D()
        method to strip out the Z coordinates of the return value.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame with result column containing Geometry values
EXAMPLES:
    from teradataml import DataFrame, in_schema
    from teradataml import GeoDataFrame, load_example_data
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes").select(["skey", "points"])
    print(geodf)
    # Create a teradataml DataFrame on 'SYSSPATIAL.SPATIAL_REF_SYS'
    sysref = DataFrame(in_schema("SYSSPATIAL", "SPATIAL_REF_SYS"))
    # Join the teradataml GeoDataFrame and DataFrame.
    sysref = sysref.join(sysref, how="cross", lsuffix="l", rsuffix="r")
    geodf_sysref = geodf.join(sysref, on="skey==l_SRID")
    # Execute the transform function to transform geometry in column 'points'.
    geodf_sysref.transform(geodf_sysref.l_SRTEXT, geodf_sysref.r_SRTEXT)
union
teradataml.geospatial.geodataframe.GeoDataFrame.union = union(self, geom_column)
DESCRIPTION:
    Returns a Geometry value that represents the point set union of two
    Geometry values.
PARAMETERS:
    geom_column:
        Required Argument.
        Specifies the other Geometry value.
        Types: str, ColumnExpression, GeometryType
SUPPORTED GEOMETRY TYPES:
    All Geometry types except geometry collections.
    Note:
        This method can be called on 3D geometries (those that include Z
        coordinates). However, the Z coordinate is ignored in method
        calculations. Consequently, any Z coordinates returned by this
        method should be ignored. Teradata recommends using the make_2D()
        method to strip out the Z coordinates of the return value.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
    Notes:
        Returns a Geometry type where the representation is one of the
        possible set of types in the following table, depending on the
        parameter types.
        +==============+=====+=====+==============+======+======+=======+=======+
        | a u b        | Ø   | Pnt | Line, GeoSeq | Poly | MPnt | MLine | MPoly |
        +==============+=====+=====+==============+======+======+=======+=======+
        | Ø            | Ø   | R01 | R02          | R03  | R04  | R05   | R06   |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        | Pnt          | R01 | R20 | R15          | R22  | R04  | R17   | R19   |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        | Line, GeoSeq | R02 | R15 | R16          | R21  | R15  | R16   | R18   |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        | Poly         | R03 | R22 | R21          | R23  | R22  | R21   | R23   |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        | MPnt         | R04 | R04 | R15          | R22  | R04  | R17   | R19   |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        | MLine        | R05 | R17 | R16          | R21  | R17  | R16   | R18   |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        | MPoly        | R06 | R19 | R18          | R23  | R19  | R18   | R23   |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        Where:
            Pnt, R09    = ST_Point
            Line, R02   = ST_LineString
            Poly, R03   = ST_Polygon
            MPnt, R04   = ST_MultiPoint
            MLine, R05  = ST_MultiLineString
            MPoly, R06  = ST_MultiPolygon
            GeoSeq      = GeoSequence
            R15         = ST_LineString,
                          ST_GeomCollection of ST_Point and ST_LineString values
            R16         = ST_LineString, ST_MultiLineString
            R17         = ST_MultiLineString,
                          ST_GeomCollection of ST_Point and ST_LineString values
            R18         = ST_MultiPolygon,
                          ST_GeomCollection of ST_LineString and ST_Polygon values
            R19         = ST_MultiPolygon,
                          ST_GeomCollection of ST_Point and ST_Polygon values
            R20         = ST_Point, ST_MultiPoint
            R21         = ST_Polygon,
                          ST_GeomCollection of ST_LineString and ST_Polygon values
            R22         = ST_Polygon,
                          ST_GeomCollection of ST_Point ST_Polygon values
            R23         = ST_Polygon, ST_MultiPolygon
        Vantage converts GeoSequence types that are involved in the union()
        method to ST_LineString values. Therefore, union() never returns a
        GeoSequence type.
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only few columns from GeoDataFrame.
    pols_lines = geodf.select(["skey", "polygons", "linestrings"])[geodf.skey.isin([1001, 1002, 1003])]
    # Example 1: Get the point set union of geometries in columns 'polygons' and 'linestrings'.
    # Set the geometry property of GeoDataFrame to point to 'linestrings' column.
    pols_lines.geometry = pols_lines.linestrings
    pols_lines.union(geom_column=pols_lines.polygons)
    # Example 2: Get the point set union of geometries in column 'polygons' and
    #            a GeometryType object - Polygon.
    # Set the geometry property of GeoDataFrame to point to 'polygons' column.
    pols_lines.geometry = pols_lines.polygons
    # Create an object of Polygon GeometryType.
    po1 = Polygon([(0, 0), (0, 20), (20, 20), (20, 0), (0, 0)])
    # Pass the Polygon() GeometryType object as input to "geom_column" argument.
    pols_lines.union(po1)
within
teradataml.geospatial.geodataframe.GeoDataFrame.within = within(self, geom_column)
DESCRIPTION:
    Tests if a Geometry value is spatially within another Geometry value.
    Note:
        *  Using a geospatial index can greatly improve the performance of
          queries that use this method.
PARAMETERS:
    geom_column:
        Required Argument.
        Specifies the other Geometry value.
        Types: str, ColumnExpression, GeometryType
SUPPORTED GEOMETRY TYPES:
    All Geometry types except geometry collections.
    Note:
        This method can be called on 3D geometries (those that include Z
        coordinates). However, the Z coordinate is ignored in method
        calculations.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
    Resultant column contains:
        * 1, if the geometry is within other geometry
        * 0, if the geometry is not within other geometry
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only few columns from GeoDataFrame.
    pols_lines = geodf.select(["skey", "polygons", "linestrings"])
    # Example 1: Check whether geometries in column 'linestrings' is spatially within
    #            the geometries in column 'polygons'.
    # Set the geometry property of GeoDataFrame to point to 'linestrings' column.
    pols_lines.geometry = pols_lines.linestrings
    pols_lines.within(geom_column=pols_lines.polygons)
    # Example 2: Check whether geometries in column 'linestrings' is spatially within
    #            the geometries specified by Polygon geometry type.
    # Create an object of Polygon GeometryType.
    po1 = Polygon([(0, 0), (0, 20), (20, 20), (20, 0), (0, 0)])
    # Pass the Polygon() GeometryType object as input to "geom_column" argument.
    pols_lines.within(geom_column=po1)
wkb_geom_to_sql
teradataml.geospatial.geodataframe.GeoDataFrame.wkb_geom_to_sql = wkb_geom_to_sql(self, column)
DESCRIPTION:
    Returns a specified Geometry value.
    Notes:
        *  Vantage provides this method because it is in the SQL/MM
          Spatial standard, where it is defined to implement transform
          functionality that allows importing the Geometry type to the server.
PARAMETERS:
    column:
        Required Argument.
        Specifies the geometry value specified in the well-known binary (WKB) format.
        The data type of "column" is BLOB. The maximum size of "column" is
        approximately 16 MB (the exact maximum is 16 MB - 1 KB), allowing
        for a representation of approximately one million points. For more
        information on WKB formats, see Well-Known Binary Format in SQL documentation.
        Types: str, ColumnExpression
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Get the WKB geometry in SQL format.
    # Let's select only few columns from GeoDataFrame.
    geodfbin = geodf.select(["skey", "points"]).assign(str_column=geodf.points.to_binary())
    geodfbin.wkb_geom_to_sql(column="str_column")
wkt_geom_to_sql
teradataml.geospatial.geodataframe.GeoDataFrame.wkt_geom_to_sql = wkt_geom_to_sql(self, column)
DESCRIPTION:
    Returns a specified Geometry value.
    Notes:
        *  Vantage provides this method because it is in the SQL/MM
          Spatial standard, where it is defined to implement transform
          functionality that allows importing the Geometry type to the server.
PARAMETERS:
    column:
        Required Argument.
        Specifies the geometry value specified in the well-known text (WKT) format.
        The data type of "column" is CLOB. The maximum size of "column" is
        approximately 16 MB (the exact maximum is 16 MB - 1 KB), allowing
        for a representation of approximately one million points. For more
        information on WKT formats, see Well-Known Text Format in SQL documentation.
        Types: str, ColumnExpression
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Get the WKT geometry in SQL format.
    # Let's select only few columns from GeoDataFrame.
    geodftxt = geodf.select(["skey", "points"]).assign(str_column=geodf.points.to_text())
    geodftxt.wkt_geom_to_sql(column="str_column")
Methods for Point Geometry
spherical_buffer
teradataml.geospatial.geodataframe.GeoDataFrame.spherical_buffer = spherical_buffer(self, distance, radius=6371000.0)
DESCRIPTION:
    Returns an MBR that represents the minimum and maximum latitude and
    longitude of all points within a given distance from a point. The earth
    is modeled as a sphere.
    Notes:
        *  spherical_buffer() and spheroidal_buffer() basically perform
          the same function, but with different performance and accuracy.
          spherical_buffer() models the earth as a simple sphere, so it
          performs better than spheroidal_buffer(), but with less accuracy.
          spheroidal_buffer() models the earth as a spheroid, so it returns
          a more accurate MBR, but it runs slower than spherical_buffer().
        *  The returned MBR cannot cross the longitude value of +/-180 or the
          latitude value of +/-90. The MBR cannot cover more than 180 degrees
          of latitude or longitude on the sphere or spheroid.
PARAMETERS:
    distance:
        Required Argument.
        Specifies the distance, in meters, from the point to
        calculate the MBR.
        Types: float, int, str, ColumnExpression
    radius:
        Optional Argument.
        Specifies the radius of the sphere used to model the earth.
        If omitted, the method uses a value of 6371000.0 meters.
        Default Value: 6371000.0
        Types: float, int, str, ColumnExpression
SUPPORTED GEOMETRY TYPES:
    ST_Point
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only few columns from GeoDataFrame.
    points = geodf.select(["skey", "points"])[geodf.skey.isin([1001, 1002, 1003])]
    # Example 1: Get an MBR that represents the minimum and maximum latitude and longitude of
    #            all points within a given distance of 2.8 from a ST_Point in columns 'points'
    #            with default values for other arguments.
    points.spherical_buffer(distance=2.8)
    # Example 2: Get an MBR that represents the minimum and maximum latitude and longitude of
    #            all points within a given distance of 2.8 and radius as 6371000 from a
    #            ST_Point in columns 'points' with default values for rest of the arguments.
    points.spherical_buffer(distance=2.8, radius=6371000)
spherical_distance
teradataml.geospatial.geodataframe.GeoDataFrame.spherical_distance = spherical_distance(self, geom_column)
DESCRIPTION:
    Returns the spherical distance between two spherical coordinates on the
    planet using the Haversine Formula. Both coordinates must be specified
    as ST_Point values.
PARAMETERS:
    geom_column:
        Required Argument.
        Specifies a Geometry for the other spherical coordinate.
        Types: str, ColumnExpression, GeometryType
SUPPORTED GEOMETRY TYPES:
    ST_Point
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only few columns from GeoDataFrame and join the GeoDataFrame to self.
    points = geodf.select(["skey", "points"])[geodf.skey.isin([1001, 1002, 1003])]
    points = points.join(points, how="cross", lsuffix="l", rsuffix="r")
    # Example 1: Get the spherical distance between two spherical coordinates on the planet using
    #            the Haversine Formula, where coordinates are in columns 'l_point' and 'r_point'.
    points.spherical_distance(points.r_points)
    # Example 2: Get the spherical distance between the spherical coordinates on the planet using
    #            the Haversine Formula, where set of points is in column "point" and another geometry
    #            defined by the Point object.
    # Create an object of Point GeometryType.
    p1 = Point(1,3)
    # Pass the Point() GeometryType object as input to "geom_column" argument.
    points.spherical_distance(geom_column=p1)
spheroidal_buffer
teradataml.geospatial.geodataframe.GeoDataFrame.spheroidal_buffer = spheroidal_buffer(self, distance, semimajor=6378137.0, invﬂattening=298.257223563)
DESCRIPTION:
    Returns an MBR that represents the minimum and maximum latitude and
    longitude of all points within a given distance from the point. The
    earth is modeled as a spheroid.
    Notes:
        *  For the MBR calculation, the planet is modeled as a spheroid. If you
          do not pass in values for the "semimajor" and "invflattening" arguments,
          the computation uses the semimajor axis and the inverse flattening
          ratio from the World Geodetic System, WGS84. A value of 6,378,137.0
          meters is used for the semimajor axis, and a value of 298.257223563
          is used for the inverse flattening ratio.
        *  spherical_buffer() and spheroidal_buffer() perform a similar
          function, but with different performance and accuracy.
          spherical_buffer() models the earth as a simple sphere, so it
          performs better than spheroidal_buffer(), but with less accuracy.
          spheroidal_buffer() models the earth as a spheroid, so it returns
          a more accurate MBR, but it runs slower than spherical_buffer().
        *  The returned MBR cannot cross the longitude value of +/-180 or the
          latitude value of +/-90. The MBR cannot cover more than 180 degrees
          of latitude or longitude on the sphere or spheroid.
PARAMETERS:
    distance:
        Required Argument.
        Specifies the distance, in meters, from the point to
        calculate the MBR.
        Types: float, int, str, ColumnExpression
    semimajor:
        Optional Argument.
        Specifies the length, in meters, of the semimajor axis of
        the spheroid. If omitted, the method uses the WGS84 value of
        6,378,137.0 meters.
        Default Value: 6378137.0
        Types: float, int
    invflattening:
        Optional Argument.
        Specifies the inverse flattening ratio of the spheroid. If
        omitted, the method uses the WGS84 value of 298.257223563.
        Default Value: 298.257223563
        Types: float, int
SUPPORTED GEOMETRY TYPES:
    ST_Point
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only few columns from GeoDataFrame.
    points = geodf.select(["skey", "points"])[geodf.skey.isin([1001, 1002, 1003])]
    # Example 1: Get an MBR that represents the minimum and maximum latitude and longitude of
    #            all points within a given distance of 2.8 from a ST_Point in columns 'points'
    #            with default values for other arguments.
    points.spheroidal_buffer(distance=2.8)
    # Example 2: Get an MBR that represents the minimum and maximum latitude and longitude of
    #            all points within a given distance of 2.8, semimajor as 637813 and invflattening
    #            as 298 from a ST_Point in columns 'points'.
    points.spheroidal_buffer(distance=2.8, semimajor=637813, invflattening=298)
spheroidal_distance
teradataml.geospatial.geodataframe.GeoDataFrame.spheroidal_distance = spheroidal_distance(self, geom_column, semimajor=6378137.0, invﬂattening=298.257223563)
DESCRIPTION:
    Returns the distance, in meters, between two spherical coordinates.
    Notes:
        *  For the distance calculation, the planet is modeled as a spheroid.
        *  If you do not pass in values for the "semimajor" and "invflattening"
          arguments, the computation uses the semimajor axis and the inverse
          flattening ratio from the World Geodetic System, WGS84. A value of
          6,378,137.0 meters is used for the semimajor axis, and a value of
          298.257223563 is used for the inverse flattening ratio.
        *  Distance calculations between antipodal or nearly antipodal points
          using the spheroidal_distance() method may fail to converge,
          generating a Vantage error. For these distance
          calculations use instead the spherical_distance() method.
PARAMETERS:
    geom_column:
        Required Argument.
        Specifies a Geometry for the other coordinate.
        Types: str, ColumnExpression, GeometryType
    semimajor:
        Optional Argument.
        Specifies the length in meters of the semimajor axis of the
        spheroid.
        Default Value: 6378137.0
        Types: float, int
    invflattening:
        Optional Argument.
        Specifies the inverse flattening ratio of the spheroid.
        Default Value: 298.257223563
        Types: float, int
SUPPORTED GEOMETRY TYPES:
    ST_Point
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only few columns from GeoDataFrame.
    points = geodf.select(["skey", "points"])[geodf.skey.isin([1001, 1002, 1003])]
    points = points.join(points, how="cross", lsuffix="l", rsuffix="r")
    # Example 1: Get the spheroidal distance between two points from columns 'l_points' and 'r_points'.
    points.spheroidal_distance(points.r_points)
    # Example 2: Get the spheroidal distance between points from column 'l_points' and
    #            a GeometryType object Point.
    # Create an object of Point GeometryType.
    p1 = Point(1,3)
    # Pass the Point() GeometryType object as input to "geom_column" argument.
    points.spheroidal_distance(geom_column=p1, semimajor=637813, invflattening=298)
set_x
teradataml.geospatial.geodataframe.GeoDataFrame.set_x = set_x(self, xcoord)
DESCRIPTION:
    Set the X coordinate of an ST_Point value.
PARAMETERS:
    xcoord:
        Required Argument.
        Specifies a value to the new X coordinate of the ST_Point value.
        Types: float, int, str, ColumnExpression
SUPPORTED GEOMETRY TYPES:
    ST_Point
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Set the X coordinate of the ST_Point geometries in column 'points'.
    # Let's select only few columns from GeoDataFrame.
    points = geodf.select(["skey", "points"])[geodf.skey.isin([1001, 1002, 1003])]
    points.set_x(xcoord=1000)
set_y
teradataml.geospatial.geodataframe.GeoDataFrame.set_y = set_y(self, ycoord)
DESCRIPTION:
    Set the Y coordinate of an ST_Point value.
PARAMETERS:
    ycoord:
        Required Argument.
        Specifies a value to the new Y coordinate of the ST_Point value.
        Types: float, int, str, ColumnExpression
SUPPORTED GEOMETRY TYPES:
    ST_Point
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Set the Y coordinate of the ST_Point geometries in column 'points'.
    # Let's select only few columns from GeoDataFrame.
    points = geodf.select(["skey", "points"])[geodf.skey.isin([1001, 1002, 1003])]
    points.set_y(ycoord=1000)
set_z
teradataml.geospatial.geodataframe.GeoDataFrame.set_z = set_z(self, zcoord)
DESCRIPTION:
    Set the Z coordinate of an ST_Point value.
    Note:
        *  This method can be used to convert a 2D point to a 3D point if a
          "zcoord" argument is passed.
PARAMETERS:
    zcoord:
        Required Argument.
        Specifies a value to the new Z coordinate of the ST_Point value.
        Types: float, int, str, ColumnExpression
SUPPORTED GEOMETRY TYPES:
    ST_Point
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Set the Z coordinate of the ST_Point geometries in column 'points'.
    # Let's select only few columns from GeoDataFrame.
    points = geodf.select(["skey", "points"])[geodf.skey.isin([1001, 1002, 1003])]
    points.set_z(zcoord=1000)
Methods for LineString Geometry
end_point
teradataml.geospatial.geodataframe.GeoDataFrame.end_point = end_point(self)
DESCRIPTION:
    Returns the end point of an ST_LineString or GeoSequence value.
    Note:
        *  This method returns a point having a Z coordinate if the input
          geometry is three-dimensional.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    ST_LineString and GeoSequence
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
    Resultant column contains a NULL, if the Geometry is an empty set.
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Get the end points of (ST_Linestring) geometries in column
    #            'linestrings'.
    # Process the GeoDataFrame to select only two columns, skey and linestrings and filter the
    # rows where geometry type is ST_Linestring.
    lines = geodf.select(["skey", "linestrings"])[geodf.linestrings.geom_type == "ST_Linestring"]
    lines.end_point()
length
teradataml.geospatial.geodataframe.GeoDataFrame.length = length(self)
DESCRIPTION:
    Returns the length measurement of a Geometry type that represents an
    ST_LineString, GeoSequence, or ST_MultiLineString value.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    ST_LineString, GeoSequence, and ST_MultiLineString
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Get the length of ST_Linestring/ST_MultiLinestring geometries
    #            in column 'linestrings'.
    # Let's select only few columns from GeoDataFrame.
    lines = geodf.select(["skey", "linestrings"])
    lines.length()
length_3D
teradataml.geospatial.geodataframe.GeoDataFrame.length_3D = length_3D(self)
DESCRIPTION:
    Returns the length of a 3D LineString or MultiLineString, taking into
    account the Z coordinates in the calculation.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    LineString and MultiLineString types that include Z coordinates
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
    Resultant column contains a 0, if the LineString or MultiLineString is an empty set.
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Get the length of 3D ST_Linestring/ST_MultiLinestring geometries
    #            in column 'linestrings'.
    # Let's select only few columns from GeoDataFrame.
    lines = geodf.select(["skey", "linestrings"])[geodf.linestrings.is_3D == 1]
    lines.length_3D()
line_interpolate_point
teradataml.geospatial.geodataframe.GeoDataFrame.line_interpolate_point = line_interpolate_point(self, proportion)
DESCRIPTION:
    Returns a point interpolated along a Geometry type that represents an
    ST_LineString or GeoSequence value, given a proportional distance along
    that line.
PARAMETERS:
    proportion:
        Required Argument.
        Specifies a value between 0 and 1 that represents the fraction of the
        total 2-dimensional length of the ST_LineString where the point must
        be located.
        Types: float, str, ColumnExpression
SUPPORTED GEOMETRY TYPES:
    ST_LineString and GeoSequence
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame with result column containing ST_Point Geometry values
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Get the point interpolated along the ST_Linestring geometries
    #            in column 'linestrings'.
    # Let's select only few columns from GeoDataFrame and filter rows which contains
    # only ST_Linestring geometries.
    lines = geodf.select(["skey", "linestrings"])[geodf.linestrings.geom_type == "ST_Linestring"]
    lines.line_interpolate_point(proportion=0.28)
num_points
teradataml.geospatial.geodataframe.GeoDataFrame.num_points = num_points(self)
DESCRIPTION:
    Returns the number of points in a Geometry type that represents an
    ST_LineString or GeoSequence value.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    ST_LineString and GeoSequence
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Get the number of points in a geometry in column 'linestrings'.
    # Let's select only few columns from GeoDataFrame and rows containing ST_Linestrings geometries only.
    lines = geodf.select(["skey", "linestrings"])[geodf.linestrings.geom_type == "ST_Linestring"]
    lines.num_points()
point
teradataml.geospatial.geodataframe.GeoDataFrame.point = point(self, position)
DESCRIPTION:
    Returns the specified point from a Geometry type that represents an
    ST_LineString or GeoSequence value.
    Note:
        *  This method returns a point having a Z coordinate if the input
          geometry is three-dimensional.
PARAMETERS:
    position:
        Required Argument.
        Specifies a value for the requested point, where the position of the
        first point is 1.
        Types: int, str, ColumnExpression
SUPPORTED GEOMETRY TYPES:
    ST_LineString and GeoSequence
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame with result column containing Geometry values
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only few columns from GeoDataFrame, also filter rows containing
    # only ST_Linestring geometries.
    lines = geodf.select(["skey", "linestrings"])[geodf.linestrings.geom_type == "ST_Linestring"]
    # Example 1: Get the point specified at 1st position in ST_LineString geometries in column 'linestrings'.
    lines.point(1)
    # Example 2: Get the point specified at 2nd position in ST_LineString geometries in column 'linestrings'.
    lines.point(position=2)
start_point
teradataml.geospatial.geodataframe.GeoDataFrame.start_point = start_point(self)
DESCRIPTION:
    Returns the start point of an ST_LineString or GeoSequence value.
    Note:
        *  This method returns a point having a Z coordinate if the input
          geometry is three-dimensional.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    ST_LineString and GeoSequence
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
    Resultant column contains a NULL, if the Geometry is an empty set.
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Get the start points of (ST_Linestring) geometries in column
    #            'linestrings'.
    # Process the GeoDataFrame to select only two columns, skey and linestrings and filter the
    # rows where geometry type is ST_Linestring.
    lines = geodf.select(["skey", "linestrings"])[geodf.linestrings.geom_type == "ST_Linestring"]
    lines.start_point()
Methods for Polygon Geometry
interiors
teradataml.geospatial.geodataframe.GeoDataFrame.interiors = interiors(self, position)
DESCRIPTION:
    Returns the specified interior ring of a Geometry type that represents
    an ST_Polygon value.
PARAMETERS:
    position:
        Required Argument.
        Specifies a value for the requested interior ring, where the
        position of the first interior ring is 1.
        Types: int, str, ColumnExpression
SUPPORTED GEOMETRY TYPES:
    ST_Polygon
    Note:
        This method can be called on 3D geometries (those that include Z
        coordinates). However, the Z coordinate is ignored in method
        calculations. Consequently, any Z coordinates returned by this
        method should be ignored. Teradata recommends using the make_2D()
        method to strip out the Z coordinates of the return value.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame with result column containing ST_LineString Geometry values
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Get the interior ring specified at 1st index for polygons.
    # Filter the data to select few rows and couple of columns (skey and polygons).
    polygons = geodf.select(["skey", "polygons"])[geodf.skey.isin([1002, 1004])]
    polygons.interiors(position=1)
num_interior_ring
teradataml.geospatial.geodataframe.GeoDataFrame.num_interior_ring = num_interior_ring(self)
DESCRIPTION:
    Returns the number of interior rings of a Geometry type that represents
    an ST_Polygon value.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    ST_Polygon
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Get the number of interior rings of ST_Polygon geometries in column 'polygons'.
    # Let's select only few columns from GeoDataFrame and rows containing ST_Polygon geometries only.
    pols = geodf.select(["skey", "polygons"])[geodf.polygons.geom_type == "ST_Polygon"]
    pols.num_interior_ring()
point_on_surface
teradataml.geospatial.geodataframe.GeoDataFrame.point_on_surface = point_on_surface(self)
DESCRIPTION:
    Returns a point that is guaranteed to spatially intersect an ST_Polygon,
    or at least one of the component polygons of an ST_MultiPolygon.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    ST_Polygon and ST_MultiPolygon
    Note:
        This method can be called on 3D geometries (those that include Z
        coordinates). However, the Z coordinate is dropped, and only x and y
        coordinates are returned.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame with result column containing Geometry values
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Get the point in polygon geometries that is guaranteed to spatially intersect.
    # Let's select only few columns from GeoDataFrame.
    pols = geodf.select(["skey", "polygons"])[geodf.skey.isin([1001, 1002, 1003])]
    pols.point_on_surface()
Methods for GeometryCollection Geometry
geom_component
teradataml.geospatial.geodataframe.GeoDataFrame.geom_component = geom_component(self, position)
DESCRIPTION:
    Returns the geometry of one component member of a composite geometry
    type (ST_GeomCollection, ST_MultiPoint, ST_MultiLineString, or
    ST_MultiPolygon). The element to be returned is specified by position
    with the collection.
PARAMETERS:
    position:
        Required Argument.
        Specifies a value representing the position in the collection of the
        geometry to be returned. The position of the first element in the
        collection is number 1.
        Types: int, str, ColumnExpression
SUPPORTED GEOMETRY TYPES:
    ST_GeomCollection, ST_MultiPoint, ST_MultiLineString, and ST_MultiPolygon
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame with result column containing Geometry values
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Get the geometry component at position 1 in ST_GeometryCollection, a composite geometry type,
    #            geometries in column 'geom_collections'.
    geom_collections = geodf.select(["skey", "geom_collections"])
    geom_collections.geom_component(position=1)
num_geometry
teradataml.geospatial.geodataframe.GeoDataFrame.num_geometry = num_geometry(self)
DESCRIPTION:
    Returns the number of distinct geometries that constitute a composite
    geometry type (ST_GeomCollection, ST_MultiPoint, ST_MultiLineString, or
    ST_MultiPolygon).
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    ST_GeomCollection, ST_MultiPoint, ST_MultiLineString, and ST_MultiPolygon
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Get the number of distinct geometries that constitute the composite geometry type
    #            ST_GeometryCollection in column 'geom_collections'.
    # Let's select only few columns from GeoDataFrame.
    geom_collections = geodf.select(["skey", "geom_collections"])[geodf.skey.isin([1001, 1004, 1005])]
    geom_collections.num_geometry()
Methods for GeoSequence Geometry
clip
teradataml.geospatial.geodataframe.GeoDataFrame.clip = clip(self, start_timestamp, end_timestamp)
DESCRIPTION:
    Returns a GeoSequence type containing the subset of points and
    associated data that lie between the two timestamp input arguments.
PARAMETERS:
    start_timestamp:
        Required Argument.
        Specifies a TIMESTAMP value as the beginning timestamp.
        Types: str
    end_timestamp:
        Required Argument.
        Specifies a TIMESTAMP value as the ending timestamp.
        Types: str
SUPPORTED GEOMETRY TYPES:
    GeoSequence
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only skey and geosequence columns from GeoDataFrame, as clip() function works
    # with only ST_Geosequence geometry type.
    geoseq = geodf.select(["skey", "geosequence"])
    # Example 1: clip the geometries of type ST_Geosequence in column 'geosequence', within the
    #            timestamp "TIMESTAMP '2007-03-14 01:35:00'" and "TIMESTAMP '2007-03-14 01:35:08'".
    geoseq.clip(start_timestamp="TIMESTAMP '2007-03-14 01:35:00'",
    end_timestamp="TIMESTAMP '2007-03-14 01:35:08'")
get_ﬁnal_timestamp
teradataml.geospatial.geodataframe.GeoDataFrame.get_ﬁnal_timestamp = get_ﬁnal_timestamp(self)
DESCRIPTION:
    Returns the TimeStamp of the last point of a GeoSequence.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    GeoSequence
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Get the timestamp of the last point of a Geosequence geometry in 'geosequence' column.
    # Select 'skey' and 'geosequence' column and filter the rows where geometries are not None.
    geoseq = geodf.select(["skey", "geosequence"])[geodf.geosequence != None]
    geoseq.get_final_timestamp()
get_link
teradataml.geospatial.geodataframe.GeoDataFrame.get_link = get_link(self, index)
DESCRIPTION:
    Get the link ID of a specified point in a GeoSequence.
PARAMETERS:
    index:
        Required Argument.
        Specifies the index of the point in the GeoSequence, where the index of the
        first point is 1.
        Types: int, str, ColumnExpression
SUPPORTED GEOMETRY TYPES:
    GeoSequence
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Select 'skey' and 'geosequence' column and filter the rows where geometries are not None.
    geoseq = geodf.select(["skey", "geosequence"])[geodf.geosequence != None]
    # Example 1: Get the link ID of the point at 1st position in a Geosequence geometry in 'geosequence' column.
    geoseq.get_link(1)
    # Example 2: Get the link ID of the point at 2nd position in a Geosequence geometry in 'geosequence' column.
    geoseq.get_link(index=2)
get_user_ﬁeld
teradataml.geospatial.geodataframe.GeoDataFrame.get_user_ﬁeld = get_user_ﬁeld(self, ﬁeld_index, index)
DESCRIPTION:
    Returns the user field specified by "field_index" for the point specified by
    "index" for a GeoSequence type.
PARAMETERS:
    field_index:
        Required Argument.
        Specifies the index of the user field within the point, where the index of the
        first user field is 1.
        Types: int, str, ColumnExpression
    index:
        Required Argument.
        Specifies the index of the point within the GeoSequence type, where the index
        of the first point is 1.
        Types: int, str, ColumnExpression
SUPPORTED GEOMETRY TYPES:
    GeoSequence
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Select 'skey' and 'geosequence' column and filter the rows where geometries are not None.
    geoseq = geodf.select(["skey", "geosequence"])[geodf.geosequence != None]
    # Example 1: Get the 1st user field with respect to the point at index 1 in a Geosequence
    #            geometry in 'geosequence' column.
    geoseq.get_user_field(1, 1)
    # Example 2: Get the 2nd user field with respect to the point at index 1 in a Geosequence
    #            geometry in 'geosequence' column.
    geoseq.get_user_field(field_index=2, index=1)
get_user_ﬁeld_count
teradataml.geospatial.geodataframe.GeoDataFrame.get_user_ﬁeld_count = get_user_ﬁeld_count(self)
DESCRIPTION:
    Returns the number of user fields associated with each point of a
    GeoSequence.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    GeoSequence
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Get the number of user fields associated to each point in a Geosequence
    #            geometry in 'geosequence' column.
    # Select 'skey' and 'geosequence' column and filter the rows where geometries are not None.
    geoseq = geodf.select(["skey", "geosequence"])[geodf.geosequence != None]
    geoseq.get_user_field_count()
point_heading
teradataml.geospatial.geodataframe.GeoDataFrame.point_heading = point_heading(self, index)
DESCRIPTION:
    Returns the heading for the specified point of a GeoSequence.
    The value is calculated as the angle (in degrees) between a vertical
    line and the line segment from the specified point to the next point
    clockwise from the North.
PARAMETERS:
    index:
        Required Argument.
        Specifies the index of the point to return the heading of, where the index of
        the first point in the GeoSequence is 1.
        Types: int, str, ColumnExpression
SUPPORTED GEOMETRY TYPES:
    GeoSequence
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only few columns from GeoDataFrame.
    geoseq = geodf.select(["skey", "geosequence"])[geodf.skey.isin([1001, 1002])]
    # Example 1: Get the heading for the point at 1st index in Geosequence geometries in
    #            "geosequence" column.
    geoseq.point_heading(1)
    # Example 2: Get the heading for the point at 2nd index in Geosequence geometries in
    #            "geosequence" column.
    geoseq.point_heading(index=2)
set_link
teradataml.geospatial.geodataframe.GeoDataFrame.set_link = set_link(self, index, link_id)
DESCRIPTION:
    Set the link ID of a specified point in a GeoSequence.
PARAMETERS:
    index:
        Required Argument.
        Specifies the index of the point in the GeoSequence, where the index of the
        first point is 1.
        Types: int, str, ColumnExpression
    link_id:
        Required Argument.
        Specifies the new link ID of the specified point in the GeoSequence. The data type
        of linkID is DECIMAL(18,0).
        Types: float
SUPPORTED GEOMETRY TYPES:
    GeoSequence
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Set the link ID for the point at the 1st index in geometries in column 'geosequence'.
    # Let's select only few columns from GeoDataFrame.
    geoseq = geodf.select(["skey", "geosequence"])[geodf.skey.isin([1001, 1002])]
    geoseq.set_link(index=1, link_id=20.2)
speed
teradataml.geospatial.geodataframe.GeoDataFrame.speed = speed(self, index=None, begin_index=None, end_index=None)
DESCRIPTION:
    Returns the approximate speed at a specified point (speed(index
    INTEGER)) or between two points (speed(iBegin INTEGER, iEnd INTEGER))
    for a GeoSequence type.
    Notes:
        * If "begin_index" and "end_index" arguments are passed, then
          the speed is calculated as the distance between the two points
          (along the LineString) divided by the time between them.
        * If "index" is passed, then if the point is:
            * The first point, the distance between the first and second
              points is divided by the time between them.
            * The last point, the distance between the second to the last
              and last points is divided by the time between them.
            * Any other point, the distance between the previous point
              and the next point (along the LineString) is divided by the
              time difference between the previous point and the next point.
        * The units used for distance are the same units that are used by
          the coordinate system for the GeoSequence value, and the time
          units are in hours.
PARAMETERS:
    index:
        Optional Argument.
        Specifies the index of the point within the GeoSequence type,
        where the index of the first point is 1.
        Default Value: None
        Types: int, str, ColumnExpression
    begin_index:
        Optional Argument.
        Specifies the index of the starting point for which to
        calculate the speed, where the index of the first point in a
        GeoSequence type is 1.
        Default Value: None
        Types: int, str, ColumnExpression
    end_index:
        Optional Argument.
        Specifies the index of the ending point for which to calculate
        the speed, where the index of the first point in a GeoSequence is 1.
        Default Value: None
        Types: int, str, ColumnExpression
SUPPORTED GEOMETRY TYPES:
    GeoSequence
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only few columns from GeoDataFrame.
    geoseq = geodf.select(["skey", "geosequence"])[geodf.skey.isin([1001, 1002])]
    # Example 1: Get the approximate speed at a point specified at 1st index for Geosequence geometries.
    geoseq.speed(index=1)
    # Example 2: Get the approximate speed between points at 1st and 2nd index for Geosequence geometries.
    geoseq.speed(begin_index=1, end_index=2)
Filtering Functions and Methods
intersects_mbb
teradataml.geospatial.geodataframe.GeoDataFrame.intersects_mbb = intersects_mbb(self, geom_column)
DESCRIPTION:
    Tests whether a 3D geometry spatially intersects a specified MBB.
PARAMETERS:
    geom_column:
        Required Argument.
        Specifies a minimum bounding box.
        Types: str, ColumnExpression, GeometryType
SUPPORTED GEOMETRY TYPES:
    3D Point, 3D MultiPoint, 3D LineString, 3D MultiLineString
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
    Resultant column contains:
        * 1, if the input 3D geometry intersects the MBB argument.
        * 0, if the input 3D geometry does not intersect the MBB argument.
        * Returns an error if the input geometry is not 3D (does not have
          a z coordinate).
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Check whether the 3D ST_Linestring/ST_MultiLinestring geometries
    #            in column 'linestrings' intersects with MBB or not.
    # First, let's process the GeoDataFrame to select few columns and filter only 3D geometries
    # in column 'points', then execute GeoDataFrame.mbb() function to get MBB representation of
    # ST_Point/ST_MultiPoint geometries.
    geodfmbb = geodf.select(["skey", "points", "linestrings"])[geodf.points.is_3D == 1].mbb()
    # Set the geometry property of GeoDataFrame to point to 'linestrings' column.
    geodfmbb.geometry = geodfmbb.linestrings
    geodfmbb.intersects_mbb(geom_column=geodfmbb.mbb_points_geom)
mbb_ﬁlter
teradataml.geospatial.geodataframe.GeoDataFrame.mbb_ﬁlter = mbb_ﬁlter(self, geom_column)
DESCRIPTION:
    Tests whether the MBBs of two 3D geometries spatially intersect.
PARAMETERS:
    geom_column:
        Required Argument.
        Specifies a 3D Geometry.
        Types: str, ColumnExpression, GeometryType
SUPPORTED GEOMETRY TYPES:
    All 3D Geometry types.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
    Resultant column contains:
        * 1, if the MBB of the input 3D geometry intersects the MBB
          of the geometry passed to the method.
        * 0, if the MBB of the input 3D geometry does not intersect
          the MBB of the geometry passed to the method.
        * Returns an error if either geometry is not 3D (does not
          have a z coordinate).
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only few columns from GeoDataFrame.
    points_lines = geodf[geodf.skey.isin([1004, 1005, 1006])].select(["skey", "points", "linestrings"])
    # Example 1: Check whether MBBs of geometries in column 'points' and 'lienstrings'
    #            spatially intersect or not.
    points_lines.mbb_filter(points_lines.linestrings)
    # Example 2: Check whether MBBs of geometries in column 'linestrings' and GeometryType object
    #            Linestring spatially intersect or not.
    # Set the geometry property of GeoDataFrame to point to 'linestrings' column.
    points_lines.geometry = points_lines.linestrings
    # Create an object of Linestring GeometryType.
    l1 = LineString([(1,3,6), (3,0,6), (6,0,1)])
    # Pass the LineString() GeometryType object as input to "geom_column" argument.
    points_lines.mbb_filter(l1)
mbr_ﬁlter
teradataml.geospatial.geodataframe.GeoDataFrame.mbr_ﬁlter = mbr_ﬁlter(self, geom_column)
DESCRIPTION:
    Tests whether the MBRs of two 2D geometries spatially intersect.
PARAMETERS:
    geom_column:
        Required Argument.
        Specifies a 2D Geometry.
        Types: str, ColumnExpression, GeometryType
SUPPORTED GEOMETRY TYPES:
    All 2D Geometry types.
    Although mbr_filter() will accept 3D geometries, the Z coordinates
    are ignored in the calculations.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
    Resultant column contains:
        * 1, if the MBR of the input geometry intersects the
          MBR of the geometry passed to the method.
        * 0, if the MBR of the input geometry does not intersect
          the MBR of the geometry passed to the method.
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only few columns from GeoDataFrame.
    points_lines = geodf[geodf.skey.isin([1004, 1005, 1006])].select(["skey", "points", "linestrings"])
    # Example 1: Check whether MBRs of geometries in column 'points' and 'lienstrings'
    #            spatially intersect or not.
    points_lines.mbr_filter(points_lines.linestrings)
    # Example 2: Check whether MBRs of geometries in column 'linestrings' and GeometryType object
    #            Linestring spatially intersect or not.
    # Set the geometry property of GeoDataFrame to point to 'linestrings' column.
    points_lines.geometry = points_lines.linestrings
    # Create an object of Linestring GeometryType.
    l1 = LineString([(1,3,6), (3,0,6), (6,0,1)])
    # Pass the LineString() GeometryType object as input to "geom_column" argument.
    points_lines.mbr_filter(l1)
within_mbb
teradataml.geospatial.geodataframe.GeoDataFrame.within_mbb = within_mbb(self, geom_column)
DESCRIPTION:
    Tests whether a 3D geometry is spatially within the bounds of a
    specified MBB.
PARAMETERS:
    geom_column:
        Required Argument.
        Specifies a minimum bounding box.
        Types: str, ColumnExpression, GeometryType
SUPPORTED GEOMETRY TYPES:
    All 3D Geometry types.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrame
    Resultant column contains:
        * 1, if the input 3D geometry is spatially within MBB argument.
        * 0, if the input 3D geometry is not spatially within the MBB
          argument.
        * Returns an error if the input geometry is not 3D (does not
          have a z coordinate).
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Check whether the geometries in column 'points' is withing the
    #            specified MBB geometries or not.
    # Let's select only few columns from GeoDataFrame and filter only 3D rows.
    geodfmbb = geodf.select(["skey", "points"]).mbb()[geodf.points.is_3D == 1]
    # Join the GeoDataFrame with self.
    geodfmbb = geodfmbb.join(geodfmbb, how="cross", lsuffix="l", rsuffix="r")
    geodfmbb.within_mbb(geom_column=geodfmbb.r_mbb_points_geom)
teradataml GeoDataFrameColumn
Geospatial Specific Properties
Properties for All Types of Geometries
boundary
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.boundary
DESCRIPTION:
    Returns the boundary of the Geometry value.
    Notes:
        The boundary of a Geometry value is a set of Geometry values of the
        next lower dimension.
        +=====================================+=====================================+
        | IF the Geometry represents ...      | THEN the boundary ...               |
        +=====================================+=====================================+
        | an ST_Point or ST_MulitPoint value  | is the empty set.                   |
        +-------------------------------------+-------------------------------------+
        | a nonclosed ST_LineString or        | consists of the start and end       |
        | GeoSequence value                   | ST_Point values.                    |
        +-------------------------------------+-------------------------------------+
        | a closed ST_LineString or           | is empty.                           |
        | GeoSequence value                   |                                     |
        +-------------------------------------+-------------------------------------+
        | an ST_MultiLineString value         | that are in the boundaries of an    |
        | consists of ST_Point values         | odd number of its element           |
        |                                     | ST_LineString values.               |
        +-------------------------------------+-------------------------------------+
        | an ST_Polygon value                 | consists of its set of linear rings |
        +-------------------------------------+-------------------------------------+
        | an ST_MultiPolygon value            | consists of the set of linear rings |
        |                                     |  of its ST_Polygon values.          |
        +-------------------------------------+-------------------------------------+
        | an ST_GeomCollection where the      | consists of geometry values drawn   |
        | interiors of the geometries in the  | from the boundaries of the element  |
        | collection are disjoint             | geometries, where the geometry      |
        |                                     | values are in the boundaries of an  |
        |                                     | odd number of element geometries.   |
        +=====================================+=====================================+
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
    Note:
        This method can be called on 3D geometries (those that include Z
        coordinates). However, the Z coordinate is ignored in method
        calculations. Consequently, any Z coordinates returned by this
        method should be ignored. Teradata recommends using the make_2D()
        method to strip out the Z coordinates of the return value.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn containing Geometry values
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    gdf = GeoDataFrame("sample_shapes")
    gdf = gdf.select(["skey", "polygons", "linestrings"])
    gdf
    # Example 1: Get the boundary of all Polygon geometries in column 'polygons'.
    gdf.assign(res=gdf.polygons.boundary)
    # Example 2: Filter the rows where boundary of geometry (polygon) is same as that of the Linestring Geometry type object.
    # Create Linestring Geometry objects.
    l1 = LineString([(0, 0), (0, 20), (20, 20), (20, 0), (0, 0)])
    l2 = LineString([(0,0,0), (0,0,20), (0,20,0), (0,20,20), (20,0,0), (20,0,20), (20,20,0), (20,20,20), (0,0,0)])
    # Execute the function using slice filtering.
    gdf[(gdf.polygons.boundary == l1) | (gdf.polygons.boundary == l2)]
centroid
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.centroid
DESCRIPTION:
    Returns the mathematical centroid of an ST_Polygon or ST_MultiPolygon
    value.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    ST_Polygon or ST_MultiPolygon.
    Note:
        This method can be called on 3D geometries (those that include Z
        coordinates). However, the Z coordinate is dropped, and only x and y
        coordinates are returned.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn containing ST_Point Geometry values
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    gdf = GeoDataFrame("sample_shapes")
    gdf = gdf.select(["skey", "polygons", "linestrings"])[gdf.skey.isin([1001, 1002, 1003])]
    gdf
    # Example 1: Get the centroid of all Polygon geometries in column 'polygons'.
    gdf.assign(res=gdf.polygons.centroid)
convex_hull
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.convex_hull
DESCRIPTION:
    Returns the convex hull of the Geometry value.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
    Note:
        This method can be called on 3D geometries (those that include Z
        coordinates). However, the Z coordinate is ignored in method
        calculations. Consequently, any Z coordinates returned by this
        method should be ignored. Teradata recommends using the make_2D()
        method to strip out the Z coordinates of the return value.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn containing Geometry values
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    gdf = GeoDataFrame("sample_shapes")
    gdf = gdf.select(["skey", "polygons", "linestrings"])
    gdf
    # Example 1: Get the convex hull of all geometries in column 'polygons'.
    gdf.assign(res=gdf.polygons.convex_hull)
    # Example 2: Filter the rows where centroid of geometry (polygon) is same as that of the Polygon Geometry type object.
    # Create Polygon Geometry objects.
    po1 = Polygon([(0, 0), (0, 20), (20, 20), (20, 0), (0, 0)])
    # Execute the function using slice filtering.
    gdf[gdf.polygons.convex_hull == po1]
coord_dim
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.coord_dim
DESCRIPTION:
    Returns the coordinate dimension of a geometry.
    Note:
        *  The coordinate dimension is the same as the coordinate dimension of
          the spatial reference system for the Geometry value.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
    Resultant column contains:
        * 1, if the input geometry is 1D
        * 2, if the input geometry is 2D
        * 3, if the input geometry is 3D
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    gdf = GeoDataFrame("sample_shapes")
    gdf = gdf.select(["skey", "polygons", "linestrings"])
    gdf
    # Example 1: Get the coordinate dimension of all geometries in column 'polygons'.
    gdf.assign(res=gdf.polygons.coord_dim)
    # Example 2: Filter the rows where coordinate dimension of geometry (polygon) is equal to 3.
    gdf[gdf.polygons.coord_dim == 3]
dimension
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.dimension
DESCRIPTION:
    Returns the dimension of the Geometry type.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
    Resultant column contains:
        * 0, for a 0-dimensional geometry
        * 1, for a 1D geometry
        * 2, for a 2D geometry
        * -1, if the input geometry is empty
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    gdf = GeoDataFrame("sample_shapes")
    gdf = gdf.select(["skey", "polygons", "linestrings"])
    gdf
    # Example 1: Get the dimension of all geometries in column 'polygons'.
    gdf.assign(res=gdf.polygons.dimension)
    # Example 2: Filter the rows where dimension of geometry (polygon) is equal to 2.
    gdf[gdf.polygons.dimension == 2]
geom_type
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.geom_type
DESCRIPTION:
    Returns the Geometry type of the Geometry value.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
    Resultant column contains any of the following strings:
        * 'ST_Point'
        * 'ST_LineString'
        * 'ST_Polygon'
        * 'ST_MultiPoint'
        * 'ST_MultiLineString'
        * 'ST_MultiPolygon'
        * 'ST_GeomCollection'
        * 'GeoSequence'
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    gdf = GeoDataFrame("sample_shapes")
    gdf = gdf.select(["skey", "polygons", "linestrings"])
    gdf
    # Example 1: Get the geometry type of all geometries in column 'polygons'.
    gdf.assign(res=gdf.polygons.geom_type)
    # Example 2: Filter the rows where geometry type is 'ST_MultiPolygon'.
    gdf[gdf.polygons.geom_type == "ST_MultiPolygon"]
is_3D
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.is_3D
DESCRIPTION:
    Tests if a Geometry value has Z coordinate value.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
    Resultant column contains:
        * 1, if the Geometry contains Z coordinates
        * 0, if the Geometry does not contain Z coordinates
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    gdf = GeoDataFrame("sample_shapes")
    gdf = gdf.select(["skey", "polygons", "linestrings"])
    gdf
    # Example 1: Check if geometries in column 'polygons' is 3D or not.
    gdf.assign(res=gdf.polygons.is_3D)
    # Example 2: Filter the rows where geometry in column 'polygon' is 3D.
    gdf[gdf.polygons.is_3D == 1]
is_empty
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.is_empty
DESCRIPTION:
    Tests if a Geometry value corresponds to the empty set.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
    Resultant column contains:
        * 1, if the geometry is empty
        * 0, if the geometry is not empty
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    gdf = GeoDataFrame("sample_shapes")
    gdf = gdf.select(["skey", "polygons", "linestrings"])
    gdf
    # Example 1: Check if geometries in column 'polygons' is empty or not.
    gdf.assign(res=gdf.polygons.is_empty)
    # Example 2: Filter the rows where geometry in column 'polygons' is empty or not.
    gdf[gdf.polygons.is_empty == 1]
is_simple
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.is_simple
DESCRIPTION:
    Tests if a Geometry value has no anomalous geometric points, such as
    self intersection tangency.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
    Resultant column contains:
        * 1, if the geometry is simple, with no anomalous points
        * 0, if the geometry is not simple
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    gdf = GeoDataFrame("sample_shapes")
    gdf = gdf.select(["skey", "polygons", "linestrings"])
    gdf
    # Example 1: Check if geometries in column 'polygons' is simple or not.
    gdf.assign(res=gdf.polygons.is_simple)
    # Example 2: Filter the rows where geometry in column 'polygons' is not simple.
    gdf[gdf.polygons.is_simple == 0]
is_valid
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.is_valid
DESCRIPTION:
    Tests if a Geometry value is well-formed.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
    Note:
        This method can be called on 3D geometries (those that include Z
        coordinates). However, the Z coordinate is ignored in method
        calculations.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
    Resultant column contains:
        * 1, if the geometry is valid
        * 0, if the geometry is not valid
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    gdf = GeoDataFrame("sample_shapes")
    gdf = gdf.select(["skey", "polygons", "linestrings"])
    gdf
    # Example 1: Check if geometries in column 'polygons' is valid or not.
    gdf.assign(res=gdf.polygons.is_valid)
    # Example 2: Filter the rows where geometry in column 'polygons' is valid.
    gdf[gdf.polygons.is_valid == 1]
max_x
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.max_x
DESCRIPTION:
    Returns the maximum X coordinate of a Geometry value.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
    Resultant column contains a NULL, if the Geometry is an empty set.
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    gdf = GeoDataFrame("sample_shapes")
    gdf = gdf.select(["skey", "polygons", "linestrings"])
    gdf
    # Example 1: Get the maximum X coordinate of all Polygon geometries in column 'polygons'.
    gdf.assign(res=gdf.polygons.max_x)
    # Example 2: Filter the rows where maximum X coordinate value of geometry in column 'polygons' is greater than 20.
    gdf[gdf.polygons.max_x > 20]
max_y
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.max_y
DESCRIPTION:
    Returns the maximum Y coordinate of a Geometry value.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
    Resultant column contains a NULL, if the Geometry is an empty set.
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    gdf = GeoDataFrame("sample_shapes")
    gdf = gdf.select(["skey", "polygons", "linestrings"])
    gdf
    # Example 1: Get the maximum Y coordinate of all Polygon geometries in column 'polygons'.
    gdf.assign(res=gdf.polygons.max_y)
    # Example 2: Filter the rows where maximum Y coordinate value of geometry in column 'polygons' is greater than 20.
    gdf[gdf.polygons.max_y > 20]
max_z
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.max_z
DESCRIPTION:
    Returns the maximum Z coordinate of a Geometry value.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
    Resultant column contains a NULL, if the Geometry is an empty set.
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    gdf = GeoDataFrame("sample_shapes")
    gdf = gdf.select(["skey", "polygons", "linestrings"])
    gdf
    # Example 1: Get the maximum Z coordinate of all Polygon geometries in column 'polygons'.
    gdf.assign(res=gdf.polygons.max_z)
    # Example 2: Filter the rows where maximum Z coordinate value of geometry in column 'polygons' is greater than 20.
    gdf[gdf.polygons.max_z > 20]
min_x
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.min_x
DESCRIPTION:
    Returns the minimum X coordinate of a Geometry value.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
    Resultant column contains a NULL, if the Geometry is an empty set.
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    gdf = GeoDataFrame("sample_shapes")
    gdf = gdf.select(["skey", "polygons", "linestrings"])
    gdf
    # Example 1: Get the minimum X coordinate of all Polygon geometries in column 'polygons'.
    gdf.assign(res=gdf.polygons.min_x)
    # Example 2: Filter the rows where minimum X coordinate value of geometry in column 'polygons' is greater than 0.5
    gdf[gdf.polygons.min_x > 0.5]
min_y
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.min_y
DESCRIPTION:
    Returns the minimum Y coordinate of a Geometry value.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
    Resultant column contains a NULL, if the Geometry is an empty set.
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    gdf = GeoDataFrame("sample_shapes")
    gdf = gdf.select(["skey", "polygons", "linestrings"])
    gdf
    # Example 1: Get the minimum Y coordinate of all Polygon geometries in column 'polygons'.
    gdf.assign(res=gdf.polygons.min_y)
    # Example 2: Filter the rows where minimum Y coordinate value of geometry in column 'polygons' is greater than 0.5
    gdf[gdf.polygons.min_y > 0.5]
min_z
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.min_z
DESCRIPTION:
    Returns the minimum Z coordinate of a Geometry value.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
    Resultant column contains a NULL, if the Geometry is an empty set.
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    gdf = GeoDataFrame("sample_shapes")
    gdf = gdf.select(["skey", "polygons", "linestrings"])
    gdf
    # Example 1: Get the minimum Z coordinate of all Polygon geometries in column 'polygons'.
    gdf.assign(res=gdf.polygons.min_z)
    # Example 2: Filter the rows where minimum Z coordinate value of geometry in column 'polygons' is not None.
    gdf[gdf.polygons.min_z != None]
srid
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.srid
DESCRIPTION:
    Get the spatial reference system identifier of the Geometry value.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    gdf = GeoDataFrame("sample_shapes")
    gdf = gdf.select(["skey", "polygons", "linestrings"])
    gdf
    # Example 1: Get the spatial reference system identifier of all geometries in column 'polygons'.
    gdf.assign(res=gdf.polygons.srid)
    # Example 2: Filter the rows where spatial reference system identifier of polygon is 0.
    gdf[gdf.polygons.srid == 0]
Properties for Point Geometry
x
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.x
DESCRIPTION:
    Get the X coordinate of an ST_Point value.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    ST_Point
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    gdf = GeoDataFrame("sample_shapes")
    gdf = gdf.select(["skey", "points", "linestrings"])[gdf.points.geom_type == "ST_Point"]
    gdf
    # Example 1: Get the X coordinate of all geometries in column "points".
    gdf.assign(res=gdf.points.x)
y
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.y
DESCRIPTION:
    Get the Y coordinate of an ST_Point value.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    ST_Point
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    gdf = GeoDataFrame("sample_shapes")
    gdf = gdf.select(["skey", "points", "linestrings"])[gdf.points.geom_type == "ST_Point"]
    gdf
    # Example 1: Get the Y coordinate of all geometries in column "points".
    gdf.assign(res=gdf.points.y)
z
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.z
DESCRIPTION:
    Get the Z coordinate of an ST_Point value.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    ST_Point
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    gdf = GeoDataFrame("sample_shapes")
    gdf = gdf.select(["skey", "points", "linestrings"])[(gdf.points.is_3D == 1) &
    (gdf.points.geom_type == "ST_Point")]
    gdf
    # Example 1: Get the Z coordinate of all geometries in column "points".
    gdf.assign(res=gdf.points.z)
Properties for LineString Geometry
is_closed_3D
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.is_closed_3D
DESCRIPTION:
    Tests whether a 3D LineString or 3D MultiLineString is closed, taking
    into account the Z coordinates in the calculation.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    LineString and MultiLineString types that include Z coordinates
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
    Resultant column contains:
        * 1, if the 3D LineString or 3D MultiLineString is closed.
        * 0, if the 3D LineString or 3D MultiLineString is not closed or is empty.
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    gdf = GeoDataFrame("sample_shapes")
    # Select few columns and filter rows containing only 3D geometries in 'linestrings' column.
    gdf = gdf.select(["skey", "polygons", "linestrings"])[gdf.linestrings.is_3D == 1]
    gdf
    # Example 1: Check if 3D geometries in column "linestings" is are closed or not.
    gdf.assign(res=gdf.linestrings.is_closed_3D)
    # Example 2: Filter the rows where 3D geometry in column 'linestrings' is not closed.
    gdf[gdf.linestrings.is_closed_3D != 1]
is_closed
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.is_closed
DESCRIPTION:
    Tests if a Geometry type that represents an ST_LineString, GeoSequence,
    or ST_MultiLineString value is closed.
    Note:
        *  An ST_LineString value is closed if the start point of the
          ST_LineString value is equal to the end point.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    ST_LineString, GeoSequence, and ST_MultiLineString
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
    Resultant column contains:
        * 1, if the ST_LineString, GeoSequence, or ST_LineString components
          of an ST_MultiLineString are closed.
        * 0, if the ST_LineString, GeoSequence, or ST_LineString components
          of an ST_MultiLineString are not closed, or if the input geometry
          is empty.
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    gdf = GeoDataFrame("sample_shapes")
    gdf = gdf.select(["skey", "polygons", "linestrings"])[gdf.skey.isin([1001, 1002, 1003])]
    gdf
    # Example 1: Check if geometries in column 'linestrings' is are closed or not.
    gdf.assign(res=gdf.linestrings.is_closed)
    # Example 2: Filter the rows where geometry in column 'linestrings' is closed.
    gdf[gdf.linestrings.is_closed == 1]
is_ring
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.is_ring
DESCRIPTION:
    Tests if a Geometry type that represents an ST_LineString or a
    GeoSequence value is a ring.
    Note:
        *  is_ring returns zero if the geometry value passed to it is an
          empty set.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    ST_LineString or GeoSequence
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
    Resultant column contains:
        * 1, if the ST_LineString or GeoSequence value is simple (has no
          anomalous geometric points, such as self intersection tangency)
          and is closed (the start point of the ST_LineString value is
          equal to the end point)
        * 0, in all other cases.
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    gdf = GeoDataFrame("sample_shapes")
    gdf = gdf.select(["skey", "polygons", "linestrings"])[~gdf.skey.isin([1009, 1008, 1010, 1007])]
    gdf
    # Example 1: Check if geometries in column 'linestrings' is ring or not.
    gdf.assign(res=gdf.linestrings.is_ring)
    # Example 2: Filter the rows where geometry in column 'linestrings' is not ring.
    gdf[gdf.linestrings.is_ring == 0]
Properties for Polygon Geometry
area
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.area
DESCRIPTION:
    Returns the area measurement of an ST_Polygon or ST_MultiPolygon. For
    ST_MultiPolygon, returns the sum of the area measurements of the
    component polygons.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    ST_Polygon and ST_MultiPolygon
    Note:
        This method can be called on 3D geometries (those that include Z
        coordinates). However, the Z coordinate is ignored in method
        calculations. Consequently, any Z coordinates returned by this
        method should be ignored. Teradata recommends using the make_2D()
        method to strip out the Z coordinates of the return value.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    gdf = GeoDataFrame("sample_shapes")
    gdf = gdf.select(["skey", "polygons", "linestrings"])
    gdf
    # Example 1: Get the area of all geometries in column 'polygons'.
    gdf.assign(res=gdf.polygons.area)
    # Example 2: Filter the rows where area of geometry (polygon) is greater than 400.
    gdf[gdf.polygons.area > 400]
exterior
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.exterior
DESCRIPTION:
    Get the exterior ring of a Geometry type that represents an
    ST_Polygon value.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    ST_Polygon
    Note:
        This method can be called on 3D geometries (those that include Z
        coordinates). However, the Z coordinate is ignored in method
        calculations. Consequently, any Z coordinates returned by this
        method should be ignored. Teradata recommends using the make_2D()
        method to strip out the Z coordinates of the return value.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn containing ST_LineString Geometry values
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    gdf = GeoDataFrame("sample_shapes")
    gdf = gdf.select(["skey", "polygons", "linestrings"])[gdf.skey.isin([1001, 1002, 1003])]
    gdf
    # Example 1: Get the exterior ring of all Polygon geometries in column 'polygons'.
    gdf.assign(res=gdf.polygons.exterior)
perimeter
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.perimeter
DESCRIPTION:
    Returns the boundary length of an ST_Polygon, or the sum of the boundary
    lengths of the component polygons of an ST_MultiPolygon.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    ST_Polygon or ST_MultiPolygon
    Note:
        This method can be called on 3D geometries (those that include Z
        coordinates). However, the Z coordinate is ignored in method
        calculations.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    gdf = GeoDataFrame("sample_shapes")
    gdf = gdf.select(["skey", "polygons", "linestrings"])
    gdf
    # Example 1: Get the perimeter of all geometries in column 'polygons'.
    gdf.assign(res=gdf.polygons.perimeter)
    # Example 2: Filter the rows where perimeter of polygon is 80.
    gdf[gdf.polygons.perimeter == 80]
Geospatial Specific Methods
Methods for All Types of Geometries
buffer
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.buffer = buffer(self, distance)
DESCRIPTION:
    Returns all points whose distance from a Geometry value is less than or
    equal to a specified distance.
    Note:
        *  The "distance" argument is measured in the linear units of measure in
          the spatial reference system of the Geometry value.
PARAMETERS:
    distance:
        Required Argument.
        Specifies the distance from the geometry value to the buffer.
        Types: float, str, ColumnExpression
SUPPORTED GEOMETRY TYPES:
    All Geometry types
    Note:
        This method can be called on 3D geometries (those that include Z
        coordinates). However, the Z coordinate is dropped, and only x and y
        coordinates are returned.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn containing Geometry values
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only skey and points columns from GeoDataFrame.
    points_df = geodf.select(["skey", "points"])
    # Example 1: Get all the points with distance less than or equal to 2.8 from the geometries
    #            specified in column 'points'
    points_df.assign(res = points_df.points.buffer(2.8))
contains
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.contains = contains(self, geom_column)
DESCRIPTION:
    Tests if a Geometry value spatially contains another Geometry value.
    Note:
        *  Using a geospatial index can greatly improve the performance of
          queries that use this method.
PARAMETERS:
    geom_column:
        Required Argument.
        Specifies the other Geometry value.
        Types: str, ColumnExpression, GeometryType
SUPPORTED GEOMETRY TYPES:
    All Geometry types except geometry collections.
    Note:
        This method can be called on 3D geometries (those that include Z
        coordinates). However, the Z coordinate is ignored in method
        calculations.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
    Resultant column contains:
        * 1, if the input geometry contains other geometry
        * 0, if the input geometry does not contain other geometry
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only few columns from GeoDataFrame.
    points_lines = geodf.select(["skey", "points", "linestrings"])
    # Example 1: Check if the ST_Linestring/ST_MultiLinestring type geometries in column
    #            'linestrings' contains the geometries in column 'points' of type ST_Point/ST_MultiPoint.
    # Pass the GeoDataFrameColumnColumn object of 'points' columns as input to "geom_column" argument.
    contains_expr = points_lines.linestrings.contains(geom_column=points_lines.points)
    points_lines.assign(res = contains_expr)
    # Example 2: Check if the ST_Linestring/ST_MultiLinestring type geometries in column
    #            'linestrings' contains the geometry specified by a GeometryType object - Point.
    # Create a Point GeometryType object with co-ordinates 1 and 3.
    point1 = Point(1, 3)
    # Pass the Point() GeometryType object as input to "geom_column" argument.
    contains_expr = points_lines.points.contains(geom_column=point1)
    points_lines.assign(res = contains_expr)
    # Example 3: Filter rows if the ST_Linestring/ST_MultiLinestring type geometries in column
    #            'linestrings' contains the geometries in column 'points' of type ST_Point/ST_MultiPoint.
    # Pass the GeoDataFrameColumnColumn object of 'points' columns as input to "geom_column" argument.
    contains_expr = points_lines.linestrings.contains(geom_column=points_lines.points)
    points_lines[contains_expr == 1]
crosses
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.crosses = crosses(self, geom_column)
DESCRIPTION:
    Tests if a Geometry value spatially crosses another Geometry value.
    Note:
        *  Using a geospatial index can greatly improve the performance of
          queries that use this method.
PARAMETERS:
    geom_column:
        Required Argument.
        Specifies the other Geometry value.
        Types: str, ColumnExpression, GeometryType
SUPPORTED GEOMETRY TYPES:
    All Geometry types except geometry collections.
    Note:
        This method can be called on 3D geometries (those that include Z
        coordinates). However, the Z coordinate is ignored in method
        calculations.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
    Resultant column contains:
        * 1, if the geometries cross
        * 0, if the geometries do not cross
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only few columns from GeoDataFrame.
    points_lines = geodf.select(["skey", "points", "linestrings"])
    # Example 1: Check if the ST_Linestring/ST_MultiLinestring type geometries in column
    #            'linestrings' crosses the geometries in column 'points' of type ST_Point/ST_MultiPoint.
    # Pass the GeoDataFrameColumnColumn object of 'points' columns as input to "geom_column" argument.
    crosses_expr = points_lines.linestrings.crosses(geom_column=points_lines.points)
    points_lines.assign(res = crosses_expr)
    # Example 2: Check if the ST_Linestring/ST_MultiLinestring type geometries in column
    #            'linestrings' crosses the geometry specified by a GeometryType object - LineString.
    # Create an object of Linestring GeometryType.
    line1 = LineString([(3, 1), (0,2)])
    # Pass the Linestring() GeometryType object as input to "geom_column" argument.
    crosses_expr = points_lines.linestrings.crosses(geom_column=line1)
    points_lines.assign(res = crosses_expr)
    # Example 3: Filter rows if the ST_Linestring/ST_MultiLinestring type geometries in column
    #            'linestrings' crosses the geometry specified by a GeometryType object - LineString.
    # Create an object of Linestring GeometryType.
    line1 = LineString([(3, 1), (0,2)])
    # Pass the Linestring() GeometryType object as input to "geom_column" argument.
    crosses_expr = points_lines.linestrings.crosses(geom_column=line1)
    points_lines[crosses_expr == 1]
difference
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.difference = difference(self, geom_column)
DESCRIPTION:
    Returns a Geometry value that represents the point set difference of
    two Geometry values.
PARAMETERS:
    geom_column:
        Required Argument.
        Specifies the other Geometry value.
        Types: str, ColumnExpression, GeometryType
SUPPORTED GEOMETRY TYPES:
    All Geometry types except geometry collections.
    Note:
        This method can be called on 3D geometries (those that include Z
        coordinates). However, the Z coordinate is ignored in method
        calculations. Consequently, any Z coordinates returned by this
        method should be ignored. Teradata recommends using the make_2D()
        method to strip out the Z coordinates of the return value.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
    Notes:
        Returns a Geometry type where the representation is one of the
        possible set of types in the following table, depending on the
        parameter types.
        +==============+=====+=====+==============+======+======+=======+=======+
        | a - b        | Ø   | Pnt | Line, GeoSeq | Poly | MPnt | MLine | MPoly |
        +==============+=====+=====+==============+======+======+=======+=======+
        | Ø            | Ø   | Ø   | Ø            | Ø    | Ø    | Ø     | Ø     |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        | Pnt          | R01 | R09 | R09          | R09  | R09  | R09   | R09   |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        | Line, GeoSeq | R02 | R02 | R08          | R08  | R02  | R08   | R08   |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        | Poly         | R03 | R03 | R03          | R14  | R14  | R14   | R14   |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        | MPnt         | R04 | R13 | R13          | R13  | R13  | R13   | R13   |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        | MLine        | R05 | R05 | R08          | R08  | R05  | R08   | R08   |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        | MPoly        | R06 | R06 | R06          | R14  | R06  | R05   | R06   |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        Where:
            Pnt, R01   = ST_Point
            Line, R02  = ST_LineString
            Poly, R03  = ST_Polygon
            MPnt, R04  = ST_MultiPoint
            MLine, R05 = ST_MultiLineString
            MPoly, R06 = ST_MultiPolygon
            GeoSeq     = GeoSequence
            R08        = Ø, ST_LineString, ST_MultiLineString
            R09        = Ø, ST_Point
            R13        = Ø, ST_Point, ST_MultiPoint
            R14        = Ø, ST_Polygon, ST_MultiPolygon
        Vantage converts GeoSequence types that are involved in the difference()
        method to ST_LineString values. Therefore, difference() never returns a GeoSequence
        type.
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only few columns from GeoDataFrame.
    points_lines = geodf.select(["skey", "points", "linestrings"])
    # Example 1: Get the difference between ST_Linestring/ST_MultiLinestring geometries in column
    #            'linestrings' and ST_Point/ST_MultiPoint geometries in column 'points'.
    _expr = points_lines.linestrings.difference(geom_column=points_lines.points)
    points_lines.assign(res = _expr)
    # Example 2: Get the difference between ST_Linestring/ST_MultiLinestring geometries in column
    #            'linestrings' and the geometry specified by a GeometryType object - LineString.
    # Create an object of LineString GeometryType.
    l1 = LineString([(3, 1), (0,2)])
    # Pass the LineString() GeometryType object as input to "geom_column" argument.
    _expr = points_lines.linestrings.difference(geom_column=l1)
    points_lines.assign(res = _expr)
    # Example 3: Filter the rows where the difference between ST_Linestring/ST_MultiLinestring
    #            geometries in column 'linestrings' and ST_Point/ST_MultiPoint geometries in
    #            column 'points' is same as specific LineString GeometryType object.
    # Create an object of LineString GeometryType.
    l1 = LineString([(1, 3, 6), (3, 0, 6), (6, 0, 1)])
    _expr = points_lines.linestrings.difference(geom_column=points_lines.points)
    points_lines[(_expr == l1)]
disjoint
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.disjoint = disjoint(self, geom_column)
DESCRIPTION:
    Tests if a Geometry value is spatially disjoint from another Geometry
    value.
PARAMETERS:
    geom_column:
        Required Argument.
        Specifies the other Geometry value.
        Types: str, ColumnExpression, GeometryType
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
    Note:
        This method can be called on 3D geometries (those that include Z
        coordinates). However, the Z coordinate is ignored in method
        calculations.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
    Resultant column contains:
        * 1, if the geometries are spatially disjoint
        * 0, if the geometries are not spatially disjoint
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only few columns from GeoDataFrame.
    points_lines = geodf.select(["skey", "points", "linestrings"])
    # Example 1: Check whether the ST_Linestring/ST_MultiLinestring geometries in column
    #            'linestrings' and ST_Point/ST_MultiPoint geometries in column 'points'
    #            are disjoint or not.
    disjoint1_expr = points_lines.linestrings.disjoint(geom_column=points_lines.points)
    points_lines.assign(res = disjoint1_expr)
    # Example 2: Check whether the ST_Linestring/ST_MultiLinestring geometries in column
    #            'linestrings' and the geometry specified by a GeometryType object - LineString,
    #            are disjoint or not.
    # Create an object of Linestring GeometryType.
    l1 = LineString([(3, 1), (0,2)])
    # Pass the LineString() GeometryType object as input to "geom_column" argument.
    disjoint2_expr = points_lines.linestrings.disjoint(geom_column=l1)
    points_lines.assign(res = disjoint2_expr)
    # Example 3: Filter rows where ST_Linestring/ST_MultiLinestring geometries in column
    #            'linestrings' and ST_Point/ST_MultiPoint geometries in column 'points'
    #            are disjoint.
    disjoint1_expr = points_lines.linestrings.disjoint(geom_column=points_lines.points)
    points_lines[disjoint1_expr == 1]
distance
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.distance = distance(self, geom_column)
DESCRIPTION:
    Returns the distance between two Geometry values.
    Notes:
        *  distance() returns NULL if the geometry value passed to it is the
          empty set.
        *  Both geometries must use the same spatial reference system (have the
          same SRS ID).
        *  If a geospatial index is defined on the geospatial data column to
          which this method is applied, the index will only be used if method
          appears in the slice filtering such that the distance between the
          geometries is less than or equal to a constant.
          For example:
            *  geodf[geodf.geometry.distance(otherGeom) < 10]
            *  geodf[10 >= geodf.geometry.distance(otherGeom)]
PARAMETERS:
    geom_column:
        Required Argument.
        Specifies the other Geometry value.
        Types: str, ColumnExpression, GeometryType
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only few columns from GeoDataFrame.
    points_lines = geodf.select(["skey", "points", "linestrings"])
    # Example 1: Calculate the distance between (ST_Linestring/ST_MultiLinestring) geometries in column
    #            'linestrings' and (ST_Point/ST_MultiPoint) geometries in column 'points'.
    distance1_expr = points_lines.points.distance(geom_column=points_lines.linestrings)
    points_lines.assign(res = distance1_expr)
    # Example 2: Calculate the distance between (ST_Linestring/ST_MultiLinestring) geometries in column
    #            'linestrings' and the geometry specified by a GeometryType object - Point.
    # Create an object of Point GeometryType.
    p1 = Point(3, 1)
    # Pass the Point() GeometryType object as input to "geom_column" argument.
    distance2_expr = points_lines.points.distance(geom_column=p1)
    points_lines.assign(res = distance2_expr)
    # Example 3: Get all the rows where distance between (ST_Linestring/ST_MultiLinestring) geometries in column
    #            'linestrings' and (ST_Point/ST_MultiPoint) geometries in column 'points' is > 10 and less than 100.
    distance1_expr = points_lines.points.distance(geom_column=points_lines.linestrings)
    points_lines[(distance1_expr > 10) & (distance1_expr < 100)]
distance_3D
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.distance_3D = distance_3D(self, geom_column)
DESCRIPTION:
    Returns the distance between two Geometry values. Function
    considers Z coordinate values, if they exist in the geometries passed to
    it.
    Notes:
        *  distance() returns NULL if the geometry value passed to it is the
          empty set.
        *  Both geometries must use the same spatial reference system (have the
          same SRS ID).
        *  If a geospatial index is defined on the geospatial data column to
          which this method is applied, the index will only be used if method
          appears in the slice filtering such that the distance between the
          geometries is less than or equal to a constant.
          For example:
            *  geodf[geodf.geometry.distance(otherGeom) < 10]
            *  geodf[10 >= geodf.geometry.distance(otherGeom)]
PARAMETERS:
    geom_column:
        Required Argument.
        Specifies the other Geometry value.
        Types: str, ColumnExpression, GeometryType
SUPPORTED GEOMETRY TYPES:
    ST_Point, ST_MultiPoint, ST_LineString, and ST_MultiLineString
    To be valid with distance_3D, these shapes must include Z coordinates.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
    Resultant column contains:
        * a NULL, if the Geometry is an empty set.
        * 0, if the two geometries intersect.
        * a float in all other cases. The distance units are those of
          the two input geometries.
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Function works with only 3D geometries, hence let's select only few columns from GeoDataFrame
    # and filter the rows which contains 3D geometries.
    points_lines = geodf.select(["skey", "points", "linestrings"])[geodf.points.is_3D == 1]
    # Example 1: Calculate the distance between 3D geometries in column 'linestrings' and 3D
    #            geometries in column 'points'.
    distance1_expr = points_lines.points.distance_3D(geom_column=points_lines.linestrings)
    points_lines.assign(res = distance1_expr)
    # Example 2: Calculate the distance between 3D geometries in column 'linestrings' and 3D Point
    #            GeometryType object.
    # Create a 3D Point GeometryType object.
    p1 = Point(3, 1, 0)
    # Pass the Point() GeometryType object as input to "geom_column" argument.
    distance1_expr = points_lines.points.distance_3D(geom_column=p1)
    points_lines.assign(res = distance1_expr)
    # Example 3: Get all the rows where the distance between 3D geometries in column 'linestrings' and 3D
    #            geometries in column 'points' is greater than 30 but less than 50.
    distance1_expr = points_lines.points.distance_3D(geom_column=points_lines.linestrings)
    points_lines[(distance1_expr > 30) & (distance1_expr < 50)]
envelope
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.envelope = envelope(self)
DESCRIPTION:
    Returns the bounding rectangle for the Geometry value.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
    Note:
        This method can be called on 3D geometries (those that include Z
        coordinates). However, the Z coordinate is dropped, and only x and y
        coordinates are returned.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn containing Geometry values
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Get the bounding rectangle for (ST_Point/ST_MultiPoint) geometries in column
    #            'points'.
    points_lines = geodf.select(["skey", "points", "linestrings"])
    ep_expr = points_lines.geometry.envelope()
    points_lines.assign(res = ep_expr)
    # Example 2: Get the bounding rectangle for (ST_Linestring/ST_MultiLinestring) geometries in column
    #            'linestrings'.
    ep_expr = points_lines.linestrings.envelope()
    points_lines.assign(res = ep_expr)
    # Example 3: Filter the rows where the bounding rectangle for (ST_Point/ST_MultiPoint)
    #            geometries in column 'points' is same as the specified Polygon GeometryType
    #            object.
    # Create an object of Polygon GeometryType.
    po1 = Polygon([(1, 1), (20, 1), (20, 5), (1, 5), (1, 1)])
    points_lines[points_lines.points.envelope() == po1]
geom_equals
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.geom_equals = geom_equals(self, geom_column)
DESCRIPTION:
    Tests if a Geometry value is spatially equal to another Geometry value.
    Note:
        *  Using a geospatial index can greatly improve the performance of
          queries that use this method.
PARAMETERS:
    geom_column:
        Required Argument.
        Specifies the other Geometry value.
        Types: str, ColumnExpression, GeometryType
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
    Resultant column contains:
        * 1, if the geometries are spatially equal
        * 0, if the geometries are not spatially equal
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only few columns from GeoDataFrame.
    points_lines = geodf.select(["skey", "points", "linestrings"])
    # Example 1: Check whether the ST_Point/ST_MultiPoint type geometries in column
    #            'points' equals geometry specified by a GeometryType object - Point.
    # Create an object of Point GeometryType.
    p1 = Point(10, 20)
    # Pass the Point() GeometryType object as input to "geom_column" argument.
    ge_expr = points_lines.geometry.geom_equals(geom_column=p1)
    points_lines.assign(res = ge_expr)
    # Example 2: Get all rows where he ST_Linestring/ST_MultiLinestring type geometries in column
    #            'linestrings' is not equal to geometry specified by a GeometryType object - LineString.
    # Create an object of Linestring GeometryType.
    l1 = LineString([(1,3), (3,0), (0,1)])
    # Pass the LineString() GeometryType object as input to "geom_column" argument.
    ge_expr = points_lines.linestrings.geom_equals(geom_column=l1)
    points_lines[ge_expr != 1] # One can use [ge_expr == 0] as well.
intersection
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.intersection = intersection(self, geom_column)
DESCRIPTION:
    Returns a Geometry type where the value represents the point set
    intersection of two Geometry values.
PARAMETERS:
    geom_column:
        Required Argument.
        Specifies the other Geometry value.
        Types: str, ColumnExpression, GeometryType
SUPPORTED GEOMETRY TYPES:
    All Geometry types, including GeoSequence, but not the ST_GeomCollection type.
    Note:
        This method can be called on 3D geometries (those that include Z
        coordinates). However, the Z coordinate is ignored in method
        calculations. Consequently, any Z coordinates returned by this
        method should be ignored. Teradata recommends using the make_2D()
        method to strip out the Z coordinates of the return value.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
    Notes:
        Returns a Geometry type where the representation is one of the
        possible set of types in the following table, depending on the
        parameter types.
        +==============+=====+=====+==============+======+======+=======+=======+
        | a n b        | Ø   | Pnt | Line, GeoSeq | Poly | MPnt | MLine | MPoly |
        +==============+=====+=====+==============+======+======+=======+=======+
        | Ø            | Ø   | Ø   | Ø            | Ø    | Ø    | Ø     | Ø     |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        | Pnt          | Ø   | R09 | R09          | R09  | R09  | R09   | R09   |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        | Line, GeoSeq | Ø   | R09 | R11          | R11  | R13  | R11   | R11   |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        | Poly         | Ø   | R09 | R11          | R12  | R13  | R11   | R12   |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        | MPnt         | Ø   | R09 | R13          | R13  | R13  | R13   | R13   |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        | MLine        | Ø   | R09 | R11          | R11  | R13  | R11   | R11   |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        | MPoly        | Ø   | R09 | R11          | R12  | R13  | R11   | R12   |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        Where:
            Pnt     = ST_Point
            Line    = ST_LineString
            Poly    = ST_Polygon
            MPnt    = ST_MultiPoint
            MLine   = ST_MultiLineString
            MPoly   = ST_MultiPolygon
            GeoSeq  = GeoSequence
            R09     = Ø, ST_Point
            R11     = Ø, ST_Point, ST_LineString, ST_MultiPoint, ST_MultiLineString
            R12     = Ø, ST_Point, ST_LineString, ST_Polygon, ST_MultiPoint,
                      ST_MultiLineString, ST_MultiPolygon
            R13     = Ø, ST_Point, ST_MultiPoint
        Vantage converts GeoSequence types that are involved in the intersection()
        method to ST_LineString values. Therefore, intersection() never returns a
        GeoSequence type.
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only few columns from GeoDataFrame.
    points_lines = geodf.select(["skey", "points", "linestrings"])
    # Example 1: Get the point set interection geometry value between the geometries in columns
    #            'points' and 'linestrings'.
    geo_ints1 = points_lines.geometry.intersection(geom_column=points_lines.linestrings)
    points_lines.assign(res = geo_ints1)
    # Example 2: Get the point set interection geometry value between the geometries in column
    #            'linestrings' and geometry type specified by Linestring.
    # Create an object of Linestring GeometryType.
    l1 = LineString([(1,3), (3,0), (0,1)])
    # Pass the LineString() GeometryType object as input to "geom_column" argument.
    geo_ints2 = points_lines.linestrings.intersection(geom_column=l1)
    points_lines.assign(res = geo_ints2)
    # Example 3: Filter the rows where the point set interection geometry value between the geometries
    #            in columns 'points' and 'linestrings' is same as the Empty GeometryCollection object.
    # Create an object of GeometryCollection GeometryType.
    gc1 = GeometryCollection()
    # Execute intersection() function on 'points' and 'linestrings' columns.
    geo_ints1 = points_lines.points.intersection(geom_column=points_lines.linestrings)
    points_lines[geo_ints1 == gc1]
intersects
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.intersects = intersects(self, geom_column)
DESCRIPTION:
    Tests if a Geometry value spatially intersects another Geometry value.
    Note:
        *  Using a geospatial index can greatly improve the performance of
          queries that use this method.
PARAMETERS:
    geom_column:
        Required Argument.
        Specifies the other Geometry value.
        Types: str, ColumnExpression, GeometryType
SUPPORTED GEOMETRY TYPES:
    All Geometry types except geometry collections.
    Note:
        This method can be called on 3D geometries (those that include Z
        coordinates). However, the Z coordinate is ignored in method
        calculations.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
    Resultant column contains:
        * 1, if the geometries intersect
        * 0, if the geometries do not intersect
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only few columns from GeoDataFrame.
    points_lines = geodf.select(["skey", "points", "linestrings"])
    # Example 1: Check whether the ST_Point/ST_MultiPoint type geometries in column
    #            'points' intersects with ST_Linestring/ST_MultiLinestring geometries
    #            in column 'linestrings'.
    geo_intersects1 = points_lines.points.intersects(geom_column=points_lines.linestrings)
    points_lines.assign(res = geo_intersects1)
    # Example 2: Check whether the ST_Linestring/ST_MultiLinestring type geometries in column
    #            'linestrings' intersects with GeometryType object of Linestring.
    # Create an object of Linestring GeometryType.
    l1 = LineString([(1,3), (3,0), (0,1)])
    # Pass the LineString() GeometryType object as input to "geom_column" argument.
    geo_intersects2 = points_lines.linestrings.intersects(geom_column=l1)
    points_lines.assign(res = geo_intersects2)
    # Example 3: Get all the rows where the ST_Point/ST_MultiPoint type geometries in column
    #            'points' intersects with ST_Linestring/ST_MultiLinestring geometries
    #            in column 'linestrings'.
    geo_intersects1 = points_lines.points.intersects(geom_column=points_lines.linestrings)
    points_lines[geo_intersects1 == 1]
make_2D
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.make_2D = make_2D(self, validate=False)
DESCRIPTION:
    Converts a 3D Geometry value to a 2D Geometry value, and optionally
    validates that the 2D geometry is well-formed. This method strips the z
    coordinate from 3D geometries.
    Notes:
        *  The is_valid method can be used to validate converted 2D
          geometries that were not validated by make_2D().
        *  If the input geometry is already a 2D geometry value, this value is
          returned unchanged by this method.
        *  A well-formed 3D geometry value can become invalid during a 3D to 2D
          conversion. For example, if a 3D polygon lies in a plane parallel to
          the y axis, its line segments will self-intersect. make_2D() does not
          attempt to change the geometry type of the converted geometry from
          polygon to linestring to account for this, and an invalid 2D
          geometry can result.
PARAMETERS:
    validate:
        Optional Argument.
        Specifies a value that determines whether the converted 2D geometry
        is tested to ensure it is well-formed. When "validate" is set to True,
        the 2D geometry is tested. If it is not well-formed, Vantage
        returns an error. If validate is set to False or is not specified,
        the 2D geometry is not tested.
        Default Value: False
        Types: bool, str, ColumnExpression
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Convert 3D ST_Point/ST_MultiPoint geometries in column 'points' to 2D.
    # Let's select only few columns from GeoDataFrame and get only rows which contains 3D geometries.
    points_df = geodf.select(["skey", "points"])[geodf.points.is_3D == 1]
    points_df.assign(res = points_df.geometry.make_2D())
    # Example 2: Filter rows where converted 3D ST_Point/ST_MultiPoint geometries in column 'points' to 2D
    #            is same as the specified Point GeometryType object.
    # Create an object of Point GeometryType.
    p1 = Point(1, 3)
    points_df[points_df.geometry.make_2D() == p1]
mbb
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.mbb = mbb(self)
DESCRIPTION:
    Returns the 3D minimum bounding box (MBB) that encloses a 3D Geometry.
    Note:
        *  mbb() can return a two-dimensional MBB, if appropriate. Such an MBB
          has Z coordinates that are all zero. If the 3D input geometry is
          empty, all the coordinates of the resulting MBB will be zero.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    All 3D Geometry types.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only few columns from GeoDataFrame and select only geometries that are 3D.
    pos = geodf.select(["skey", "points", "polygons"])[geodf.points.is_3D == 1]
    # Example 1: Get the 3D minimum bounding box for 3D ST_Point/ST_MultiPoint geometries
    #            in column 'points'.
    pos.assign(res = pos.points.mbb())
    # Example 2: Get the 3D minimum bounding box for 3D ST_Polygon/ST_MultiPolygon geometries
    #            in column 'polygons'.
    pos.assign(res = pos.polygons.mbb())
mbr
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.mbr = mbr(self)
DESCRIPTION:
    Returns the minimum bounding rectangle (MBR) of a Geometry value.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only few columns from GeoDataFrame.
    pos = geodf.select(["skey", "points", "polygons"])
    # Example 1: Get the minimum bounding rectangle for ST_Point/ST_MultiPoint geometries
    #            in column 'points'.
    pos.assign(res = pos.points.mbr())
    # Example 2: Get the minimum bounding rectangle for ST_Polygon/ST_MultiPolygon geometries
    #            in column 'polygons'.
    pos.assign(res = pos.polygons.mbr())
overlaps
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.overlaps = overlaps(self, geom_column)
DESCRIPTION:
    Tests if a Geometry value spatially overlaps another Geometry value.
    Note:
        *  Using a geospatial index can greatly improve the performance of
          queries that use this method.
PARAMETERS:
    geom_column:
        Required Argument.
        Specifies the other Geometry value.
        Types: str, ColumnExpression, GeometryType
SUPPORTED GEOMETRY TYPES:
    Valid on all Geometry types, except geometry collections.
    Note:
        This method can be called on 3D geometries (those that include Z
        coordinates). However, the Z coordinate is ignored in method
        calculations.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
    Resultant column contains:
        * 1, if the geometries overlap
        * 0, if the geometries do not overlap
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only few columns from GeoDataFrame.
    pols_lines = geodf.select(["skey", "polygons", "linestrings"])
    # Example 1: Check whether geometries in column 'polygons' and 'lienstrings'
    #            overlaps or not.
    pols_lines.assign(res = pols_lines.polygons.overlaps(geom_column=pols_lines.linestrings))
    # Example 2: Check whether geometries in column 'lienstrings' columns and
    #            a Linestring GeometryType object overlaps or not.
    # Create an object of Linestring GeometryType.
    l1 = LineString([(1,3,6), (3,0,6), (6,0,1)])
    # Pass the LineString() GeometryType object as input to "geom_column" argument.
    pols_lines.assign(res = pols_lines.linestrings.overlaps(l1))
    # Example 3: Filter all rows whether geometries in column 'lienstrings' columns and
    #            a Linestring GeometryType object overlaps.
    # Create an object of Linestring GeometryType.
    l1 = LineString([(1,3,6), (3,0,6), (6,0,1)])
    # Pass the LineString() GeometryType object as input to "geom_column" argument.
    pols_lines[pols_lines.linestrings.overlaps(l1) == 1]
relates
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.relates = relates(self, geom_column, amatrix)
DESCRIPTION:
    Tests if a Geometry value is spatially related to another Geometry
    value.
    Note:
        *  Using a geospatial index can greatly improve the performance of
          queries that use this method.
PARAMETERS:
    geom_column:
        Required Argument.
        Specifies the other Geometry value.
        Types: str, ColumnExpression, GeometryType
    amatrix:
        Required Argument.
        Specifies a value where each character corresponds to a cell in the
        DE-9IM pattern matrix.
        Types: str
        Notes:
            The mapping for amatrix, as defined by SQL/MM Spatial, is as follows:
            +==========+=====================================================+
            | Position | DE-9IM Cell                                         |
            +==========+=====================================================+
            | 1        | (Interior(SELF) ∩ Interior(ageometry)).dimension    |
            +----------+-----------------------------------------------------+
            | 2        | (Interior(SELF) ∩ Boundary(ageometry)).dimension    |
            +----------+-----------------------------------------------------+
            | 3        | (Interior(SELF) ∩ Exterior(ageometry)).dimension    |
            +----------+-----------------------------------------------------+
            | 4        | (Boundary(SELF) ∩ Interior(ageometry)).dimension    |
            +----------+-----------------------------------------------------+
            | 5        | (Boundary(SELF) ∩ Boundary(ageometry)).dimension    |
            +----------+-----------------------------------------------------+
            | 6        | (Boundary(SELF) ∩ Exterior(ageometry)).dimension    |
            +----------+-----------------------------------------------------+
            | 7        | (Exterior(SELF) ∩ Interior(ageometry)).dimension    |
            +----------+-----------------------------------------------------+
            | 8        | (Exterior(SELF) ∩ Boundary(ageometry)).dimension    |
            +----------+-----------------------------------------------------+
            | 9        | (Exterior(SELF) ∩ Exterior(ageometry)).dimension    |
            +==========+=====================================================+
            Valid values for each character are: 'T', 'F', '0', '1', '2', and '*'.
            The value specifies the set of acceptable values for an intersection
            at that given cell. The meaning is as follows:
            +============+==========================+
            | Cell Value | Intersection Set Results |
            +============+==========================+
            | 'T'        | { 0, 1, 2 }              |
            +------------+--------------------------+
            | 'F'        | { -1 }                   |
            +------------+--------------------------+
            | '0'        | { 0 }                    |
            +------------+--------------------------+
            | '1'        | { 1 }                    |
            +------------+--------------------------+
            | '2'        | { 2 }                    |
            +------------+--------------------------+
            | '*'        | { -1, 0, 1, 2 }          |
            +============+==========================+
SUPPORTED GEOMETRY TYPES:
    All Geometry types except geometry collections.
    Note:
        This method can be called on 3D geometries (those that include Z
        coordinates). However, the Z coordinate is ignored in method
        calculations.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
    Resultant column contains:
        * 1, if the spatial relationship between the geometries corresponds
          to one of the acceptable values represented by "amatrix"
        * 0, if the spatial relationship between the geometries does not
          correspond to one of the acceptable values represented by "amatrix"
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Check if geometry values in column 'points' are spatially related to
    #            geometry values in column 'linestrings'.
    # Let's select only few columns from GeoDataFrame.
    point_lines = geodf.select(["skey", "points", "linestrings"])
    relates = point_lines.points.relates(geom_column=point_lines.linestrings, amatrix="0********")
    point_lines.assign(res = relates)
    # Example 2: Filter rows if geometry values in column 'points' are spatially related to
    #            geometry values in column 'linestrings'.
    relates = point_lines.points.relates(geom_column=point_lines.linestrings, amatrix="0********")
    point_lines[relates == 1]
set_exterior
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.set_exterior = set_exterior(self)
DESCRIPTION:
    Set the exterior ring of a Geometry type that represents an
    ST_Polygon value.
    Note:
        *  set_exterior() returns an error if the geometry value passed to it
          is the empty set.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    ST_Polygon
    Note:
        This method can be called on 3D geometries (those that include Z
        coordinates). However, the Z coordinate is ignored in method
        calculations. Consequently, any Z coordinates returned by this
        method should be ignored. Teradata recommends using the make_2D()
        method to strip out the Z coordinates of the return value.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
EXAMPLES:
    TODO
set_srid
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.set_srid = set_srid(self, srid)
DESCRIPTION:
    Set the spatial reference system identifier of the Geometry
    value.
PARAMETERS:
    srid:
        Required Argument.
        Specifies a value for the spatial reference system identifier.
        Types: int, str, ColumnExpression
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
    Resultant column contains the Geometry with the spatial reference system identifier set
    to the specified spatial reference system.
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Set the spatial reference system identifier for the geometries in column 'geosequence'.
    # Let's select only few columns from GeoDataFrame.
    geoseq = geodf.select(["skey", "geosequence"])[geodf.skey.isin([1001, 1002])]
    geoseq.assign(res = geoseq.geosequence.set_srid(srid=20))
simplify
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.simplify = simplify(self, tolerance)
DESCRIPTION:
    Simplifies a geometry by removing points that would fall within a
    specified distance tolerance.
    Notes:
        *  The simplification always returns a valid geometry.
        *  Simplified geometries require less storage space and fewer spatial
          operations during geospatial manipulations. Consequently, operations
          on simplified geometries generally perform faster.
        *  Smaller tolerance values result in a geometry closer to the input
          geometry, but will remove fewer vertices. Larger tolerance values
          will remove more vertices, but the resulting simplified geometry
          will be less similar to the original input.
PARAMETERS:
    tolerance:
        Required Argument.
        Specifies the distance tolerance for the simplification. The
        simplified line will never be farther away from the original line
        than the tolerance value.
        Types: float, str, ColumnExpression
SUPPORTED GEOMETRY TYPES:
    Valid on the following 2D geometries:
        LineString, MultiLineString, Polygon, and MultiPolygon.
    It is valid on these types within Geometry Collections. If method is called on other 2D
    geometries, those geometries are returned unchanged.
    This method is not valid on 3D geometries.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn containing Geometry values
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only few columns from GeoDataFrame.
    pols_lines = geodf.select(["skey", "polygons", "linestrings"])[geodf.skey.isin([1001, 1002, 1003])]
    # Example 1: Simplify the geometry in column 'polygons' within a specified distance tolerance of 2.8.
    pols_lines.assign(res = pols_lines.polygons.simplify(2.8))
    # Example 2: Simplify the geometry in column 'linestrings' within a specified distance tolerance of 2.8.
    pols_lines.assign(res = pols_lines.linestrings.simplify(tolerance=2.8))
sym_difference
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.sym_difference = sym_difference(self, geom_column)
DESCRIPTION:
    Returns a Geometry value that represents the point set symmetric
    difference of two Geometry values.
PARAMETERS:
    geom_column:
        Required Argument.
        Specifies the other Geometry value.
        Types: str, ColumnExpression, GeometryType
SUPPORTED GEOMETRY TYPES:
    All Geometry types except geometry collections.
    Note:
        This method can be called on 3D geometries (those that include Z
        coordinates). However, the Z coordinate is ignored in method
        calculations. Consequently, any Z coordinates returned by this
        method should be ignored. Teradata recommends using the make_2D()
        method to strip out the Z coordinates of the return value.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
    Notes:
        Returns a Geometry type where the representation is one of the
        possible set of types in the following table, depending on the
        parameter types.
        +==============+=====+=====+==============+======+======+=======+=======+
        | u (a - b)    | Ø   | Pnt | Line, GeoSeq | Poly | MPnt | MLine | MPoly |
        +==============+=====+=====+==============+======+======+=======+=======+
        | Ø            | Ø   | R01 | R02          | R03  | R04  | R05   | R06   |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        | Pnt          | R01 | R10 | R15          | R22  | R10  | R17   | R19   |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        | Line, GeoSeq | R02 | R15 | R08          | R21  | R15  | R08   | R18   |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        | Poly         | R03 | R22 | R21          | R14  | R22  | R21   | R14   |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        | MPnt         | R04 | R10 | R15          | R22  | R10  | R17   | R19   |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        | MLine        | R05 | R17 | R08          | R21  | R17  | R08   | R18   |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        | MPoly        | R06 | R19 | R18          | R14  | R19  | R18   | R14   |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        Where:
            Pnt, R01    = ST_Point
            Line, R02   = ST_LineString
            Poly, R03   = ST_Polygon
            MPnt, R04   = ST_MultiPoint
            MLine, R05  = ST_MultiLineString
            MPoly, R06  = ST_MultiPolygon
            GeoSeq      = GeoSequence
            R08         = Ø, ST_LineString, ST_MultiLineString
            R10         = Ø, ST_MultiPoint
            R14         = Ø, ST_Polygon, ST_MultiPolygon
            R15         = ST_LineString,
                          ST_GeomCollection of ST_Point and ST_LineString values
            R17         = ST_MultiLineString,
                          ST_GeomCollection of ST_Point and ST_LineString values
            R18         = ST_MultiPolygon,
                          ST_GeomCollection of ST_LineString and ST_Polygon values
            R19         = ST_MultiPolygon,
                          ST_GeomCollection of ST_Point and ST_Polygon values
            R21         = ST_Polygon,
                          ST_GeomCollection of ST_LineString and ST_Polygon values
            R22         = ST_Polygon,
                          ST_GeomCollection of ST_Point and ST_Polygon values
        Vantage converts GeoSequence types that are involved in the sym_difference()
        method to ST_LineString values. Therefore, sym_difference() never returns a GeoSequence
        type.
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only few columns from GeoDataFrame.
    points_lines = geodf.select(["skey", "points", "linestrings"])
    # Example 1: Get the point set symmetric difference between geometries in columns
    #            'points' and 'linestrings'.
    points_lines.assign(res = points_lines.points.intersection(geom_column=points_lines.linestrings))
    # Example 2: Get the point set symmetric difference between geometries in columns
    #            'linestrings' and a GeometryType object - Linestring.
    # Create an object of Linestring GeometryType.
    l1 = LineString([(1,3), (3,0), (0,1)])
    # Pass the LineString() GeometryType object as input to "geom_column" argument.
    points_lines.assign(res = points_lines.linestrings.intersection(l1))
    # Example 3: Filter the rows where the point set symmetric difference between geometries in columns
    #            'points' and 'linestrings' is same as the empty GeometryCollection.
    # Create an object of empty GeometryCollection GeometryType.
    gc1 = GeometryCollection()
    points_lines[points_lines.points.intersection(geom_column=points_lines.linestrings) == gc1]
to_binary
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.to_binary = to_binary(self)
DESCRIPTION:
    Returns the well-known binary (WKB) representation of a Geometry value.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn containing BLOB value
    The maximum size of the return value is 16 MB (the exact maximum is 16 MB - 1 KB).
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Get the well-known binary (WKB) representation of geometry values
    #            in column 'points'.
    # Let's select only few columns from GeoDataFrame.
    points_lines = geodf.select(["skey", "points", "linestrings"])
    points_lines.assign(res = points_lines.points.to_binary())
to_text
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.to_text = to_text(self)
DESCRIPTION:
    Returns the well-known text (WKT) representation of a Geometry value.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn containing CLOB value
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Get the well-known text (WKT) representation of geometry values
    #            in column 'linestrings'.
    # Let's select only few columns from GeoDataFrame.
    points_lines = geodf.select(["skey", "points", "linestrings"])
    points_lines.assign(res = points_lines.linestrings.to_text())
touches
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.touches = touches(self, geom_column)
DESCRIPTION:
    Tests if a Geometry value spatially touches another Geometry value.
    Note:
        *  Using a geospatial index can greatly improve the performance of
          queries that use this method.
PARAMETERS:
    geom_column:
        Required Argument.
        Specifies the other Geometry value.
        Types: str, ColumnExpression, GeometryType
SUPPORTED GEOMETRY TYPES:
    All Geometry types except geometry collections.
    Note:
        This method can be called on 3D geometries (those that include Z
        coordinates). However, the Z coordinate is ignored in method
        calculations.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
    Resultant column contains:
        * 1, if the geometries touch
        * 0, if the geometries do not touch
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only few columns from GeoDataFrame.
    pols_lines = geodf.select(["skey", "polygons", "linestrings"])
    # Example 1: Check whether geometries in column 'polygons' and 'lienstrings'
    #            touches or not.
    pols_lines.assign(res = pols_lines.linestrings.touches(geom_column=pols_lines.polygons))
    # Example 2: Check whether geometries in column 'polygons' columns and
    #            a Polygon GeometryType object touches or not.
    # Create an object of Polygon GeometryType.
    po1 = Polygon([(0, 0), (0, 20), (20, 20), (20, 0), (0, 0)])
    # Pass the Polygon() GeometryType object as input to "geom_column" argument.
    pols_lines.assign(res = pols_lines.polygons.touches(po1))
    # Example 3: Filter all rows where geometries in column 'polygons' and 'lienstrings'
    #            touches.
    pols_lines[pols_lines.linestrings.touches(geom_column=pols_lines.polygons) == 1]
transform
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.transform = transform(self, to_wkt_srs, from_wkt_srs, to_srsid=-12345)
DESCRIPTION:
    Returns a Geometry value transformed to the specified spatial reference
    system.
    Note:
        *  The SRTEXT column of the SYSSPATIAL.SPATIAL_REF_SYS metadata table
          contains WKT representations of spatial reference systems.
PARAMETERS:
    to_wkt_srs:
        Required Argument.
        Specifies a value that is the identifier of the spatial reference
        system returned by the method.
        Types: str, ColumnExpression
    from_wkt_srs:
        Required Argument.
        Specifies a value for the WKT representation of the spatial
        reference system to transform to.
        Types: str, ColumnExpression
    to_srsid:
        Optional Argument.
        Specifies a value for the WKT representation of the spatial
        reference system to assign to the geometry value (without any
        transformation) before transforming it to the spatial reference
        system specified by "to_wkt_srs".
        Default Value: -12345
        Types: int, str, ColumnExpression
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
    Note:
        This method can be called on 3D geometries (those that include Z
        coordinates). However, the Z coordinate is ignored in method
        calculations. Consequently, any Z coordinates returned by this
        method should be ignored. Teradata recommends using the make_2D()
        method to strip out the Z coordinates of the return value.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn containing Geometry values
EXAMPLES:
    from teradataml import DataFrame, in_schema
    from teradataml import GeoDataFrame, load_example_data
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Create a teradataml DataFrame on 'SYSSPATIAL.SPATIAL_REF_SYS'
    sysref = DataFrame(in_schema("SYSSPATIAL", "SPATIAL_REF_SYS"))
    # Join the teradataml GeoDataFrame and DataFrame.
    sysref = sysref.join(sysref, how="cross", lsuffix="l", rsuffix="r")
    geodf_sysref = geodf.join(sysref, on="skey==l_SRID")
    # Execute the transform function to transform geometry in column 'points'.
    trans = geodf_sysref.points.transform(geodf_sysref.l_SRTEXT, geodf_sysref.r_SRTEXT)
    geodf_sysref.assign(res = trans)
union
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.union = union(self, geom_column)
DESCRIPTION:
    Returns a Geometry value that represents the point set union of two
    Geometry values.
PARAMETERS:
    geom_column:
        Required Argument.
        Specifies the other Geometry value.
        Types: str, ColumnExpression, GeometryType
SUPPORTED GEOMETRY TYPES:
    All Geometry types except geometry collections.
    Note:
        This method can be called on 3D geometries (those that include Z
        coordinates). However, the Z coordinate is ignored in method
        calculations. Consequently, any Z coordinates returned by this
        method should be ignored. Teradata recommends using the make_2D()
        method to strip out the Z coordinates of the return value.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
    Notes:
        Returns a Geometry type where the representation is one of the
        possible set of types in the following table, depending on the
        parameter types.
        +==============+=====+=====+==============+======+======+=======+=======+
        | a u b        | Ø   | Pnt | Line, GeoSeq | Poly | MPnt | MLine | MPoly |
        +==============+=====+=====+==============+======+======+=======+=======+
        | Ø            | Ø   | R01 | R02          | R03  | R04  | R05   | R06   |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        | Pnt          | R01 | R20 | R15          | R22  | R04  | R17   | R19   |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        | Line, GeoSeq | R02 | R15 | R16          | R21  | R15  | R16   | R18   |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        | Poly         | R03 | R22 | R21          | R23  | R22  | R21   | R23   |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        | MPnt         | R04 | R04 | R15          | R22  | R04  | R17   | R19   |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        | MLine        | R05 | R17 | R16          | R21  | R17  | R16   | R18   |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        | MPoly        | R06 | R19 | R18          | R23  | R19  | R18   | R23   |
        +--------------+-----+-----+--------------+------+------+-------+-------+
        Where:
            Pnt, R09    = ST_Point
            Line, R02   = ST_LineString
            Poly, R03   = ST_Polygon
            MPnt, R04   = ST_MultiPoint
            MLine, R05  = ST_MultiLineString
            MPoly, R06  = ST_MultiPolygon
            GeoSeq      = GeoSequence
            R15         = ST_LineString,
                          ST_GeomCollection of ST_Point and ST_LineString values
            R16         = ST_LineString, ST_MultiLineString
            R17         = ST_MultiLineString,
                          ST_GeomCollection of ST_Point and ST_LineString values
            R18         = ST_MultiPolygon,
                          ST_GeomCollection of ST_LineString and ST_Polygon values
            R19         = ST_MultiPolygon,
                          ST_GeomCollection of ST_Point and ST_Polygon values
            R20         = ST_Point, ST_MultiPoint
            R21         = ST_Polygon,
                          ST_GeomCollection of ST_LineString and ST_Polygon values
            R22         = ST_Polygon,
                          ST_GeomCollection of ST_Point ST_Polygon values
            R23         = ST_Polygon, ST_MultiPolygon
        Vantage converts GeoSequence types that are involved in the union()
        method to ST_LineString values. Therefore, union() never returns a
        GeoSequence type.
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only few columns from GeoDataFrame.
    pols_lines = geodf.select(["skey", "polygons", "linestrings"])[geodf.skey.isin([1001, 1002, 1003])]
    # Example 1: Get the point set union of geometries in columns 'polygons' and 'linestrings'.
    pols_lines.assign(res = pols_lines.linestrings.union(geom_column=pols_lines.polygons))
    # Example 2: Get the point set union of geometries in column 'polygons' and
    #            a GeometryType object - Polygon.
    # Create an object of Polygon GeometryType.
    po1 = Polygon([(0, 0), (0, 20), (20, 20), (20, 0), (0, 0)])
    # Pass the Polygon() GeometryType object as input to "geom_column" argument.
    pols_lines.assign(res = pols_lines.polygons.union(po1))
within
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.within = within(self, geom_column)
DESCRIPTION:
    Tests if a Geometry value is spatially within another Geometry value.
    Note:
        *  Using a geospatial index can greatly improve the performance of
          queries that use this method.
PARAMETERS:
    geom_column:
        Required Argument.
        Specifies the other Geometry value.
        Types: str, ColumnExpression, GeometryType
SUPPORTED GEOMETRY TYPES:
    All Geometry types except geometry collections.
    Note:
        This method can be called on 3D geometries (those that include Z
        coordinates). However, the Z coordinate is ignored in method
        calculations.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
    Resultant column contains:
        * 1, if the geometry is within other geometry
        * 0, if the geometry is not within other geometry
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only few columns from GeoDataFrame.
    pols_lines = geodf.select(["skey", "polygons", "linestrings"])
    # Example 1: Check whether geometries in column 'linestrings' is spatially within
    #            the geometries in column 'polygons'.
    pols_lines.assign(res = pols_lines.linestrings.within(geom_column=pols_lines.polygons))
    # Example 2: Check whether geometries in column 'linestrings' is spatially within
    #            the geometries specified by Polygon geometry type.
    # Create an object of Polygon GeometryType.
    po1 = Polygon([(0, 0), (0, 20), (20, 20), (20, 0), (0, 0)])
    # Pass the Polygon() GeometryType object as input to "geom_column" argument.
    pols_lines.assign(res = pols_lines.linestrings.within(geom_column=po1))
    # Example 3: Filter all rows where geometries in column 'linestrings' is spatially within
    #            the geometries in column 'polygons'.
    pols_lines[pols_lines.linestrings.within(geom_column=pols_lines.polygons) == 1]
wkb_geom_to_sql
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.wkb_geom_to_sql = wkb_geom_to_sql(self, column)
DESCRIPTION:
    Returns a specified Geometry value.
    Note:
        *  Vantage provides this method because it is in the SQL/MM
          Spatial standard, where it is defined to implement transform
          functionality that allows importing the Geometry type to the server.
PARAMETERS:
    column:
        Required Argument.
        Specifies the geometry value specified in the well-known binary (WKB) format.
        The data type of "column" is BLOB. The maximum size of "column" is
        approximately 16 MB (the exact maximum is 16 MB - 1 KB), allowing
        for a representation of approximately one million points. For more
        information on WKB formats, see Well-Known Binary Format in SQL documentation.
        Types: str, ColumnExpression
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Get the WKB geometry in SQL format.
    # Let's select only few columns from GeoDataFrame.
    geodfbin = geodf.select(["skey", "points"]).assign(str_column=geodf.points.to_binary())
    geodfbin.assign(res = geodfbin.points.wkb_geom_to_sql(column="str_column"))
wkt_geom_to_sql
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.wkt_geom_to_sql = wkt_geom_to_sql(self, column)
DESCRIPTION:
    Returns a specified Geometry value.
    Notes:
        *  Vantage provides this method because it is in the SQL/MM
          Spatial standard, where it is defined to implement transform
          functionality that allows importing the Geometry type to the server.
PARAMETERS:
    column:
        Required Argument.
        Specifies the geometry value specified in the well-known text (WKT) format.
        The data type of "column" is CLOB. The maximum size of "column" is
        approximately 16 MB (the exact maximum is 16 MB - 1 KB), allowing
        for a representation of approximately one million points. For more
        information on WKT formats, see Well-Known Text Format in SQL documentation.
        Types: str, ColumnExpression
SUPPORTED GEOMETRY TYPES:
    All Geometry types.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Get the WKT geometry in SQL format.
    # Let's select only few columns from GeoDataFrame.
    geodftxt = geodf.select(["skey", "points"]).assign(str_column=geodf.points.to_text())
    geodftxt.assign(res = geodftxt.points.wkt_geom_to_sql(column="str_column"))
Methods for Point Geometry
spherical_buffer
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.spherical_buffer = spherical_buffer(self, distance, radius=6371000.0)
DESCRIPTION:
    Returns an MBR that represents the minimum and maximum latitude and
    longitude of all points within a given distance from a point. The earth
    is modeled as a sphere.
    Notes:
        *  spherical_buffer() and spheroidal_buffer() basically perform
          the same function, but with different performance and accuracy.
          spherical_buffer() models the earth as a simple sphere, so it
          performs better than spheroidal_buffer(), but with less accuracy.
          spheroidal_buffer() models the earth as a spheroid, so it returns
          a more accurate MBR, but it runs slower than spherical_buffer().
        *  The returned MBR cannot cross the longitude value of +/-180 or the
          latitude value of +/-90. The MBR cannot cover more than 180 degrees
          of latitude or longitude on the sphere or spheroid.
PARAMETERS:
    distance:
        Required Argument.
        Specifies the distance, in meters, from the point to
        calculate the MBR.
        Types: float, int, str, ColumnExpression
    radius:
        Optional Argument.
        Specifies the radius of the sphere used to model the earth.
        If omitted, the method uses a value of 6371000.0 meters.
        Default Value: 6371000.0
        Types: float, int, str, ColumnExpression
SUPPORTED GEOMETRY TYPES:
    ST_Point
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only few columns from GeoDataFrame.
    points_df = geodf.select(["skey", "points"])[geodf.skey.isin([1001, 1002, 1003])]
    # Example 1: Get an MBR that represents the minimum and maximum latitude and longitude of
    #            all points within a given distance of 2.8 from a ST_Point in columns 'points'
    #            with default values for other arguments.
    points_df.assign(res = points_df.points.spherical_buffer(distance=2.8))
    # Example 2: Get an MBR that represents the minimum and maximum latitude and longitude of
    #            all points within a given distance of 2.8 and radius as 6371000 from a
    #            ST_Point in columns 'points' with default values for rest of the arguments.
    points_df.assign(res = points_df.points.spherical_buffer(distance=2.8, radius=6371000))
spherical_distance
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.spherical_distance = spherical_distance(self, geom_column)
DESCRIPTION:
    Returns the spherical distance between two spherical coordinates on the
    planet using the Haversine Formula. Both coordinates must be specified
    as ST_Point values.
PARAMETERS:
    geom_column:
        Required Argument.
        Specifies a Geometry for the other spherical coordinate.
        Types: str, ColumnExpression, GeometryType
SUPPORTED GEOMETRY TYPES:
    ST_Point
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only few columns from GeoDataFrame and join the GeoDataFrameColumn to self.
    points_df = geodf.select(["skey", "points"])[geodf.skey.isin([1001, 1002, 1003])]
    points_df = points_df.join(points_df, how="cross", lsuffix="l", rsuffix="r")
    # Example 1: Get the spherical distance between two spherical coordinates on the planet using
    #            the Haversine Formula, where coordinates are in columns 'l_point' and 'r_point'.
    points_df.assign(res = points_df.l_points.spherical_distance(points_df.r_points))
    # Example 2: Get the spherical distance between the spherical coordinates on the planet using
    #            the Haversine Formula, where set of points is in column "point" and another geometry
    #            defined by the Point object.
    # Create an object of Point GeometryType.
    p1 = Point(1,3)
    # Pass the Point() GeometryType object as input to "geom_column" argument.
    points_df.assign(res = points_df.l_points.spherical_distance(geom_column=p1))
spheroidal_buffer
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.spheroidal_buffer = spheroidal_buffer(self, distance, semimajor=6378137.0,
invﬂattening=298.257223563)
DESCRIPTION:
    Returns an MBR that represents the minimum and maximum latitude and
    longitude of all points within a given distance from the point. The
    earth is modeled as a spheroid.
    Notes:
        *  For the MBR calculation, the planet is modeled as a spheroid. If you
          do not pass in values for the "semimajor" and "invflattening" arguments,
          the computation uses the semimajor axis and the inverse flattening
          ratio from the World Geodetic System, WGS84. A value of 6,378,137.0
          meters is used for the semimajor axis, and a value of 298.257223563
          is used for the inverse flattening ratio.
        *  spherical_buffer() and spheroidal_buffer() perform a similar
          function, but with different performance and accuracy.
          spherical_buffer() models the earth as a simple sphere, so it
          performs better than spheroidal_buffer(), but with less accuracy.
          spheroidal_buffer() models the earth as a spheroid, so it returns
          a more accurate MBR, but it runs slower than spherical_buffer().
        *  The returned MBR cannot cross the longitude value of +/-180 or the
          latitude value of +/-90. The MBR cannot cover more than 180 degrees
          of latitude or longitude on the sphere or spheroid.
PARAMETERS:
    distance:
        Required Argument.
        Specifies the distance, in meters, from the point to
        calculate the MBR.
        Types: float, int, str, ColumnExpression
    semimajor:
        Optional Argument.
        Specifies the length, in meters, of the semimajor axis of
        the spheroid. If omitted, the method uses the WGS84 value of
        6,378,137.0 meters.
        Default Value: 6378137.0
        Types: float, int
    invflattening:
        Optional Argument.
        Specifies the inverse flattening ratio of the spheroid. If
        omitted, the method uses the WGS84 value of 298.257223563.
        Default Value: 298.257223563
        Types: float, int
SUPPORTED GEOMETRY TYPES:
    ST_Point
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only few columns from GeoDataFrame.
    points_df = geodf.select(["skey", "points"])[geodf.skey.isin([1001, 1002, 1003])]
    # Example 1: Get an MBR that represents the minimum and maximum latitude and longitude of
    #            all points within a given distance of 2.8 from a ST_Point in columns 'points'
    #            with default values for other arguments.
    points_df.assign(res = points_df.points.spheroidal_buffer(distance=2.8))
    # Example 2: Get an MBR that represents the minimum and maximum latitude and longitude of
    #            all points within a given distance of 2.8, semimajor as 637813 and invflattening
    #            as 298 from a ST_Point in columns 'points'.
    points_df.assign(res = points_df.points.spheroidal_buffer(distance=2.8, semimajor=637813, invflattening=298))
spheroidal_distance
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.spheroidal_distance = spheroidal_distance(self, geom_column, semimajor=6378137.0,
invﬂattening=298.257223563)
DESCRIPTION:
    Returns the distance, in meters, between two spherical coordinates.
    Notes:
        *  For the distance calculation, the planet is modeled as a spheroid.
        *  If you do not pass in values for the "semimajor" and "invflattening"
          arguments, the computation uses the semimajor axis and the inverse
          flattening ratio from the World Geodetic System, WGS84. A value of
          6,378,137.0 meters is used for the semimajor axis, and a value of
          298.257223563 is used for the inverse flattening ratio.
        *  Distance calculations between antipodal or nearly antipodal points
          using the spheroidal_distance() method may fail to converge,
          generating a Vantage error. For these distance
          calculations use instead the spherical_distance() method.
PARAMETERS:
    geom_column:
        Required Argument.
        Specifies a Geometry for the other coordinate.
        Types: str, ColumnExpression, GeometryType
    semimajor:
        Optional Argument.
        Specifies the length in meters of the semimajor axis of the
        spheroid.
        Default Value: 6378137.0
        Types: float, int
    invflattening:
        Optional Argument.
        Specifies the inverse flattening ratio of the spheroid.
        Default Value: 298.257223563
        Types: float, int
SUPPORTED GEOMETRY TYPES:
    ST_Point
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only few columns from GeoDataFrame.
    points_df = geodf.select(["skey", "points"])[geodf.skey.isin([1001, 1002, 1003])]
    points_df = points_df.join(points_df, how="cross", lsuffix="l", rsuffix="r")
    # Example 1: Get the spheroidal distance between two points from columns 'l_points' and 'r_points'.
    points_df.assign(res = points_df.l_points.spheroidal_distance(points_df.r_points))
    # Example 2: Get the spheroidal distance between points from column 'l_points' and
    #            a GeometryType object Point.
    # Create an object of Point GeometryType.
    p1 = Point(1,3)
    # Pass the Point() GeometryType object as input to "geom_column" argument.
    points_df.assign(res = points_df.l_points.spheroidal_distance(geom_column=p1, semimajor=637813, invflattening=298))
set_x
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.set_x = set_x(self, xcoord)
DESCRIPTION:
    Set the X coordinate of an ST_Point value.
PARAMETERS:
    xcoord:
        Required Argument.
        Specifies a value to the new X coordinate of the ST_Point value.
        Types: float, int, str, ColumnExpression
SUPPORTED GEOMETRY TYPES:
    ST_Point
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Set the X coordinate of the ST_Point geometries in column 'points'.
    # Let's select only few columns from GeoDataFrame.
    points_df = geodf.select(["skey", "points"])[geodf.skey.isin([1001, 1002, 1003])]
    points_df.assign(res = points_df.points.set_x(xcoord=1000))
set_y
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.set_y = set_y(self, ycoord)
DESCRIPTION:
    Set the Y coordinate of an ST_Point value.
PARAMETERS:
    ycoord:
        Required Argument.
        Specifies a value to the new Y coordinate of the ST_Point value.
        Types: float, int, str, ColumnExpression
SUPPORTED GEOMETRY TYPES:
    ST_Point
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Set the Y coordinate of the ST_Point geometries in column 'points'.
    # Let's select only few columns from GeoDataFrame.
    points_df = geodf.select(["skey", "points"])[geodf.skey.isin([1001, 1002, 1003])]
    points_df.assign(res = points_df.points.set_y(ycoord=1000))
set_z
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.set_z = set_z(self, zcoord)
DESCRIPTION:
    Set the Z coordinate of an ST_Point value.
    Note:
        *  This method can be used to convert a 2D point to a 3D point if a
          zcoord argument is passed.
PARAMETERS:
    zcoord:
        Required Argument.
        Specifies a value to the new Z coordinate of the ST_Point value.
        Types: float, int, str, ColumnExpression
SUPPORTED GEOMETRY TYPES:
    ST_Point
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Set the Z coordinate of the ST_Point geometries in column 'points'.
    # Let's select only few columns from GeoDataFrame.
    points_df = geodf.select(["skey", "points"])[geodf.skey.isin([1001, 1002, 1003])]
    points_df.assign(res = points_df.points.set_z(zcoord=1000))
Methods for LineString Geometry
end_point
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.end_point = end_point(self)
DESCRIPTION:
    Returns the end point of an ST_LineString or GeoSequence value.
    Note:
        *  This method returns a point having a Z coordinate if the input
          geometry is three-dimensional.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    ST_LineString and GeoSequence
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
    Resultant column contains a NULL, if the Geometry is an empty set.
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Get the end points of (ST_Linestring) geometries in column
    #            'linestrings'.
    # Process the GeoDataFrameColumn to select only two columns, skey and linestrings and filter the
    # rows where geometry type is ST_Linestring.
    lines = geodf.select(["skey", "linestrings"])[geodf.linestrings.geom_type == "ST_Linestring"]
    ep_expr = lines.geometry.end_point()
    lines.assign(res = ep_expr)
    # Example 2: Filter the rows where the end points of (ST_Linestring) geometries in column
    #            'linestrings' is same as the specified Point GeometryType object.
    # Create an object of Point GeometryType.
    p1 = Point(0, 1)
    # Execute the 'end_point()' on GeoDataFrameColumn pointed by geometry property and pass
    # the Point object created above to the slice filter.
    lines[lines.geometry.end_point() == p1]
length
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.length = length(self)
DESCRIPTION:
    Returns the length measurement of a Geometry type that represents an
    ST_LineString, GeoSequence, or ST_MultiLineString value.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    ST_LineString, GeoSequence, and ST_MultiLineString
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Get the length of ST_Linestring/ST_MultiLinestring geometries
    #            in column 'linestrings'.
    # Let's select only few columns from GeoDataFrame.
    lines = geodf.select(["skey", "linestrings"])
    lines.assign(res = lines.geometry.length())
    # Example 2: Filter all rows where the length of ST_Linestring/ST_MultiLinestring geometries
    #            in column 'linestrings' is less than or equal to 10.
    # Let's select only few columns from GeoDataFrame.
    lines = geodf.select(["skey", "linestrings"])
    lines[lines.geometry.length() <= 10]
length_3D
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.length_3D = length_3D(self)
DESCRIPTION:
    Returns the length of a 3D LineString or MultiLineString, taking into
    account the Z coordinates in the calculation.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    LineString and MultiLineString types that include Z coordinates
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
    Resultant column contains a 0, if the LineString or MultiLineString is an empty set.
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Get the length of 3D ST_Linestring/ST_MultiLinestring geometries
    #            in column 'linestrings'.
    # Let's select only few columns from GeoDataFrame.
    lines = geodf.select(["skey", "linestrings"])[geodf.linestrings.is_3D == 1]
    lines.assign(res = lines.geometry.length_3D())
    # Example 2: Filter all rows where the length of 3D ST_Linestring/ST_MultiLinestring geometries
    #            in column 'linestrings' is less than or equal to 10.
    lines[lines.geometry.length() <= 10]
line_interpolate_point
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.line_interpolate_point = line_interpolate_point(self, proportion)
DESCRIPTION:
    Returns a point interpolated along a Geometry type that represents an
    ST_LineString or GeoSequence value, given a proportional distance along
    that line.
PARAMETERS:
    proportion:
        Required Argument.
        Specifies a value between 0 and 1 that represents the fraction of the
        total 2-dimensional length of the ST_LineString where the point must
        be located.
        Types: float, str, ColumnExpression
SUPPORTED GEOMETRY TYPES:
    ST_LineString and GeoSequence
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn containing ST_Point Geometry values
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Get the point interpolated along the ST_Linestring geometries
    #            in column 'linestrings'.
    # Let's select only few columns from GeoDataFrame and filter rows which contains
    # only ST_Linestring geometries.
    lines = geodf.select(["skey", "linestrings"])[geodf.linestrings.geom_type == "ST_Linestring"]
    lines.assign(res = lines.geometry.line_interpolate_point(proportion=0.28))
    # Example 2: Filter the rows where the point interpolated along the ST_Linestring geometries
    #            in column 'linestrings' is same as the specified Point GeometryType object.
    # Create an object of Point GeometryType.
    p1 = Point(1.84, 1.84)
    lines[lines.geometry.line_interpolate_point(proportion=0.28) == p1]
num_points
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.num_points = num_points(self)
DESCRIPTION:
    Returns the number of points in a Geometry type that represents an
    ST_LineString or GeoSequence value.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    ST_LineString and GeoSequence
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Get the number of points in a geometry in column 'linestrings'.
    # Let's select only few columns from GeoDataFrame and rows containing ST_Linestrings geometries only.
    lines = geodf.select(["skey", "linestrings"])[geodf.linestrings.geom_type == "ST_Linestring"]
    lines.assign(res = lines.linestrings.num_points())
point
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.point = point(self, position)
DESCRIPTION:
    Returns the specified point from a Geometry type that represents an
    ST_LineString or GeoSequence value.
    Note:
        *  This method returns a point having a Z coordinate if the input
          geometry is three-dimensional.
PARAMETERS:
    position:
        Required Argument.
        Specifies a value for the requested point, where the position of the
        first point is 1.
        Types: int, str, ColumnExpression
SUPPORTED GEOMETRY TYPES:
    ST_LineString and GeoSequence
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn containing Geometry values
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only few columns from GeoDataFrame, also filter rows containing
    # only ST_Linestring geometries.
    lines = geodf.select(["skey", "linestrings"])[geodf.linestrings.geom_type == "ST_Linestring"]
    # Example 1: Get the point specified at 1st position in ST_LineString geometries in column 'linestrings'.
    lines.assign(res = lines.linestrings.point(1))
    # Example 2: Get the point specified at 2nd position in ST_LineString geometries in column 'linestrings'.
    lines.assign(res = lines.linestrings.point(position=2))
    # Example 3: Filter the rows where the point specified at 1st position in ST_LineString geometries
    #            in column 'linestrings' is same as the specified Point GeometryType object.
    # Create an object of Point GeometryType.
    p1 = Point(1, 1)
    lines[lines.linestrings.point(1) == p1]
start_point
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.start_point = start_point(self)
DESCRIPTION:
    Returns the start point of an ST_LineString or GeoSequence value.
    Note:
        *  This method returns a point having a Z coordinate if the input
          geometry is three-dimensional.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    ST_LineString and GeoSequence
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
    Resultant column contains a NULL, if the Geometry is an empty set.
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Get the start points of (ST_Linestring) geometries in column
    #            'linestrings'.
    # Process the GeoDataFrameColumn to select only two columns, skey and linestrings and filter the
    # rows where geometry type is ST_Linestring.
    lines = geodf.select(["skey", "linestrings"])[geodf.linestrings.geom_type == "ST_Linestring"]
    lines.assign(res = lines.linestrings.start_point())
    # Example 2: Filter the rows where the start points of (ST_Linestring) geometries in column
    #            'linestrings' is same as the specified Point GeometryType object.
    # Create an object of Point GeometryType.
    p1 = Point(1, 1)
    lines[lines.linestrings.start_point() == p1]
Methods for Polygon Geometry
interiors
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.interiors = interiors(self, position)
DESCRIPTION:
    Returns the specified interior ring of a Geometry type that represents
    an ST_Polygon value.
PARAMETERS:
    position:
        Required Argument.
        Specifies a value for the requested interior ring, where the
        position of the first interior ring is 1.
        Types: int, str, ColumnExpression
SUPPORTED GEOMETRY TYPES:
    ST_Polygon
    Note:
        This method can be called on 3D geometries (those that include Z
        coordinates). However, the Z coordinate is ignored in method
        calculations. Consequently, any Z coordinates returned by this
        method should be ignored. Teradata recommends using the make_2D()
        method to strip out the Z coordinates of the return value.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn containing ST_LineString Geometry values
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Get the interior ring specified at 1st index for polygons.
    # Filter the data to select few rows and couple of columns (skey and polygons).
    polygons = geodf.select(["skey", "polygons"])[geodf.skey.isin([1002, 1004])]
    geo_interiors = polygons.geometry.interiors(position=1)
    polygons.assign(res = geo_interiors)
    # Example 2: Filter the rows where the interior ring specified at 1st index for polygons
    #            is same as the specified LineString GeometryType object.
    # Create an object of LineString GeometryType.
    l1 = LineString([(5, 5), (5, 10), (10, 10), (10, 5), (5, 5)])
    # Execute the interiors() function on 'polygons' column.
    geo_interiors = polygons.geometry.interiors(position=1)
    polygons[geo_interiors == l1]
num_interior_ring
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.num_interior_ring = num_interior_ring(self)
DESCRIPTION:
    Returns the number of interior rings of a Geometry type that represents
    an ST_Polygon value.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    ST_Polygon
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Get the number of interior rings of ST_Polygon geometries in column 'polygons'.
    # Let's select only few columns from GeoDataFrame and rows containing ST_Polygon geometries only.
    pols = geodf.select(["skey", "polygons"])[geodf.polygons.geom_type == "ST_Polygon"]
    pols.assign(res = pols.polygons.num_interior_ring())
point_on_surface
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.point_on_surface = point_on_surface(self)
DESCRIPTION:
    Returns a point that is guaranteed to spatially intersect an ST_Polygon,
    or at least one of the component polygons of an ST_MultiPolygon.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    ST_Polygon and ST_MultiPolygon
    Note:
        This method can be called on 3D geometries (those that include Z
        coordinates). However, the Z coordinate is dropped, and only x and y
        coordinates are returned.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn containing Geometry values
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Get the point in polygon geometries that is guaranteed to spatially intersect.
    # Let's select only few columns from GeoDataFrame.
    pols = geodf.select(["skey", "polygons"])[geodf.skey.isin([1001, 1002, 1003])]
    pols.assign(res = pols.polygons.point_on_surface())
Methods for GeometryCollection Geometry
geom_component
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.geom_component = geom_component(self, position)
DESCRIPTION:
    Returns the geometry of one component member of a composite geometry
    type (ST_GeomCollection, ST_MultiPoint, ST_MultiLineString, or
    ST_MultiPolygon). The element to be returned is specified by position
    with the collection.
PARAMETERS:
    position:
        Required Argument.
        Specifies a value representing the position in the collection of the
        geometry to be returned. The position of the first element in the
        collection is number 1.
        Types: int, str, ColumnExpression
SUPPORTED GEOMETRY TYPES:
    ST_GeomCollection, ST_MultiPoint, ST_MultiLineString, and ST_MultiPolygon
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn containing Geometry values
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Get the geometry component at position 1 in ST_GeometryCollection, a composite geometry type,
    #            geometries in column 'geom_collections'.
    geom_collections = geodf.select(["skey", "geom_collections"])
    ep_expr = geom_collections.geometry.geom_component(position=1)
    geom_collections.assign(res = ep_expr)
    # Example 2: Filter the rows where the geometry component at position 1 in ST_GeometryCollection,
    #            a composite geometry type, geometries in column 'geom_collections' is same as the
    #            specified Point GeometryType object.
    # Create an object of Point GeometryType.
    p1 = Point(10, 20, 30)
    # Execute the geom_component() function on 'geom_collections' column.
    ep_expr = geom_collections.geometry.geom_component(position=1)
    geom_collections[ep_expr == p1]
num_geometry
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.num_geometry = num_geometry(self)
DESCRIPTION:
    Returns the number of distinct geometries that constitute a composite
    geometry type (ST_GeomCollection, ST_MultiPoint, ST_MultiLineString, or
    ST_MultiPolygon).
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    ST_GeomCollection, ST_MultiPoint, ST_MultiLineString, and ST_MultiPolygon
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Get the number of distinct geometries that constitute the composite geometry type
    #            ST_GeometryCollection in column 'geom_collections'.
    # Let's select only few columns from GeoDataFrame.
    geomcdf = geodf.select(["skey", "geom_collections"])[geodf.skey.isin([1001, 1004, 1005])]
    geomcdf.assign(res = geomcdf.geom_collections.num_geometry())
    # Example 2: Get all the rows where the number of distinct geometries that constitute the
    #            composite geometry type ST_GeometryCollection in column 'geom_collections' is 3.
    geomcdf[geomcdf.geom_collections.num_geometry() == 3]
Methods for GeoSequence Geometry
clip
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.clip = clip(self, start_timestamp, end_timestamp)
DESCRIPTION:
    Returns a GeoSequence type containing the subset of points and
    associated data that lie between the two timestamp input arguments.
PARAMETERS:
    start_timestamp:
        Required Argument.
        Specifies a TIMESTAMP value as the beginning timestamp.
        Types: str
    end_timestamp:
        Required Argument.
        Specifies a TIMESTAMP value as the ending timestamp.
        Types: str
SUPPORTED GEOMETRY TYPES:
    GeoSequence
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only skey and geosequence columns from GeoDataFrame, as clip() function works
    # with only ST_Geosequence geometry type.
    geoseq = geodf.select(["skey", "geosequence"])
    # Example 1: clip the geometries of type ST_Geosequence in column 'geosequence', within the
    #            timestamp "TIMESTAMP '2007-03-14 01:35:00'" and "TIMESTAMP '2007-03-14 01:35:08'".
    geo_clip = geoseq.geosequence.clip(start_timestamp="TIMESTAMP '2007-03-14 01:35:00'",
    end_timestamp="TIMESTAMP '2007-03-14 01:35:08'")
    geoseq.assign(res = geo_clip)
get_ﬁnal_timestamp
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.get_ﬁnal_timestamp = get_ﬁnal_timestamp(self)
DESCRIPTION:
    Returns the TimeStamp of the last point of a GeoSequence.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    GeoSequence
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Get the timestamp of the last point of a Geosequence geometry in 'geosequence' column.
    # Select 'skey' and 'geosequence' column and filter the rows where geometries are not None.
    geoseq = geodf.select(["skey", "geosequence"])[geodf.geosequence != None]
    geo_gft = geoseq.geosequence.get_final_timestamp()
    geoseq.assign(res = geo_gft)
get_init_timestamp
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.get_init_timestamp = get_init_timestamp(self)
DESCRIPTION:
    Returns the TimeStamp of the first point of a GeoSequence.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    GeoSequence.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Get the timestamp of the first point of a Geosequence geometry in 'geosequence' column.
    # Select 'skey' and 'geosequence' column and filter the rows where geometries are not None.
    geoseq = geodf.select(["skey", "geosequence"])[geodf.geosequence != None]
    geo_git = geoseq.geosequence.get_init_timestamp()
    geoseq.assign(res = geo_git)
get_link
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.get_link = get_link(self, index)
DESCRIPTION:
    Get the link ID of a specified point in a GeoSequence.
PARAMETERS:
    index:
        Required Argument.
        Specifies the index of the point in the GeoSequence, where the index of the
        first point is 1.
        Types: int, str, ColumnExpression
SUPPORTED GEOMETRY TYPES:
    GeoSequence
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Select 'skey' and 'geosequence' column and filter the rows where geometries are not None.
    geoseq = geodf.select(["skey", "geosequence"])[geodf.geosequence != None]
    # Example 1: Get the link ID of the point at 1st position in a Geosequence geometry in 'geosequence' column.
    geo_gl1 = geoseq.geosequence.get_link(1)
    geoseq.assign(res = geo_gl1)
    # Example 2: Get all the rows where the link ID of the point at 2nd position in a Geosequence
    #            geometry in 'geosequence' column is equal to 2.
    geo_gl2 = geoseq.geosequence.get_link(index=2)
    geoseq[geo_gl2 == 2]
get_user_ﬁeld
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.get_user_ﬁeld = get_user_ﬁeld(self, ﬁeld_index, index)
DESCRIPTION:
    Returns the user field specified by "field_index" for the point specified by
    "index" for a GeoSequence type.
PARAMETERS:
    field_index:
        Required Argument.
        Specifies the index of the user field within the point, where the index of the
        first user field is 1.
        Types: int, str, ColumnExpression
    index:
        Required Argument.
        Specifies the index of the point within the GeoSequence type, where the index
        of the first point is 1.
        Types: int, str, ColumnExpression
SUPPORTED GEOMETRY TYPES:
    GeoSequence
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Select 'skey' and 'geosequence' column and filter the rows where geometries are not None.
    geoseq = geodf.select(["skey", "geosequence"])[geodf.geosequence != None]
    # Example 1: Get the 1st user field with respect to the point at index 1 in a Geosequence
    #            geometry in 'geosequence' column.
    geo_guf1 = geoseq.geosequence.get_user_field(1, 1)
    geoseq.assign(res = geo_guf1)
    # Example 2: Get all the rows where the 2nd user field with respect to the point at index 1
    #            in a Geosequence geometry in 'geosequence' column is negative.
    geo_guf2 = geoseq.geosequence.get_user_field(field_index=2, index=1)
    geoseq[geo_guf2 < 0]
get_user_ﬁeld_count
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.get_user_ﬁeld_count = get_user_ﬁeld_count(self)
DESCRIPTION:
    Returns the number of user fields associated with each point of a
    GeoSequence.
PARAMETERS:
    None
SUPPORTED GEOMETRY TYPES:
    GeoSequence
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Get the number of user fields associated to each point in a Geosequence
    #            geometry in 'geosequence' column.
    # Select 'skey' and 'geosequence' column and filter the rows where geometries are not None.
    geoseq = geodf.select(["skey", "geosequence"])[geodf.geosequence != None]
    geo_gufc = geoseq.geosequence.get_user_field_count()
    geoseq.assign(res = geo_gufc)
point_heading
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.point_heading = point_heading(self, index)
DESCRIPTION:
    Returns the heading for the specified point of a GeoSequence.
    The value is calculated as the angle (in degrees) between a vertical
    line and the line segment from the specified point to the next point
    clockwise from the North.
PARAMETERS:
    index:
        Required Argument.
        Specifies the index of the point to return the heading of, where the index of
        the first point in the GeoSequence is 1.
        Types: int, str, ColumnExpression
SUPPORTED GEOMETRY TYPES:
    GeoSequence
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only few columns from GeoDataFrame.
    geoseq = geodf.select(["skey", "geosequence"])[geodf.skey.isin([1001, 1002])]
    # Example 1: Get the heading for the point at 1st index in Geosequence geometries in
    #            "geosequence" column.
    geoseq.assign(res = geoseq.geosequence.point_heading(1))
    # Example 2: Get the heading for the point at 2nd index in Geosequence geometries in
    #            "geosequence" column.
    geoseq.assign(res = geoseq.geosequence.point_heading(index=2))
set_link
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.set_link = set_link(self, index, link_id)
DESCRIPTION:
    Set the link ID of a specified point in a GeoSequence.
PARAMETERS:
    index:
        Required Argument.
        Specifies the index of the point in the GeoSequence, where the index of the
        first point is 1.
        Types: int, str, ColumnExpression
    link_id:
        Required Argument.
        Specifies the new link ID of the specified point in the GeoSequence. The data type
        of linkID is DECIMAL(18,0).
        Types: float
SUPPORTED GEOMETRY TYPES:
    GeoSequence
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Set the link ID for the point at the 1st index in geometries in column 'geosequence'.
    # Let's select only few columns from GeoDataFrame.
    geoseq = geodf.select(["skey", "geosequence"])[geodf.skey.isin([1001, 1002])]
    geoseq.assign(res = geoseq.geosequence.set_link(index=1, link_id=20.2))
speed
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.speed = speed(self, index=None, begin_index=None, end_index=None)
DESCRIPTION:
    Returns the approximate speed at a specified point (speed(index
    INTEGER)) or between two points (speed(iBegin INTEGER, iEnd INTEGER))
    for a GeoSequence type.
    Notes:
        * If "begin_index" and "end_index" arguments are passed, then
          the speed is calculated as the distance between the two points
          (along the LineString) divided by the time between them.
        * If "index" is passed, then if the point is:
            * The first point, the distance between the first and second
              points is divided by the time between them.
            * The last point, the distance between the second to the last
              and last points is divided by the time between them.
            * Any other point, the distance between the previous point
              and the next point (along the LineString) is divided by the
              time difference between the previous point and the next point.
        * The units used for distance are the same units that are used by
          the coordinate system for the GeoSequence value, and the time
          units are in hours.
PARAMETERS:
    index:
        Optional Argument.
        Specifies the index of the point within the GeoSequence type,
        where the index of the first point is 1.
        Default Value: None
        Types: int, str, ColumnExpression
    begin_index:
        Optional Argument.
        Specifies the index of the starting point for which to
        calculate the speed, where the index of the first point in a
        GeoSequence type is 1.
        Default Value: None
        Types: int, str, ColumnExpression
    end_index:
        Optional Argument.
        Specifies the index of the ending point for which to calculate
        the speed, where the index of the first point in a GeoSequence is 1.
        Default Value: None
        Types: int, str, ColumnExpression
SUPPORTED GEOMETRY TYPES:
    GeoSequence
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only few columns from GeoDataFrame.
    geoseq = geodf.select(["skey", "geosequence"])[geodf.skey.isin([1001, 1002])]
    # Example 1: Get the approximate speed at a point specified at 1st index for Geosequence geometries.
    geoseq.assign(res = geoseq.geosequence.speed(index=1))
    # Example 2: Get the approximate speed between points at 1st and 2nd index for Geosequence geometries.
    geoseq.assign(res = geoseq.geosequence.speed(begin_index=1, end_index=2))
Filtering Functions and Methods
intersects_mbb
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.intersects_mbb = intersects_mbb(self, geom_column)
DESCRIPTION:
    Tests whether a 3D geometry spatially intersects a specified MBB.
PARAMETERS:
    geom_column:
        Required Argument.
        Specifies a minimum bounding box.
        Types: str, ColumnExpression, GeometryType
SUPPORTED GEOMETRY TYPES:
    3D Point, 3D MultiPoint, 3D LineString, 3D MultiLineString
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
    Resultant column contains:
        * 1, if the input 3D geometry intersects the MBB argument.
        * 0, if the input 3D geometry does not intersect the MBB argument.
        * Returns an error if the input geometry is not 3D (does not have
          a z coordinate).
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Example 1: Check whether the 3D ST_Linestring/ST_MultiLinestring geometries
    #            in column 'linestrings' intersects with MBB or not.
    # First, let's process the GeoDataFrameColumn to select few columns and filter only 3D geometries
    # in column 'points', then execute GeoDataFrameColumn.mbb() function to get MBB representation of
    # ST_Point/ST_MultiPoint geometries.
    geodfmbb = geodf.select(["skey", "points", "linestrings"])[geodf.points.is_3D == 1].mbb()
    geo_im = geodfmbb.linestrings.intersects_mbb(geom_column=geodfmbb.mbb_points_geom)
    geodfmbb.assign(res = geo_im)
    # Example 2: Filter all the rows where the 3D ST_Linestring/ST_MultiLinestring geometries
    #            in column 'linestrings' intersects with mbb.
    geodfmbb[geo_im == 1]
mbb_ﬁlter
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.mbb_ﬁlter = mbb_ﬁlter(self, geom_column)
DESCRIPTION:
    Tests whether the MBBs of two 3D geometries spatially intersect.
PARAMETERS:
    geom_column:
        Required Argument.
        Specifies a 3D Geometry.
        Types: str, ColumnExpression, GeometryType
SUPPORTED GEOMETRY TYPES:
    All 3D Geometry types.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
    Resultant column contains:
        * 1, if the MBB of the input 3D geometry intersects the MBB
          of the geometry passed to the method.
        * 0, if the MBB of the input 3D geometry does not intersect
          the MBB of the geometry passed to the method.
        * Returns an error if either geometry is not 3D (does not
          have a z coordinate).
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only few columns from GeoDataFrame.
    points_lines = geodf[geodf.skey.isin([1004, 1005, 1006])].select(["skey", "points", "linestrings"])
    # Example 1: Check whether MBBs of geometries in column 'points' and 'lienstrings'
    #            spatially intersect or not.
    points_lines.assign(res = points_lines.points.mbb_filter(points_lines.linestrings))
    # Example 2: Check whether MBBs of geometries in column 'linestrings' and GeometryType object
    #            Linestring spatially intersect or not.
    # Create an object of Linestring GeometryType.
    l1 = LineString([(1,3,6), (3,0,6), (6,0,1)])
    # Pass the LineString() GeometryType object as input to "geom_column" argument.
    points_lines.assign(res = points_lines.linestrings.mbb_filter(l1))
mbr_ﬁlter
teradataml.geospatial.geodataframecolumn.GeoDataFrameColumn.mbr_ﬁlter = mbr_ﬁlter(self, geom_column)
DESCRIPTION:
    Tests whether the MBRs of two 2D geometries spatially intersect.
PARAMETERS:
    geom_column:
        Required Argument.
        Specifies a 2D Geometry.
        Types: str, ColumnExpression, GeometryType
SUPPORTED GEOMETRY TYPES:
    All 2D Geometry types.
    Although mbr_filter() will accept 3D geometries, the Z coordinates
    are ignored in the calculations.
RAISES:
    TypeError, ValueError, TeradataMlException
RETURNS:
    GeoDataFrameColumn
    Resultant column contains:
        * 1, if the MBR of the input geometry intersects the
          MBR of the geometry passed to the method.
        * 0, if the MBR of the input geometry does not intersect
          the MBR of the geometry passed to the method.
EXAMPLES:
    from teradataml import GeoDataFrame, load_example_data
    from teradataml import Point, LineString, Polygon, GeometryCollection
    # Load example data.
    load_example_data("geodataframe", "sample_shapes")
    # Create a GeoDataFrame.
    geodf = GeoDataFrame("sample_shapes")
    print(geodf)
    # Let's select only few columns from GeoDataFrame.