# TorqueTiles 2.0

## Abstract

TorqueTiles are JSON representations of multidimensional data with
geospatial coordinates. TorqueTiles are broken into two document types,
the Metadata and the Tiles. The Metadata document describes the shared
information across the TileCube dataset. For each tile requested there
is a Tile document returned that describes the data on that tile.

TorqueTiles are referenced on the [Tile Map Service Specification]
(http://wiki.osgeo.org/wiki/Tile_Map_Service_Specification)
and are currently restricted to [global-mercator]
(http://wiki.osgeo.org/wiki/Tile_Map_Service_Specification#global-mercator).
Coordinates within a TileCube are measured in pixel offsets from the
border and assume 256x256 pixel tiles.


## Metadata

The TorqueMap Metadata document describes key tileset information, it includes:

- **translate**: time range span information (start, end); in the "time" column
- **resolution**: pixel resolution power of two (1/4, 1/2,... 2, 4, 16); the scale from 256x256 pixels
- **data_steps**: number of steps (integer)
- **column_type**: "integer" or "date" (not mandatory)
- **minzoom**: minimum zoom level
- **maxzoom**: max zoom level
- **tiles**: tile array for this set
- **bounds**: [bounding box](http://wiki.openstreetmap.org/wiki/Bounding_Box) for tileset

```
{
    translate: [start, end], 
    resolution: 2           
    # scale: 1/resolution,
    data_steps: 365,
    column_type: "number"
    "minzoom": 0,
    "maxzoom": 11,
    "tiles": [
        'http://a.host.com/{z}/{x}/{y}.png',
        'http://b.host.com/{z}/{x}/{y}.png',
        'http://c.host.com/{z}/{x}/{y}.png',
        'http://d.host.com/{z}/{x}/{y}.png'
    ],
    "bounds": [ -180, -85.05112877980659, 180, 85.0511287798066 ]
}
```

## Using the Metadata

### Choosing a resolution

TileCubes are typically rendered on tiles of 256x256 pixel tiles. It is therefore recommended that you choose a scale that will render perfectly along the borders of the 256x256 tile, otherwise rendering artificats are often introducted. The possible pixel resolutions are,
```1, 2, 4, 8, 16, 32, 64, 128, 256```


## Tiles

Tiles are required to contain a core set of information to be rendered,
that information includes the number of pixels with data, and the x and
y values for each pixel. 

### tile url schema

for metadata:

```
http://host.com/medatada.torque.json
```

for tiles:

```
http://host.com/{z}/{x}/{y}.torque.[json|bin]
```

### tile format

tile format has two encoding formats, json and binary

#### json
it's a list fo objects with this format:

 - x: x pixel coordinate in tile system reference (int)
 - y: y pixel coordinate in tile system reference (int)
 - steps: time slots when this pixel is **active**, there is data at that time
 - values: values for each time slot

```
[
    {
        x: 25,
        y: 77,
        values: [ 1, 10 ],
        steps: [214, 215]
    },
...
]
```

#### binary

```
[
    {
        x__uint8: 19,
        y__uint8: 30,
        vals__uint8: [
            1
        ],
        dates__uint16: [
            225
        ]
    }
]
```

## Using the Tile

### Using values

The maximum value allowed in the ```values``` attribute is **255**

### Extracting current time

You can extracted from the ```current step```, ```translate``` and ```steps```:

```
current_time = translate.start  + step * (translate.end - translate.start)/data_steps;
```

### Encoding categories

The ```vals``` column can be used to store 