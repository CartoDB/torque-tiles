# TileCubes Specification

TileCubes is a specification for transferring temporal or categorical geospatial data. 

# Versions

* **Development** - [1.0](https://github.com/cartodb/tilecubes/blob/master/1.0/spec.md)

# Changelog

## Roadmap

 * 

## 1.0

* `name='format'` row **required** in `metadata` table.
* `name='bounds'` row suggested in `metadata` table.
* optional UTFGrid-based interaction spec.

# Concept

TileCubes utilizes the client side resolution for rendering to optimize the transfer of multidimensional data for maps. 

# Implementations

* GBIF Link
* CartoDB Link

# Authors

* Andrew Hill (andrewxhill)
* Javier Santana (javisantana)
* Javier de la Torre (jatorre)
* Sandro Santilli (strk)
* Tim Robertson (timrobertson100)