## Mapping from OGC EO Dataset Metadata GeoJSON(-LD) Encoding Standard to STAC

This describes the mapping from [OGC 17-003r2](https://docs.opengeospatial.org/is/17-003r2/17-003r2.html) to [STAC 
Items](https://github.com/radiantearth/stac-spec/blob/master/item-spec/item-spec.md). Thanks to Yves Coene for putting this together, this document
just ports [his original work](https://docs.google.com/document/d/1LDew69b104krdln66eGGPSjHX9rbs4UTYabPGM3FMo8/edit) for more visibility.

Note there is also [a mapping](http://docs.opengeospatial.org/is/17-047r1/17-047r1.html#113) from this standard to the general [OGC Open Search EO GeoJSON
Response](http://docs.opengeospatial.org/is/17-047r1/17-047r1.html).

| **STAC Property**                                                                        |                   | **OGC 17-003r2 Property**                                                                                             
|------------------------------------------------------------------------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------|
| **[Item Fields](https://github.com/radiantearth/stac-spec/blob/master/item-spec/item-spec.md#item-fields)** |                   |                                                                                                                        |
| id                                                                                       |                   | $.properties.identifier                                                                                                |
| collection                                                                               |                   | $.properties.parentIdentifier                                                                                          |
|                                                                                          |                   |                                                                                                                        |
| **[Common Metadata](https://github.com/radiantearth/stac-spec/blob/master/item-spec/common-metadata.md)**       |                   |                                                                                                                        |
| title                                                                                    | string            | $.properties.title                                                                                                     |
| description                                                                              | string            | $.properties.abstract (See OGC 17-084r1)                                                                               |
| created                                                                                  | string            | $.properties.creationDate                                                                                             |
| assets.*.created (role="data")                                                           | string            | $..productInformation.processingDate                                                                                             |
| updated                                                                                 | string            | $.properties.updated (metadata)                                                                                                 |
| links                                                                        | [Link Object]            | $.properties.links  |
| assets.*.updated                                                                        | string            | $.properties.availabilityTime (data)  |
| assets.* (role="data")                                                                   | Asset Object            | $.properties.links.data  |
| assets.* (role="thumbnail")                                                               | Asset Object            | $.properties.links.previews  |
| assets.* (role="metadata")                                                               | Asset Object            | $.properties.links.alternates  |
| datetime                                                                                 | string \| null    | $.properties.date                                                                                                      |
| start_datetime                                                                           | string            | $..acquisitionInformation[\*].acquisitionParameters.beginningDateTime                                                   
| end_datetime                                                                             | string            | $..acquisitionInformation[\*].acquisitionParameters.endingDateTime                                                      
| providers                                                                                | [Provider Object] | See OGC 17-084r1                                                                                                       |
| license                                                                                  | string            | See OGC 17-084r1                                                                                                       |
| platform                                                                                 | string            | $..acquisitionInformation[\*].platform.platformShortName $..acquisitionInformation[\*].platform.platformSerialIdentifier |
| instruments                                                                              | [string]          | $..acquisitionInformation[\*].instrument.instrumentShortName                                                            |
| constellation                                                                            | string            | $..acquisitionInformation[\*].platform.platformShortName                                                                |
| mission                                                                                  | string            |                                                                                                                        |
| gsd                                                                                      | number            |                                                                                                                        |
|                                                                                          |                   |                                                      
| **[EO Extension](https://github.com/stac-extensions/eo)**                      |                   |                                                                                                                        |
| eo:cloud_cover                                                                           | number            | $.properties.productInformation.cloudCover 
| eo:snow_cover                                                                           | number            | $.properties.productInformation.snowCover |
| eo:bands                                                                                 | [Band Object]     |                                                                                                                        |
|  |                                                                  |
| **[SAR Extension](https://github.com/stac-extensions/sar)**           |                   |                                                                                                                        |
| sar:instrument_mode (M)                                                                  | string            | $..acquisitionParameters.operationalMode                                                                               |
| sar:polarizations (M)                                                                  | [string]            |  $..acquisitionParameters.polarisationChannels |
|  sar:product_type (M)       |   string              |  $.properties.productInformation.productType   |
|  sar:observation_direction  |     string            | $..acquisitionParameters.antennaLookDirection |
|                                                                                          |                   |    
| **[SAT Extension](https://github.com/stac-extensions/sat)** |                   |         |
|  sat:orbit_state  |     string            | $.properties.acquisitionInformation[*].acquisitionParameters.orbitDirection |
|  sat:absolute_orbit  |     integer            | $.properties.acquisitionInformation[*].acquisitionParameters.orbitNumber |
|  sat:anx_datetime  |     string            | $.properties.acquisitionInformation[*].acquisitionParameters.ascendingNodeDate |
|  sat:relative_orbit |  integer | $..acquisitionParameters.relativeOrbitNumber |
|                                                                                          |                   |    
| **[Scientific Extension](https://github.com/stac-extensions/scientific)** |                   |         |
|  sci:doi  |     string            | $.properties.doi |
|  sci:citation  |     string            | $.properties.bibliographicCitation (OGC 17-084r1) |
|                                                                                          |                   |    
| **[Version Extension](https://github.com/stac-extensions/version)** |                   |         |
|  version  |     string            | $properties.productInformation.version |
|                                                                                          |                   |    
| **[View Extension](https://github.com/stac-extensions/view)** |                   |         |
|  view:off_nadir  |     number            | $..acquisitionAngles.instrumentElevationAngle  |
|  view:incidence_angle  |     number            | $..acquisitionAngles.incidenceAngle  |
|  view:azimuth  |     number            | $..acquisitionAngles.instrumentAzimuthAngle  |
|  view:sun_azimuth  |     number            | $..acquisitionAngles.illuminationAzimuthAngle  |
|  view:sun_elevation  |     number            | $..acquisitionAngles.illuminationElevationAngle  |
|                                                                                          |                   |    
| **[Projection Extension](https://github.com/stac-extensions/projection)** |                   |         |
|  proj:epsg (M)  |     integer            | $.properties.productInformation.referenceSystemIdentifier |
|                                                                                          |                   |    
| **[Timestamps Extension](https://github.com/stac-extensions/timestamps)** |                   |         |
|  published  |     string            | $.properties.published |
|  expires  |     string            | $.properties.available |
|                                                                                          |                   |    
| **[Landsat Extension](https://landsat.usgs.gov/stac/landsat-extension/schema.json)** |                   |         |
|  landsat:wrs_path  |     string            | $..acquisitionParameters.wrsLongitudeGrid |
|  landsat:wrs_row  |     string            | $..acquisitionParameters.wrsLatitudeGrid |
|  landsat:scene_id  |     string            |  |
|                                                                                          |                   |    
| **[Processing Extension](https://github.com/stac-extensions/processing)** |                   |         |
|  processing:level  |     string            | $..productInformation.processingLevel |
|  processing:facility  |     string            | $..productInformation.processingCenter |
|  processing:software  |     Map<string, string>            | $..productInformation.processorName  |
|  processing:software  |     Map<string, string>            | $..productInformation.processorVersion |
|                                                                                          |                   |    
| **[Hyperspectral Extension](https://github.com/stac-extensions/hsi)** |                   |         |
|  hsi:wavelength_min  |     [number]            | $..wavelengths[*].startWavelength |
|  hsi:wavelength_max  |     [number]            | $..wavelengths[*].endWavelength |
|                                                                                          |                   |    
| **[TBD Offering Extension](https://github.com/stac-extensions/web-map-links/issues/8)** |               |         |
|  $.assets.\*.roles  |     [string (uri)]           | $..offerings[\*].code.    |
|    |                 | $..offerings[\*].operations[\*].code |
|  $.assets.*.method  |     string            | $..offerings[\*].operations[\*].method |
|  $.assets.*.type  |     string            | $..offerings[\*].operations[\*].type |
|  $.assets.*.href  |     string (uri)           | $..offerings[\*].operations[\*].href |




