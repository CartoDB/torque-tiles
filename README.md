# TileCubes Specification

TileCubes is a specification for transferring temporal or categorical
geospatial data. TileCubes have been used in a number of production
projects since 2011 without public documentation or description of their
implementation. This specification is meant to allow any data service
provider to begin serving TileCubes for rendering data on the web.

# Versions

* **Development** - [1.0]
  (https://github.com/andrewxhill/tilecubes/blob/master/1.0/spec.md)

# Changelog

## Roadmap

 * specification
 * client
 * examples
 * interactivity
 * flexible dimension number
   * In the future, data can be multidimensional
     (e.g. Month:Gender:AgeGroup), however V1.x is limited to Key:Value
     (e.g. Month:Gender) and Key:Value repetitions (e.g. Month:Gender,
     Month:AgeGroup)

## 1.0



# Concept

TileCubes utilizes the client side resolution for rendering to optimize
the transfer of multidimensional data for maps.

# Implementations

* GBIF
* [CartoDB](http://cartodb.com)'s [Torque](https://github.com/CartoDB/torque) -- temporal maps

# Authors

* Andrew Hill (andrewxhill)
* Javier Santana (javisantana)
* Javier de la Torre (jatorre)
* Sandro Santilli (strk)
* Tim Robertson (timrobertson100)

The authors draw upon two existing specifications to guide the
development and publication of TileCubes:

* [MBTiles](https://github.com/mapbox/mbtiles-spec) Tile Map
* [Tile Map Service](http://wiki.osgeo.org/wiki/Tile_Map_Service_Specification)
