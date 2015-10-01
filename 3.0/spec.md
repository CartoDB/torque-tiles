# TorqueTiles 3.0

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

- **start**: start time, in steps or unix timestamp
- **end**: end time, in steps or unix timestamp
- **resolution**: pixel resolution power of two (1/4, 1/2,... 2, 4, 16); the scale from 256x256 pixels
- **data_steps**: number of steps (integer)
- **column_type**: "integer" or "date", default "integer"
- **minzoom**: minimum zoom level, optional
- **maxzoom**: max zoom level, optional
- **tiles**: tile array for this set, required
- **bounds**: [bounding box](http://wiki.openstreetmap.org/wiki/Bounding_Box) for tileset, optional
- **data_XXX**: data mapping for each value. See ``Tiles`` section, this attribute is the same than
  `data_XXX` but globally. It's used for save space when all the tiles share the same data mapping.

```
{
    start: 0,
    end: 100, 
    resolution: 2
    # scale: 1/resolution,
    data_steps: 365,
    column_type: "number"
    "minzoom": 0,
    "maxzoom": 11,
    "tiles": [
        'http://a.host.com/{z}/{x}/{y}.torque.json',
        'http://b.host.com/{z}/{x}/{y}.torque.json',
        'http://c.host.com/{z}/{x}/{y}.torque.json',
        'http://d.host.com/{z}/{x}/{y}.torque.json'
    ],
    "bounds": [ -180, -85.05112877980659, 180, 85.0511287798066 ]
    "data_attribute_1": {
        "1": { "category": "test" },
        "3": { "category": "otherone" },
    }
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

### Tile URL schema

```
http://host.com/{z}/{x}/{y}.torque.[json|bin]
```

### Tile format
Each Torque tile is a JSON document containing an array, each of whose elements represents a point within the tile, notated using the following format:

 - x: x pixel coordinate in tile system reference (int)
 - y: y pixel coordinate in tile system reference (int)
 - steps: time slots when this pixel is **active**, there is data at that time
 - values_XXXX: integer values for each time slot and attribute. At least one values array should exist.
 - data_XXXX: contains the mapping from values to data. This is a key value where the value can contain
   any JSON object. This attribute is not required.

 There step array and values array must have the same length, so for each step entry another one
 should exist 

```
[
    {
        x: 25,
        y: 77,
        values_attribute_1: [ 1, 10, 0 ],
        values_attribute_2: [ 3, 0, 10 ],
        steps: [214, 215, 216]
        data_attribute_1: {
            "1": { "category": "test" },
            "3": { "category": "otherone" },
        }
    },

...
]
```

## Using the Tile

### Extract the pixel position

``x`` and ``y`` are in range [0, 256/resolution] so to get the final pixel position (based on 256x256 tiles) the following math should be used:

```
pixel_x = x * resolution
pixel_y = y * resolution
```

The coordinate origin for Torque tiles is the **bottom left corner**.

### Extracting current time

You can extracted from the ```current step```, ```translate``` and ```steps```:

```
current_time = translate.start  + step * (translate.end - translate.start)/data_steps;
```

### Encoding categories

The ```vals``` column can be used to store 

## Binary transport
To be done
