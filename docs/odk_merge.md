# odk_merge.py

This program conflates the data collected using ODK Collect with
existing OSM data. Many buildings in OSM were from imports of AI
derived building footprints, and the only tages are
_building=yes_. When doing ground data collection, in addition to
collecting new data, you want to add or correct tags in existing OSM
data.

The file of collected data is downloaded as a CSV file from ODK Central.
Then it's converted to OSM XML using **CSVDump.py**. Once converted,
to OSM XML format, all tags can be merged or added to existing OSM
data.

## Usage

    usage: odk_merge.py [-h] [-v] [-c ODKFILE] [-f OSMFILE] [-o OUTFILE] [-b BOUNDARY]

    This program conflates ODK data with existing features from OSM.

    options:
    -h, --help            show this help message and exit
    -v, --verbose         verbose output
    -c ODKFILE, --odkfile ODKFILE - ODK CSV file downloaded from ODK Central
    -f OSMFILE, --osmfile OSMFILE - OSM XML file created by odkconvert
    -o OUTFILE, --outfile OUTFILE - Output file from the merge
    -b BOUNDARY, --boundary BOUNDARY - Boundary polygon to limit the data size

The boundary file is a polygon to limit the dataset size, useful when
using downloaded OSM data for entire countries, or using a large
database. Most ODK data files are not usually very large. It can be in
any format, but GeoJson is the most common.

To specify a database as the OSM source, the input file gets prefixed
with _pg:_, followed by the database name. Otherwise use a disk
file.

The ODK source is an OSM XML file created by _CSVDump.py_, where all the
tags have been converted from the ODK Central submission download. The
output file is in OSM XML format, and contains modified entries where
existing data has the new tags added.

## Examples
**Example 1:** 
Merge ODK data with existing OSM data for a specific area:

    ./odk_merge.py -f existing_osm_data.osm -c collected_data.csv -o merged_data.osm

This command merges the ODK data in `collected_data.csv` with the existing OSM data in `existing_osm_data.osm` and outputs the result to `merged_data.osm`.

**Example 2:**
Merge ODK data with OSM database:

    ./odk_merge.py -f pg:osm_database -c collected_data.csv -o merged_data.osm

This command merges the ODK data in `collected_data.csv` with the OSM data in the database named `osm_database` and outputs the result to merged_data.osm.

**Example 3:**
Limit the merge to a specific boundary:

    ./odk_merge.py -f existing_osm_data.osm -c collected_data.csv -o merged_data.osm -b boundary.geojson

This command merges the ODK data in `collected_data.csv` with the existing OSM data in `existing_osm_data.osm`, but only within the boundary defined by `boundary.geojson`. The output is saved to `merged_data.osm`.
