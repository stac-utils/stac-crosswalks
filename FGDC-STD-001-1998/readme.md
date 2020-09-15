## FGDC Metadata (NAIP)

This aims for a cross walk from FGDC text metadata files commonly found with [NAIP 
imagery](https://www.fsa.usda.gov/programs-and-services/aerial-photography/imagery-programs/naip-imagery/) to STAC.

[sample document](m_3008601_ne_16_1_20150804.txt)

## Example from Stac-gen for parsing NAIP
Stac-gen first parses the FGDC Metadata with the mp command line tool into an xml document, then reads the xml document using an xml parsing library.  Example code is in python.  In this example we are finding the data collection data or dates, place keywords, but then we hard code some naip specific data into the STAC eo extension properties.  It would be better if there were a better mapping from the original metadata into STAC, but allowing for information from other data sources such as: user input, geotiff metadata, filename parsing.

```
# NOTE these are presumably naip specific, will need to reconsider
    #      if ever supporting another source with fgdc metadata

    # time period calendar date
    for e in root.findall('./idinfo/timeperd/timeinfo/sngdate/caldate'):
        item_dict['properties']['datetime'] = datetime.strptime(e.text, '%Y%m%d')
        print('parsed date: {} from raw: {}'.format(item_dict['properties']['datetime'], e.text))

    # place keywords
    places = [e.text for e in root.findall('./idinfo/keywords/place/placekey')]
    # Can't find a stac standard property for this but it seems useful
    item_dict['properties']['place_keywords'] = ';'.join(places)
    print(item_dict['properties']['place_keywords'])
    print('time to parse xml: {}s'.format(time.time()-t0))

    # do some more naip-specific stuff since this part would need to be reworked 
    # to support non-naip sources with fgdc metadata anyway
    item_dict['properties']['eo:gsd'] = 1.0
    item_dict['properties']['eo:instrument'] = "Leica ADS100"
    item_dict['properties']['eo:constellation'] = "NAIP"
    item_dict['properties']['eo:bands'] = [
            # credit Jeff Albrecht (github.com/geospatial-jeff):
            # https://github.com/geospatial-jeff/cognition-datasources-naip/blob/master/docs/example.json
            {
                "name": "B01",
                "common_name": "red",
                "gsd": 1.0,
                "center_wavelength": 635,
                "full_width_half_max": 16,
                "accuracy": 6
            },
            {
                "name": "B02",
                "common_name": "green",
                "gsd": 1.0,
                "center_wavelength": 555,
                "full_width_half_max": 30,
                "accuracy": 6
            },
            {
                "name": "B03",
                "common_name": "blue",
                "gsd": 1.0,
                "center_wavelength": 465,
                "full_width_half_max": 30,
                "accuracy": 6
            },
            {
                "name": "B04",
                "common_name": "nir",
                "gsd": 1.0,
                "center_wavelength": 845,
                "full_width_half_max": 37,
                "accuracy": 6
            }
        ]

    # add link to fgdc metadata
    item_dict['assets']['metadata'] = {
                'href': 's3://{}/{}'.format(stac_config['BUCKET_NAME'], fgdc_key),
                'type': 'text/plain',
                'title': 'FGDC metadata',
            }
```

