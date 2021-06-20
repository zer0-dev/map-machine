**Röntgen** (or **Roentgen** when ASCII is preferred) project consists of

  * simple Python [OpenStreetMap](http://openstreetmap.org) renderer (see [usage](#usage), [renderer documentation](#map-generation)),
  * [set of icons](#icon-set),
  * and [map styles](#map-styles).

[![Build Status](https://travis-ci.org/enzet/Roentgen.svg?branch=master)](https://travis-ci.org/enzet/Roentgen)

The idea behind Röntgen project is to have a possibility to *display any map feature* represented by OpenStreetMap data tags by means of colors, shapes, and icons.

Röntgen is primarily created for OpenStreetMap contributors. Suppose, you spent time adding colors for building walls, benches and shelters for bus stops but they are not represented on the standard tile layer. Röntgen helps to display all changes you made.

Nevertheless, Röntgen map generator can generate precise but messy maps for OSM contributors as well as pretty and clean maps for OSM users.

Usage
-----

To get SVG map, just run

```bash
python roentgen.py -b <lon1>,<lat1>,<lon2>,<lat2>
```

(e.g. `python roentgen.py -b 2.284,48.86,2.29,48.865`). It will automatically download OSM data and write output map to `map.svg`. For more options see [Map generation](#map-generation).

Map features
------------

### Building levels ###

Simple shapes for walls and shade in proportion to [`building:levels`](https://wiki.openstreetmap.org/wiki/Key:building:levels) value.

![3D buildings](doc/buildings.png)

### Trees ###

Visualization of tree leaf types (broadleaved or needleleaved) and genus or taxon by means of icon shapes and leaf cycles (deciduous or evergreen) by means of color.

![Trees](doc/trees.png)

### Viewpoint and camera direction ###

Visualize [`direction`](https://wiki.openstreetmap.org/wiki/Key:direction) tag for [`tourism`](https://wiki.openstreetmap.org/wiki/Key:tourism)=[`viewpoint`](https://wiki.openstreetmap.org/wiki/Tag:tourism=viewpoint) and [`camera:direction`](https://wiki.openstreetmap.org/wiki/Key:camera:direction) for [`man_made`](https://wiki.openstreetmap.org/wiki/Key:man_made)=[`surveillance`](https://wiki.openstreetmap.org/wiki/Tag:man_made=surveillance).

![Surveillance](doc/surveillance.png)

### Power tower design ###

Visualize [`design`](https://wiki.openstreetmap.org/wiki/Key:design) values used with [`power`](https://wiki.openstreetmap.org/wiki/Key:power)=[`tower`](https://wiki.openstreetmap.org/wiki/Tag:power=tower) tag.

![Power tower design](doc/power_tower_design.png)

![Power tower design](doc/power.png)

### Emergency ###

![Emergency](doc/emergency.png)

### Japanese map symbols ###

There are [special symbols](https://en.wikipedia.org/wiki/List_of_Japanese_map_symbols) appearing on Japanese maps.

![Japanese map symbols](doc/japanese.png)

Icon set
--------

If tag is drawable it is displayed using icon combination and colors. All icons are under [CC BY](http://creativecommons.org/licenses/by/4.0/) license. So, do whatever you want but give appropriate credit. Icon set is heavily inspired by [Maki](https://github.com/mapbox/maki), [Osmic](https://github.com/gmgeo/osmic), and [Temaki](https://github.com/ideditor/temaki) icon sets.

![Icons](doc/grid.png)

Feel free to request new icons via issues for whatever you want to see on the map. No matter how frequently the tag is used in OpenStreetMap since final goal is to cover all tags. However, common used tags have priority, other things being equal.

Generate icon grid and individual icons with `python roentgen.py icons`. It will create `icon_grid.svg` file, and SVG files in `icon_set/ids` directory where files are named using shape identifiers (e.g. `power_tower_portal_2_level.svg`) and in `icon_set/names` directory where files are named using shape names (e.g. `Röntgen portal two-level transmission tower.svg`). Files from the last directory are used in OpenStreetMap wiki (e.g. [`File:Röntgen_portal_two-level_transmission_tower.svg`](https://wiki.openstreetmap.org/wiki/File:R%C3%B6ntgen_portal_two-level_transmission_tower.svg)).

### Icon combination ###

Some icons can be combined into new icons.

![Bus stop icon combination](doc/bus_stop.png)

Map styles
----------

### All tags style ###

Options: `--show-missing-tags --overlap 0`.

Display as many OpenStreetMap data tags on the map as possible.

### Pretty style ###

Options: `--draw-captions main --level overground`.

Display only not overlapping icons and main captions.

### Creation time mode ###

Visualize element creation time with `--mode time`.

![Creation time mode](doc/time.png)

### Author mode ###

Every way and node displayed with the random color picked for each author with `--mode user-coloring`.

![Author mode](doc/user.png)

Map generation
--------------

### Requirements ###

  * Python (at least 3.8).

### Installation ###

```bash
pip install -r requirements.txt
```

### Script running ###

There are simple Python renderer that generates SVG map from OpenStreetMap data. You can run it using:

```bash
python roentgen.py \
    -b ${LONGITUDE_1},${LATITUDE_1},${LONGITUDE_2},${LATITUDE_2} \
    -o ${OUTPUT_FILE_NAME} \
    -s ${OSM_ZOOM_LEVEL}
```

Example:

```bash
python roentgen.py -b 2.284,48.86,2.29,48.865
```

### Main arguments ###

#### Required ####

  * `--boundary-box` or `-b`: boundary box to draw. Value: `<longitude 1>,<latitude 1>,<longitude 2>,<latitude 2>`. If first value is negative, use quotation marks and space before first `-`. For example, `-b " -122.335,47.614,-122.325,47.617"`.

#### Optional ####

  * `--scale` or `-s`: OSM [zoom level](https://wiki.openstreetmap.org/wiki/Zoom_levels). Default is 18.
  * `-o`: path to output SVG file name. Default is `map.svg`.
  * `-i`: path to input XML file name. If this argument is not set, XML file will be downloaded through OpenStreetMap API.

Check all arguments with `python roentgen.py --help`.

Tile generation
---------------

```bash
python roentgen.py tile \
    -c ${LATITUDE},${LONGITUDE} \
    -s ${OSM_ZOOM_LEVEL}
```

Tile will be stored as SVG file to `tiles/tile_<zoom level>_<x>_<y>.svg`, where `x` and `y` are tile coordinates.

Example:

```bash
python roentgen.py tile -c 55.7510637,37.6270761 -s 18
```

will generate SVG file `tiles/tile_18_158471_81953.svg`.

