# TileCubes 1.0

## Abstract

TileCubes are JSON representations of multidimensional data with geospatial coordinates. TileCubes are broken into two document types, the [Metadata][Metadata] and the [Tiles][Tiles]. The Metadata document describes the shared information across thee TileCube dataset. For each tile requested there is a Tile document returned that describes the data on that tile. 

TileCubes are referenced on the [Tile Map Service Specification](http://wiki.osgeo.org/wiki/Tile_Map_Service_Specification) and are currently restricted to [global-mercator](http://wiki.osgeo.org/wiki/Tile_Map_Service_Specification#global-mercator). Coordinates within a TileCube are measured in pixel offsets from the boarder and assume 256x256 pixel tiles. 

## Metadata

The TileCube Metadata document describes key tileset information, include zoom extents, pixel resolution, dimension names, and authors.

#### Schema

## Tiles

Tiles are required to contain a core set of information to be rendered, that information includes the number of pixels with data, and the x and y values for each pixel. From that core schema, TileCubes can be extended to facilitate more advanced datasets

#### Core tile

```json
[
	4,
	1, 4, 5, 9,
	4, 5, 1, 3
]
```

### Single dimension tiles

#### Schema

#### Content

### Multi dimension tiles

#### Schema

#### Content
