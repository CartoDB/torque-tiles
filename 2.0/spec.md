# TorqueTiles 2.0

## Abstract

TorqueTiles are a JSON representation of multidimensional data with
geospatial coordinates. TorqueTiles are broken into two document types:
the Metadata and the Tiles. The Metadata document describes the shared
information across the TileCube dataset. For each tile requested there
is a Tile document returned that describes the data in that tile.

TorqueTiles are referenced on the [Tile Map Service Specification]
(http://wiki.osgeo.org/wiki/Tile_Map_Service_Specification)
and are currently restricted to [global-mercator]
(http://wiki.osgeo.org/wiki/Tile_Map_Service_Specification#global-mercator).
Coordinates within a TileCube are measured in pixel offsets from the
border and assume 256x256 pixel tiles.


## Metadata

The TorqueMap Metadata document describes key tileset information. It includes:

- time range information (start, end)
- number of steps (integer)
- pixel resolution: power of two (1/4, 1/2,... 2, 4, 16)
- column_type: "integer" or "date" (not mandatory)

```
{
    translate: [start, end], # span in the "time" column
    resolution: 2            # resolution is the scale from 256x256 pixels
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

The following equation shows how the current time can be extracted from the current step, translate and steps:

```
current_time = start  + step * (end - start)/steps;
```


## Tiles

Tiles are required to contain a core set of information to be rendered,
that information includes the number of pixels with data, and the x and
y values for each pixel. From that core schema, TorqueTiles can be extended
to facilitate more advanced datasets

### tile url schema

for metadata:

    ```
    http://host.com/medatada.torque.json
    ```

for tiles:

    ```
    http://host.com/{z}/{x}/{y}.torque.[json|bin]
    ```

### Tile format

The tile format has two encodings:

+ JSON 
+ Binary

#### JSON
A list of objects with this format:

 - x__uint8: x pixel coordinate in tile system reference (unsigned integer, 8 bit)
 - y__uint8: y pixel coordinate in tile system reference (unsigned integer, 8 bit)
 - date__uint16: list of time slots when this pixel is **active** (i.e., there is data at that time) (unsigned integer, 16 bit)
 - vals__uint8: list of values corresponding to each time slot (unsigned integer, 8 bit)

```
[
    {
        x__uint8: 25,
        y__uint8: 77,
        vals__uint8: [ 1, 10],
        date__uint16: [214, 215]
    },
...
]
```

#### Binary
