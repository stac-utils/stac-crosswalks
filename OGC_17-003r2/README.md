## Mapping from OGC EO Dataset Metadata GeoJSON(-LD) Encoding Standard to STAC

This describes the mapping from [OGC 17-003r2](https://docs.opengeospatial.org/is/17-003r2/17-003r2.html) to [STAC 
Items](https://github.com/radiantearth/stac-spec/blob/master/item-spec/item-spec.md). Thanks to Yves Coene for putting this together, this document
just ports [his original work](https://docs.google.com/document/d/1LDew69b104krdln66eGGPSjHX9rbs4UTYabPGM3FMo8/edit) for more visibility.

Note there is also [a mapping](http://docs.opengeospatial.org/is/17-047r1/17-047r1.html#113) from this standard to the general [OGC Open Search EO GeoJSON
Response](http://docs.opengeospatial.org/is/17-047r1/17-047r1.html).

| **STAC Property**                                                                        |                   | **OGC 17-003r2 Property**                                                                                              |   |   |
|------------------------------------------------------------------------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------|---|---|
| **[Item Fields](https://github.com/radiantearth/stac-spec/blob/master/item-spec/item-spec.md#item-fields)** |                   |                                                                                                                        |
| id                                                                                       |                   | $.properties.identifier                                                                                                |
| collection                                                                               |                   | $.properties.parentIdentifier                                                                                          |
|                                                                                          |                   |                                                                                                                        |
| | |
| https://github.com/radiantearth/stac-spec/tree/master/extensions/eo                      |                   |                                                                                                                        |
| eo:cloud_cover                                                                           | number            | $.properties.productInformation.cloudCover                                                                             |
| eo:bands                                                                                 | [Band Object]     |                                                                                                                        |
| https://github.com/radiantearth/stac-spec/blob/master/item-spec/common-metadata.md       |                   |                                                                                                                        |
| title                                                                                    | string            | $.properties.title                                                                                                     |
| description                                                                              | string            | $.properties.abstract (See OGC 17-084r1)                                                                               |
| created                                                                                  | string            | $.properties.creationDate                                                                                              |
| updated                                                                                  | string            | $.properties.updated                                                                                                   |
| datetime                                                                                 | string \| null    | $.properties.date                                                                                                      |
| start_datetime                                                                           | string            | $..acquisitionInformation[*].acquisitionParameters.beginningDateTime                                                   
| end_datetime                                                                             | string            | $..acquisitionInformation[*].acquisitionParameters.endingDateTime                                                      
| providers                                                                                | [Provider Object] | See OGC 17-084r1                                                                                                       |
| license                                                                                  | string            | See OGC 17-084r1                                                                                                       |
| platform                                                                                 | string            | $..acquisitionInformation[*].platform.platformShortName $..acquisitionInformation[*].platform.platformSerialIdentifier |
| instruments                                                                              | [string]          | $..acquisitionInformation[*].instrument.instrumentShortName                                                            |
| constellation                                                                            | string            | $..acquisitionInformation[*].platform.platformShortName                                                                |
| mission                                                                                  | string            |                                                                                                                        |
| gsd                                                                                      | number            |                                                                                                                        |
|                                                                                          |                   |                                                                                                                        |
| https://github.com/radiantearth/stac-spec/blob/master/extensions/sar/README.md           |                   |                                                                                                                        |
| sar:instrument_mode (M)                                                                  | string            | $..acquisitionParameters.operationalMode                                                                               |
