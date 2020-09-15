# Landsat Metadata Crosswalks
Landsat item-level metadata is produced in an Object Description Language based on the Planetary Data System 3 specification [PDS3](https://pds.jpl.nasa.gov/datastandards/pds3/). The field names follow an internally controlled vocabulary maintained by the U.S. Geological Survey Earth Resources Observation and Scinence (EROS) Center and stored in the Landsat Metadata Definition Document (LMDD).

## Caveats

Some STAC fields will need to be hardcoded and cannot be populated from the product's native ODL/MTL metadata. The Landsat Product ID is a unique identifier for products coming out of Colleciton 1 or 2. The Landsat Scene ID can be used for provenance to tie the same observation across multiple collections or processing levels. Coordinates provided in the native metadata relate to product extent, not data extent. Data coordinates must be derived.

The full metadata for [levels 1](landsat-c2-l1.csv) and [2](landsat-c2-l2.csv) are provided in *.csv format listing the STAC field and the corresponding native metadata field by sensor/satellite. Metadata is grouped into Level-1 or Level-2 processing levels. Default to the higher processing level when fields duplicate.

## Level-1

| STAC Field | Landsat Field |
|------------|---------------|
| **[Item Fields](https://github.com/radiantearth/stac-spec/blob/master/item-spec/item-spec.md#item-fields)** | |
| id | LANDSAT_PRODUCT_ID |
| | |
| **[Common Metadata](https://github.com/radiantearth/stac-spec/blob/master/item-spec/common-metadata.md)**| |
| datetime | DATE_ACQUIRED
| created | DATE_PRODUCT_GENERATED 
| platform | SPACECRAFT_ID
| instruments | SENSOR_ID
| gsd | GRID_CELL_SIZE_REFLECTIVE, GRID_CELL_SIZE_THERMAL, GRID_CELL_SIZE_PANCHROMATIC
| | |
| **[EO Extension](https://github.com/radiantearth/stac-spec/tree/master/extensions/eo)** | |
| eo:cloud_cover | CLOUD_COVER |
| | |
| **[Scientific Extension](https://github.com/radiantearth/stac-spec/blob/master/extensions/scientific/README.md)** |
| sci:doi | DIGITAL_OBJECT_IDENTIFIER |
| | |
| **[View Extension](https://github.com/radiantearth/stac-spec/tree/master/extensions/view)** | |
| view:sun_azimuth | SUN_AZIMUTH |
| view: sun_elevation | SUN_ELEVATION | 
| view: off_nadir | ROLL_ANGLE |
| | |
| **[Landsat Extension](https://landsat.usgs.gov/stac/landsat-extension/schema.json)** | |
| landsat:scene_id | LANDSAT_SCENE_ID | 
| landsat:processing_level | PROCESSING_LEVEL
| landsat:collection_number | COLLECTION_NUMBER |
| landsat:collection_category | COLLECTION_CATEGORY |
| landsat:cloud_cover_land | CLOUD_COVER_LAND |
| landsat:wrs_type | WRS_TYPE
| landsat:wrs_path | WRS_PATH
| landsat:wrs_row | WRS_ROW
| | |
| **[Projection Extension](https://github.com/radiantearth/stac-spec/tree/master/extensions/projection)** | 
| proj:bbox | CORNER_UL_LAT_PRODUCT, CORNER_UL_LON_PRODUCT, CORNER_UR_LAT_PRODUCT, CORNER_UR_LON_PRODUCT, CORNER_LL_LAT_PRODUCT, CORNER_LL_LON_PRODUCT, CORNER_LR_LAT_PRODUCT, CORNER_LL_LON_PRODUCT


## Level-2

Use same mappings from Level-1 with differences shown below.

| STAC Field | Landsat Field |
|------------|---------------|
| **[Common Metadata](https://github.com/radiantearth/stac-spec/blob/master/item-spec/common-metadata.md)**| |
| gsd | GRID_CELL_SIZE_REFLECTIVE, GRID_CELL_SIZE_THERMAL |