# Crosswalk between STAC and OGC API - Records

This document gives a brief overview over similarities and differences between
- **STAC (API), v1.0.0** (+ extensions)
- **OGC API - Records - Part 1: Core, [draft version](http://docs.ogc.org/DRAFTS/20-004.html) (2023-05-26)**

In the following I use **OAR1** as an abbreviation for **OGC API - Records - Part 1**.

## Introduction (tl;dr)

This document will compare content model (STAC vs. OAR1) and the API specification (STAC API vs. OAR1).

Unfortunately, STAC and OGC API - Records are not fully compliant.
It means that by default a STAC API is not automatically a OGC API - Records and vice versa.
Nevertheless, there are no major conflicts in the specification (except for catalogs) so that both implementations can be made compliant with a relatively small number of additions.

### Terminology

You could map the different entities specified in STAC and OAR1 as follows:

| STAC       | OAR1              |
| ---------- | ----------------- |
| Catalog    | Folder            |
| Collection | Record Collection |
| Item       | Record            |

Nevertheless, you could also think about other mappings between the specifications. Some may argue, a STAC Collection is a OAR1 Record. Due to the way the JSON representations are defined, I'm assuming a mapping as in the table above though.

Please also note that Catalogs, Collections and Items could be Records in OAR1 and some implementations only convert Records to Record Collections once they have at least one child record. Folders/Catalogs are currently mostly used in static deployments and don't play a huge role in APIs/dynamic deployments.

## Static (content)

- The content model and static catalog behavior for STAC is specified in the standalone STAC specification and corresponding extensions.
- The content model and static catalog behavior for OAR1 is specified in the requirement classes "Record Core", "Record Collection", "Folder" (still in a PR) and "Crawlable Catalogue".

***Note:*** In the tables below, the following applies:
- **bold** properties indicate that they are required.
- ***bold and italic*** properties are required if a certain criteria is met
- ✅ means that fields are fully compatible.
- ⚠️ means that fields are generally compatible (if applicable), but the field is not required in both specs or there are other minor differences implementors need to take care of.
- ❌ means there is a conflict in the specifications, the generated JSON is NOT compatible.
- **Profile\*** = Checks the compliance if STAC would be defined as an OGC API - Records - Part 1 profile / extension.

### Catalogs

| STAC                       | OAR1                | Profile*                                                     | Compatible / Comments                                        |
| -------------------------- | ------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **type** = `Catalog`       | **type** = `Folder` | ❌ [Issue](https://github.com/opengeospatial/ogcapi-records/issues/264) | ❌ OAR1 and STAC are currently in conflict                    |
| **stac_version**           | -                   | ✅                                                            | ⚠️                                                            |
| stac_extensions            | -                   | ✅                                                            | ✅                                                            |
| **id**                     | **id**              | ✅                                                            | ✅                                                            |
| title                      | title               | ✅                                                            | ✅                                                            |
| **description**            | description         | ✅                                                            | ⚠️ STAC disallows empty strings                               |
| **links**                  | **links**           | ⚠️ [Issue 1](https://github.com/opengeospatial/ogcapi-records/issues/276) [Issue 2](https://github.com/opengeospatial/ogcapi-records/issues/275) | ⚠️ Structure is compatible except for templated links and different relation type requirements (none in STAC; `self` in OAR1). Also, OAR1 and STAC both use the relation type `child` with a media type `application/json` to link to catalogs, but **clients can't distinguish whether they can expect OAR1 or STAC**. |
| keywords (common metadata) | keywords            | ✅                                                            | ✅                                                            |
| language (extension)       | language            | ✅                                                            | ✅                                                            |
| languages (extension)      | languages           | ✅                                                            | ✅                                                            |
| created (common metadata)  | created             | ✅                                                            | ✅                                                            |
| updated (common metadata)  | updated             | ✅                                                            | ✅                                                            |
| -                          | extent              | ✅                                                            | ⚠️ Generally aligned, but some legacy STAC tooling may detect STAC Collections based on the fields that are present. This was previously sometimes done by checking the existence of the `extent` field, so some legacy STAC tooling may detect OAR Catalogs as STAC Collections. |

### Collections

| STAC                      | OAR1                                | Profile*                                                     | Compatible / Comments                                        |
| ------------------------- | ----------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **type** = `Collection`   | **type** = `Collection`             | ✅                                                            | ✅                                                            |
| -                         | **itemType** = `record` (API only?) | ✅                                                            | ✅                                                            |
| **stac_version**          | -                                   | ✅                                                            | ⚠️                                                            |
| stac_extensions           | conformsTo (tbc)                    | ✅                                                            | ⚠️ STAC extensions could be used in `conformsTo`, but conformance classes that are not valid JSON schemas can't be used in `stac_extensions`. |
| **id**                    | **id**                              | ✅                                                            | ✅                                                            |
| title                     | **title**                           | ⚠️ [Issue](https://github.com/radiantearth/stac-spec/issues/1232) | ⚠️ [Potentially require in STAC?](https://github.com/radiantearth/stac-spec/issues/1232) |
| **description**           | description                         | ✅                                                            | ⚠️ STAC disallows empty strings                               |
| **links**                 | **links**                           | ⚠️ [Issue 1](https://github.com/opengeospatial/ogcapi-records/issues/276) [Issue 2](https://github.com/opengeospatial/ogcapi-records/issues/275) | ⚠️ Structure is compatible except for templated links, but different relation type requirements (none in STAC; `self` and `root` in OAR1). |
| keywords                  | keywords                            | ✅                                                            | ✅                                                            |
| **license**               | license                             | ⚠ [Issue](https://github.com/radiantearth/stac-spec/issues/1232) | ⚠ STAC: SPDX identifier or `proprietary` or `various`; OAR1: SPDX identifier, SPDX expression, `other` or anything |
| providers                 | -                                   | ✅                                                            | ✅                                                            |
| contacts (extension)      | contacts                            | ✅                                                            | ✅                                                            |
| **extent**                | extent                              | ✅                                                            | ⚠️ STAC requires a bounding box and a temporal interval.      |
| summaries                 | -                                   | ✅                                                            | ✅                                                            |
| assets                    | -                                   | ✅                                                            | ⚠️ Assets are provided as links (aka "associations") in OAR1. |
| language (extension)      | language                            | ✅                                                            | ✅                                                            |
| languages (extension)     | languages                           | ✅                                                            | ✅                                                            |
| -                         | recordLanguages (API only?)         | ✅                                                            | ✅                                                            |
| created (common metadata) | created                             | ✅                                                            | ✅                                                            |
| updated (common metadata) | updated                             | ✅                                                            | ✅                                                            |
| themes (extension)        | themes                              | ✅                                                            | ✅                                                            |
| crs (API only)            | crs                                 | ✅                                                            | ✅                                                            |
| -                         | rights                              | ✅                                                            | ✅                                                            |
| -                         | properties                          | ⚠️ [Issue](https://github.com/opengeospatial/ogcapi-records/issues/274) | ⚠️ STAC Collections usually list additional properties at the "top-level" of the object, not in properties. OAR1 is not clearly defining a recommended place. |

### Items / Records

Assuming a GeoJSON encoding for Records.

#### Top-level

| STAC                 | OAR1                 | Profile*                                                     | Compatible / Comments                                        |
| -------------------- | -------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **type** = `Feature` | **type** = `Feature` | ✅                                                            | ✅                                                            |
| **stac_version**     | -                    | ✅                                                            | ⚠️                                                            |
| stac_extensions      | conformsTo           | ✅                                                            | ⚠️ STAC extensions could be used in `conformsTo`, but conformance classes that are not valid JSON schemas can't be used in `stac_extensions`. |
| **id**               | **id**               | ✅                                                            | ✅                                                            |
| **geometry**         | **geometry**         | ✅                                                            | ⚠️ STAC [disallows GeometryCollections](https://github.com/radiantearth/stac-spec/issues/1160) |
| ***bbox***           | bbox                 | ✅                                                            | ⚠️ STAC requires `bbox` if `geometry` is not `null`.          |
| **properties**       |                      | ✅                                                            |                                                              |
| **links**            | **links**            | ⚠️ [Issue 1](https://github.com/opengeospatial/ogcapi-records/issues/276) [Issue 2](https://github.com/opengeospatial/ogcapi-records/issues/275) | ⚠️ Structure is compatible except for templated links, but different relation type requirements (none in STAC; `self` in OAR1). |
| **assets**           | -                    | ✅                                                            | ⚠️ Assets are provided as links (aka "associations") in OAR1. |
| ***collection***     | -                    | ✅                                                            | ✅ This field is *required* in STAC if such a relation type is present and is *not allowed* otherwise. |
| -                    | **time**             | ⚠️ [Issue 1](https://github.com/opengeospatial/ogcapi-records/issues/277) [Issue 2](https://github.com/radiantearth/stac-spec/issues/1232) | ⚠️ STAC: `datetime` / `start_datetime` / `end_datetime` in `properties`. STAC can't encode all options that OAR1 allows. |

#### Properties

| STAC                                         | OAR1              | Profile*                                                     | Compatible                                                   |
| -------------------------------------------- | ----------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| -                                            | **type**          | ⚠️ [Issue](https://github.com/radiantearth/stac-spec/issues/1232) | ⚠️                                                            |
| **datetime** / start_datetime / end_datetime | -                 | ⚠️ [Issue 1](https://github.com/opengeospatial/ogcapi-records/issues/277) [Issue 2](https://github.com/radiantearth/stac-spec/issues/1232) | ⚠️ OAR1: `time` in the top-level object                       |
| title (common metadata)                      | **title**         | ⚠️ [Issue](https://github.com/radiantearth/stac-spec/issues/1232) | ⚠️                                                            |
| description (common metadata)                | description       | ✅                                                            | ⚠️ STAC disallows empty strings                               |
| keywords (common metadata)                   | keywords          | ✅                                                            | ✅                                                            |
| license (common metadata)                    | license           | ⚠️ [Issue](https://github.com/radiantearth/stac-spec/issues/1232) | ⚠️ STAC: SPDX identifier or `proprietary` or `various`; OAR1: SPDX identifier, SPDX expression, `other` or anything |
| created (common metadata)                    | created           | ✅                                                            | ✅                                                            |
| updated (common metadata)                    | updated           | ✅                                                            | ✅                                                            |
| contacts (extension)                         | contacts          | ✅                                                            | ✅                                                            |
| themes (extension)                           | themes            | ✅                                                            | ✅                                                            |
| language (extension)                         | language          | ✅                                                            | ✅                                                            |
| languages (extension)                        | languages         | ✅                                                            | ✅                                                            |
| -                                            | resourceLanguages | ✅                                                            | ✅                                                            |
| -                                            | externalIds       | ✅                                                            | ✅                                                            |
| -                                            | rights            | ✅                                                            | ✅                                                            |
| -                                            | formats           | ✅                                                            | ✅ STAC: A similar list can be obtained from the `type` field in assets |

## API (behavioral)

- The API for STAC is specified in the standalone STAC API specification and corresponding extensions.
- The API for OAR1 is specified in the requirement classes "Records API", "Searchable Catalogue" and "Local Resources Catalogue".

Both APIs use HTTP as the basis and encourage HTTPS.
HTTP 1.1 is required for OAP1, while STAC API doesn't explicitly define an HTTP version.
The APIs follow REST principles and make use of HTTP content negotiation.
The APIs make broad use of "Web Linking" (compatible between OAR1 and STAC API).
Both specifications recommend the implementation of CORS.

The default encoding for requests and response bodies is JSON. OGC API recommends to also support
HTML as an response body encoding.
STAC API usually uses client software to render HTML output from JSON (e.g., STAC Browser).

Both specifications make broad use of OpenAPI 3.0 (or later) and JSON Schema for specification purposes.

### Landing Page

### Conformance

### Collection List

### Individual Collection

### Item List

### Individual Item

### Search



The content model for STAC is specified in the standalone STAC API specification.