# STAC Crosswalks 

This repo aims to provide some resources for people working to map common metadata formats into STAC. It is just a handful of crosswalks right now, but 
the aim is to have a place to record any crosswalks that people perform, so others can benefit from the work.

If you'd like help mapping a metadata format then [open an issue](https://github.com/stac-utils/stac-crosswalks/issues/new) and link to an example record, 
with all its assets. Or if you've completed a mapping and would like it added here just open a pull request, or add the info to an issue.

## Available Crosswalks

* [Landsat](Landsat)
* [OGC EO Dataset Metadata GeoJSON (OGC_17-003r2)](OGC_17-003r2)
* [NAIP (FGDC-STD-001-1998)](FGDC-STD-001-1998)

## Desired Crosswalks

* DCAT-US [resources.data.gov/resources/dcat-us/](https://resources.data.gov/resources/dcat-us/)
* ISO-19115

## Understanding Crosswalks

At this point all crosswalks are oriented towards [Item](https://github.com/radiantearth/stac-spec/blob/master/item-spec/item-spec.md) level mappings. 
If you've ended up at this page because someone asked you to map from an existing metadata format to STAC it's probably worth taking the time to understand
if that is really appropriate.

STAC is coming into the hype curve of 'cool new thing', that many people higher up don't fully understand but want to be on the latest thing. STAC aims for
a fairly specific purpose, and some of the mappings that people ask of it may not make sense. It should also be noted that STAC is not seen as a replacement
for existing standards - it is meant to provide a subset of fields to make the data more searchable and browsable.

The following are some general recommendations on mapping another standard to STAC.

* The case where a mapping makes the most sense is large collections of individual 'assets'. The classic example is satellite imagery, where most archives
have millions of records, but other data types are relevant. If you've got millions of metadata records then it is likely appropriate to try to map those to 
STAC `Items`. This is what this repo is primarily focused on.
* Most metadata standards are oriented towards the 'layer', 'dataset' or 'collection' level, for example to describe all the geospatial holdings of a government
or company. STAC is not really aimed at this problem, and if you already have a system with metadata in a catalog at this point we recommend sticking with what 
you've got. If you're looking to make that type of metadata more accessible we'd recommend collaborating on [OGC API - 
Records](https://github.com/opengeospatial/ogcapi-records) for a next generation approach.If you don't have any metadata exposed yet see the next bullet point. 
* STAC does offer the [Collection](https://github.com/radiantearth/stac-spec/blob/master/collection-spec/collection-spec.md) construct that is more equivalent
to what most metadata standards are oriented to. Some providers are using it as an alterative to making a full, traditional metadata record. It extends the
[Collection](http://docs.opengeospatial.org/DRAFTS/17-069r2.html#_collection_) of the OGC API - Features standard. If you don't have any layer / collection metadata
and just want to expose that information in someway then it is an easy way to do that, but it not yet part of a full ecosystem enabling search.

We are aiming to collaborate with OGC to provide a full, modern answer to making layer/dataset/collection level data more available. We hope to have a more generic
version of a STAC Collection, which can be used statically & dynamically. But we believe the OGC API - Records API will be more appropriate for it than STAC, so
it can handle vector datasets that can be fully represented as OGC Feature API's


### General Metadata Mapping advice

Note that the STAC Spec contains a ['best practices' section](https://github.com/radiantearth/stac-spec/blob/v1.0.0-beta.2/best-practices.md#field-selection-and-metadata-linking) 
with advice on how to handle mapping in general. The core thing to remember is that STAC is not a replacement for existing metadata. The STAC item should be
a subset of the overall metadata fields that are appropriate for search, and it should link to the existing metadata record as an 'asset'. 

