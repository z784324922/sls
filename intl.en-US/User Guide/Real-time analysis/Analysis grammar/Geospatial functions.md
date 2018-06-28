# Geospatial functions {#reference_zbk_pmq_zdb .reference}

## Geospatial concept {#section_rdy_klv_tdb .section}

Geospatial functions support the geometries in the Well-Known Text \(WKT\) format.

|Geometry|Well-maid text \(WKT\) Format|
|:-------|:----------------------------|
|Point| `POINT (0 0)` |
|Line string| `LINESTRING (0 0, 1 1, 1 2)` |
|Polygon| `Polygon` |
|Multi-point| `MULTIPOINT (0 0, 1 2)` |
|Multi-line string| `MULTILINESTRING ((0 0, 1 1, 1 2), (2 3, 3 2, 5 4))` |
|Multi-polygon| `MULTIPOLYGON (((0 0, 4 0, 4 4, 0 4, 0 0), (1 1, 2 1, 2 2, 1 2, 1 1)), ((-1 -1, -1 -2, -2 -2, -2 -1, -1 -1)))` |
|Geometry collection| `GEOMETRYCOLLECTION (POINT(2 3), LINESTRING (2 3, 3 4))` |

## Constructors {#section_jdf_plv_tdb .section}

|Function|Description |
|:-------|:-----------|
|ST\_Point\(double, double\) → Point|Returns a geometry type point with the given coordinate values.|
|ST\_LineFromText\(varchar\) → LineString|Returns a geometry type line string from WKT representation.|
|ST\_Polygon\(varchar\) → Polygon|Returns a geometry type polygon from WKT representation.|
|ST\_GeometryFromText\(varchar\) → Geometry|Returns a geometry type object from WKT representation.|
|ST\_AsText\(Geometry\) → varchar|Returns the WKT representation of the geometry.|

## Operations {#section_lnx_tlv_tdb .section}

|Function|Description|
|:-------|:----------|
|ST\_Boundary\(Geometry\) → Geometry|Returns the closure of the combinatorial boundary of this geometry.|
|ST\_Buffer\(Geometry, distance\) → Geometry|Returns the geometry that represents all points whose distance from the specified geometry is less than or equal to the specified distance.|
|ST\_Difference\(Geometry, Geometry\) → Geometry|Returns the geometry value that represents the point set difference of the given geometries.|
|ST\_Envelope\(Geometry\) → Geometry|Returns the bounding rectangular polygon of a geometry.|
|ST\_ExteriorRing\(Geometry\) → Geometry|Returns a line string representing the exterior ring of the input polygon.|
|ST\_Intersection\(Geometry, Geometry\) → Geometry|Returns the geometry value that represents the point set intersection of two geometries.|
|ST\_SymDifference\(Geometry, Geometry\) → Geometry|Returns the geometry value that represents the point set  symmetric difference of two geometries.|

## Relationship tests {#section_fcc_ylv_tdb .section}

|Function|Description|
|:-------|:----------|
|ST\_Contains\(Geometry, Geometry\) → boolean|Returns true if and only if no points of the second geometry lie in the exterior of the first geometry, and at least one point of the interior of the first geometry lies in the interior of the second geometry.  Returns false if the two geometries at least share an interior point.|
|ST\_Crosses\(Geometry, Geometry\) → boolean|Returns true if the supplied geometries have some, but not all, interior points in common.|
|ST\_Disjoint\(Geometry, Geometry\) → boolean|Returns true if the given geometries do not spatially intersect.|
|ST\_Equals\(Geometry, Geometry\) → boolean|Returns true if the given geometries represent the same geometry.|
|ST\_Intersects\(Geometry, Geometry\) → boolean|Returns true if the given geometries spatially intersect in two dimensions \(share any portion of space\) and false if they do not \(they are disjoint\).|
|ST\_Overlaps\(Geometry, Geometry\) → boolean|Returns true if the given geometries share space, are of the same dimension, but are not completely contained by each other.|
|ST\_Relate\(Geometry, Geometry\) → boolean|Returns true if the first geometry is spatially related to the second geometry.|
|ST\_Touches\(Geometry, Geometry\) → boolean|Geometry\) → boolean Returns true if the given geometries have at least one point in common, but their interiors do not intersect.|
|ST\_Within\(Geometry, Geometry\) → boolean|Returns true if the first geometry is completely inside the second geometry.  Returns false if the two geometries have at least one point in common.|

## Accessors {#section_u33_zlv_tdb .section}

|Function|Description|
|:-------|:----------|
|ST\_Area\(Geometry\) → double|Returns the area of a polygon using Euclidean measurement on a two dimensional plane in projected units.|
|ST\_Centroid\(Geometry\) → Geometry|Returns the point value that is the mathematical centroid of a geometry.|
|ST\_CoordDim\(Geometry\) → bigint|Returns the coordinate dimension of the geometry.|
|ST\_Dimension\(Geometry\) → bigint|Returns the inherent dimension of this geometry, which must be less than or equal to the coordinate dimension.|
|ST\_Distance\(Geometry, Geometry\) → double|Returns the minimum distance between two geometries.|
|ST\_IsClosed\(Geometry\) → boolean|Returns true if the start and end points of the line string are coincident.|
|ST\_IsEmpty\(Geometry\) → boolean|Returns true if this geometry is an empty geometry collection, polygon, or point.|
|ST\_IsRing\(Geometry\) → boolean|Returns true if and only if the line is closed and simple.|
|ST\_Length\(Geometry\) → double|Returns the length of a line string or multi-line string  using Euclidean measurement on a two  dimensional plane \(based on spatial ref\) in projected units.|
|ST\_XMax\(Geometry\) → double| Returns Y maxima of a bounding box of a geometry.|
|ST\_YMax\(Geometry\) → double|Returns Y maxima of a bounding box of a geometry.|
|T\_XMin\(Geometry\) → double|Returns X minima of a bounding box of a geometry.|
|ST\_YMin\(Geometry\) → double|Returns Y minima of a bounding box of a geometry.|
|ST\_StartPoint\(Geometry\) → point| Returns the first point of a line string geometry.|
|ST\_EndPoint\(Geometry\) → point|Returns the last point of a line string geometry.|
|ST\_X\(Point\) → double|Returns the X coordinate of the point.|
|ST\_Y\(Point\) → double|Returns the Y coordinate of the point.|
|ST\_NumPoints\(Geometry\) → bigint|Returns the number of points in a geometry.|
|ST\_NumInteriorRing\(Geometry\) → bigint|Returns the number of interior rings of a polygon.|

