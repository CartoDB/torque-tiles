# TileCubes 0.9

## Abstract

TileCubes are JSON representations of multidimensional data with geospatial coordinates. TileCubes are broken into two document types, the Metadata and the Tiles. The Metadata document describes the shared information across thee TileCube dataset. For each tile requested there is a Tile document returned that describes the data on that tile. 

TileCubes are referenced on the [Tile Map Service Specification](http://wiki.osgeo.org/wiki/Tile_Map_Service_Specification) and are currently restricted to [global-mercator](http://wiki.osgeo.org/wiki/Tile_Map_Service_Specification#global-mercator). Coordinates within a TileCube are measured in pixel offsets from the boarder and assume 256x256 pixel tiles. 

## Metadata

The TileCube Metadata document describes key tileset information, include zoom extents, pixel resolution, dimension names, and authors.

TODO

#### Schema

TODO

## Tiles

Tiles are required to contain a core set of information to be rendered, that information includes the number of pixels with data, and the x and y values for each pixel. From that core schema, TileCubes can be extended to facilitate more advanced datasets

### Single dimension with values

#### Schema

The single dimension extends the core tile with pixel based keys. 

##### Example

```json
[
  [
    {
      x: 243,
      y: 246,
      keys: [2010, 2011, 2013]
    }    
  ],
  [
    {
      x: 12,
      y: 5,
      keys: [1978]
    }    
  ],
  [
    {
      x: 27,
      y: 122,
      keys: [1983, 1984]
    }    
  ]  
]
```

### Key-value tiles

#### Schema

It is also possible to extend this, and encode key value pairs into a single tile.  
This might be used to (e.g.) provide the number of events per month (at each pixel)

```json
[
  [
    {
      x: 243,
      y: 246,
      keys: [2010, 2011, 2013],
      values: [12, 3, 55]
    }    
  ],
  [
    {
      x: 12,
      y: 5,
      keys: [1978],
      values: [24]
    }    
  ],
  [
    {
      x: 27,
      y: 122,
      keys: [1983, 1984],
      values: [11, 1]
    }    
  ]  
]
```

