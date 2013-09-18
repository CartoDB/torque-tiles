# TileCubes 1.0

## Abstract

TileCubes are JSON representations of multidimensional data with
geospatial coordinates. TileCubes are broken into two document types,
the Metadata and the Tiles. The Metadata document describes the shared
information across thee TileCube dataset. For each tile requested there
is a Tile document returned that describes the data on that tile.

TileCubes are referenced on the [Tile Map Service Specification]
(http://wiki.osgeo.org/wiki/Tile_Map_Service_Specification)
and are currently restricted to [global-mercator]
(http://wiki.osgeo.org/wiki/Tile_Map_Service_Specification#global-mercator).
Coordinates within a TileCube are measured in pixel offsets from the
boarder and assume 256x256 pixel tiles.

## Metadata

The TileCube Metadata document describes key tileset information,
include zoom extents, pixel resolution, dimension names, and authors.

TODO

* tile type
 * core
 * single dimension with values
 * key value

* pixel dimension description
 * how it is defined
 * how it changes x,y coordinates/offsets


#### Schema

## Tiles

Tiles are required to contain a core set of information to be rendered,
that information includes the number of pixels with data, and the x and
y values for each pixel. From that core schema, TileCubes can be extended
to facilitate more advanced datasets

#### Core tile

```json
[
	4,
	1, 4, 5, 9,
	4, 5, 1, 3
]
```

### Single dimension with values

#### Schema

The single dimension extends the core tile with pixel based values. A
first series of integers describe the number of values found for each
pixel. The values of each pixel are then listed in order.


```json
[
	4,          # number of pixels
	1, 4, 5, 9, # x coordinates
	4, 5, 1, 3, # y coordinates
	1, 1, 1, 2, # number of values in each pixel
	2,          # value in the first pixel
	4,          # value in the second pixel
	1,          # value in the third pixel
	5, 4        # values in the fourth pixel
]
```

In the above example, just like our core above, we report the number
of pixels with events followed by the x coordinates and then the y
coordinates. In the third row we list the number of values to expect in
each pixel. So in this case, pixels 1-3 only have a single value. The
last pixel has two values. In the final row, we list the value of each
event. We see values 2, 4, 1 for pixels 1 through 3 respectively. Finally,
for pixel 4, we see one value of 5 and a second value of 4.

##### Example

This might be used to (e.g.) represent the year something happened (at each pixel)

```json
[
  3,                # three pixels have data
  243, 12, 27,      # x coordinates of pixels
  246, 5, 122       # y coordinates of pixels
  3, 1, 2,          # number of values found in each of the pixels
  2010, 2011, 2013, # the values for pixel 1  (e.g. it happened in 2010, 2011 and 2013)
  1978,             # the values for pixel 2
  1983, 1984        # the values of pixel 3
]
```

**Note**: In practice one might compress this further by counting the
          years passed from some start.  E.g. given a start year of 1970,
          the values for 2010, 2011 and 2013 would become 40,41,43 which
          uses less bytes

#### Content

### Key-value tiles

#### Schema

It is also possible to extend this, and encode key value pairs into a
single tile.  This might be used to (e.g.) provide the number of events
per month (at each pixel)

```json
[
  3,           # three pixels have data
  243, 12, 27, # x coordinates of pixels
  246, 5, 122  # y coordinates of pixels
  3, 1, 2,     # number of dimension pairs found in each of the pixels
  1, 2, 3,     # the values for the first dimension of pixel 1
               # (i.e. months Jan,Feb,Mar)
  12, 2, 3,    # the values for the second dimension of pixel 1
               # (i.e. Jan=12, Feb=2, Mar=3)
  11,          # pixel 2 dimension 1
  34,          # pixel 2 dimension 2 (i.e. asserts that November=34)
  10, 12       # pixel 3 dimension 1
  1, 127       # pixel 3 dimension 2
]
```

#### Content
