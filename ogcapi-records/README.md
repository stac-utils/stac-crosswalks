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
| Catalog    | Record Collection |
| Collection | Record Collection |
| Item       | Record            |

Nevertheless, you could also think about other mappings between the specifications. Some may argue, a STAC Collection is a OAR1 Record. Due to the way the JSON representations are defined, I'm assuming a mapping as in the table above though.

Please also note that Catalogs, Collections and Items could be Records in OAR1 and some implementations only convert Records to Record Collections once they have at least one child record. Catalogs are currently mostly used in static deployments and don't play a huge role in APIs/dynamic deployments.

## Static (content)

- The content model and static catalog behavior for STAC is specified in the standalone STAC specification and corresponding extensions.
- The content model and static catalog behavior for OAR1 is specified in the requirement classes "Record Core", "Record Collection" and "Crawlable Catalogue".

***Note:*** In the tables below, the following applies:
- **bold** properties indicate that they are required.
- ***bold and italic*** properties are required if a certain criteria is met
- ✅ means that fields are fully compatible.
- ⚠️ means that fields are generally compatible (if applicable), but the field is not required in both specs or there are other minor differences implementors need to take care of.
- ❌ means there is a conflict in the specifications, the generated JSON is NOT compatible.
- **Profile\*** = Checks the compliance if STAC would be defined as an OGC API - Records - Part 1 profile / extension.

### Catalogs

Assuming a JSON (non-GeoJSON) encoding.

| STAC                 | OAR1                    | Profile*                                                     | Compatible / Comments                                        |
| -------------------- | ----------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **type** = `Catalog` | **type** = `Collection` | ⚠️ [Issue](https://github.com/opengeospatial/ogcapi-records/issues/264) | ⚠️ There is no Catalog in OAR1, you can use a "lightweight" Collection instead. |
| **stac_version**     | -                       | ✅                                                            | ⚠️                                                            |
| stac_extensions      | -                       | ✅                                                            | ✅                                                            |
| **id**               | **id**                  | ✅                                                            | ✅                                                            |
| title                | **title**               | ⚠️ [Issue](https://github.com/radiantearth/stac-spec/issues/1232) | ⚠️ Potentially require in STAC?                               |
| **description**      | **description**         | ✅                                                            | ⚠️ STAC disallows empty strings                               |
| **links**            | **links**               | ⚠️ [Issue 1](https://github.com/opengeospatial/ogcapi-records/issues/276) [Issue 2](https://github.com/opengeospatial/ogcapi-records/issues/275) [Issue 3](https://github.com/radiantearth/stac-spec/issues/1235) | ⚠️ Structure is compatible except for templated links and different relation type requirements (none in STAC; `self` in OAR1). Also, OAR1 and STAC both use the relation type `child` with a media type `application/json` to link to catalogs, but **clients can't distinguish whether they can expect OAR1 or STAC**. |

See below for more details about Collections.

### Collections

Assuming a JSON (non-GeoJSON) encoding.

| STAC                      | OAR1                                | Profile*                                                     | Compatible / Comments                                        |
| ------------------------- | ----------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **type** = `Collection`   | **type** = `Collection`             | ✅                                                            | ✅                                                            |
| -                         | **itemType** = `record` (API only?) | ✅                                                            | ✅                                                            |
| **stac_version**          | -                                   | ✅                                                            | ⚠️                                                            |
| stac_extensions           | conformsTo (tbc)                    | ✅                                                            | ⚠️ STAC extensions could be used in `conformsTo`, but conformance classes that are not valid JSON schemas can't be used in `stac_extensions`. |
| **id**                    | **id**                              | ✅                                                            | ✅                                                            |
| title                     | **title**                           | ⚠️ [Issue](https://github.com/radiantearth/stac-spec/issues/1232) | ⚠️ Potentially require in STAC?                               |
| **description**           | description                         | ✅                                                            | ⚠️ STAC disallows empty strings                               |
| **links**                 | **links**                           | ⚠️ [Issue 1](https://github.com/opengeospatial/ogcapi-records/issues/276) [Issue 2](https://github.com/opengeospatial/ogcapi-records/issues/275) [Issue 3](https://github.com/radiantearth/stac-spec/issues/1235) | ⚠️ Structure is compatible except for templated links, but different relation type requirements (none in STAC; `self` and `root` in OAR1). Also, OAR1 and STAC both use the relation type `child` with a media type `application/json` to link to collections, but **clients can't distinguish whether they can expect OAR1 or STAC**. |
| keywords                  | keywords                            | ✅                                                            | ✅                                                            |
| **license**               | license                             | ⚠ [Issue](https://github.com/radiantearth/stac-spec/issues/1232) | ⚠ STAC: SPDX identifier or `proprietary` or `various`; OAR1: SPDX identifier, SPDX expression or `other` |
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
| **properties**       | **properties**       | ✅                                                            |                                                              |
| **links**            | **links**            | ⚠️ [Issue 1](https://github.com/opengeospatial/ogcapi-records/issues/276) [Issue 2](https://github.com/opengeospatial/ogcapi-records/issues/275) [Issue 3](https://github.com/radiantearth/stac-spec/issues/1235) | ⚠️ Structure is compatible except for templated links, but different relation type requirements (none in STAC; `self` in OAR1). Also, OAR1 and STAC both use the relation type `item` with a media type `application/geo+json` to link to items/records, but **clients can't distinguish whether they can expect OAR1 or STAC**. |
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
| license (common metadata)                    | license           | ⚠️ [Issue](https://github.com/radiantearth/stac-spec/issues/1232) | ⚠️ STAC: SPDX identifier or `proprietary` or `various`; OAR1: SPDX identifier, SPDX expression or `other` |
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

- The API for STAC is specified in the standalone STAC API specification and corresponding extensions (conformance classes Core, Collections, Features).
- The API for OAR1 is specified in the requirement classes "Records API", "Searchable Catalogue" and "Local Resources Catalogue".

Both APIs base their work on OGC API - Features and OGC API - Common.

The APIs use HTTP as the basis and encourage HTTPS.
HTTP 1.1 is required for OAP1, while STAC API doesn't explicitly define an HTTP version.
The APIs follow REST principles and make use of HTTP content negotiation.
The APIs make broad use of "Web Linking" (compatible between OAR1 and STAC API).
Both specifications recommend the implementation of CORS.

The default encoding for requests and response bodies is JSON. OGC API recommends to also support
HTML as an response body encoding, thus content negotiation needs to be implemented more carefully for OAR1.
STAC API usually uses client software to render HTML output from JSON (e.g., STAC Browser).

Both specifications make broad use of OpenAPI 3.0 (or later) and JSON Schema for specification purposes.

### Landing Page

- STAC: `GET /` (required)
- OAR1: `GET /` (required)

As the landing pages both are based on top of [OGC API - Common](https://docs.ogc.org/is/19-072/19-072.html), they are very similar.

In OAR1 you must provide just `links` and can optionally add `title` and `description`.

STAC API requires additional properties (`stac_version`, `type`, `id`, `description`) to form a full STAC [Catalog](#catalogs). It also lists the conformance classes in the landing page, while OAR1 has them [separate](#conformance).

The use of links is a bit different in STAC API and OAR1. The implementation of the [Collection List](#collection-list) is optional in STAC. A STAC API can also just expose child links to catalogs/collections and/or to a search endpoint.

### Conformance

- STAC: `GET /conformance` (optional)
- OAR1: `GET /conformance` (required)

Both endpoints are 100% equivalent.
OAR1 requires a separate endpoint that lists conformance classes, which is optional in STAC API.
STAC API additionally lists the conformance classes in the landing page.

### Collection List

- STAC: `GET /collections` (optional)
- OAR1: `GET /collections` (required)

As the endpoints for collection lists are both based on top of [OGC API - Features - Part 1](https://docs.ogc.org/is/17-069r3/17-069r3.html), they are very similar. The difference between STAC and OAR1 is how the individual collections are encoded, see [Collections](#collections) for details.

### Individual Collection

- STAC: `GET /collections/{collectionId}` (optional)
- OAR1: `GET /collections/{collectionId}` (required)

As the endpoints for individual collections are both based on top of [OGC API - Features - Part 1](https://docs.ogc.org/is/17-069r3/17-069r3.html), they are very similar. The difference between STAC and OAR1 is how the individual collections are encoded, see [Collections](#collections) for details.

### Item List

- STAC: `GET /collections/{collectionId}/items` (optional)
- OAR1: `GET /collections/{collectionId}/items` (required)

As the endpoints for item lists (per collection) are both based on top of [OGC API - Features - Part 1](https://docs.ogc.org/is/17-069r3/17-069r3.html), they are very similar. The difference between STAC and OAR1 is how the individual items are encoded, see [Items](#items-/-records) for details. Additionally, OAR1 defines the following parameters that implementations must support: `q`, `type` and `externalid`.

### Individual Item

- STAC: `GET /collections/{collectionId}/items/{itemId}` (optional)
- OAR1: `GET /collections/{collectionId}/items/{recordId}` (required)

As the endpoints for individual items are both based on top of [OGC API - Features - Part 1](https://docs.ogc.org/is/17-069r3/17-069r3.html), they are very similar. The difference between STAC and OAR1 is how the individual items are encoded, see [Items](#items-/-records) for details.

### Search

STAC defines three ways to search for resources:

- [**Collection Search Extension**](https://github.com/stac-api-extensions/collection-search/): Search for collections via `GET /collections` (based on Local Resource Catalogue in OAR1)
- [**Item Search**](https://github.com/radiantearth/stac-api-spec/tree/main/item-search): Search (globally) for items across collections via `GET /search` and `POST /search`
- **Items per Collection**: Filter for items in a specific collection via `GET /collections/{collectionId}/items` (see [Item List](#item-list))

OAR1 defines similar ways to search for resources:

- [**Local Resource Catalogue**](https://docs.ogc.org/DRAFTS/20-004.html#clause-local-resources-catalogue): Search for record collections via `GET /collections`
- **Record Search**: A global search for records is planned in the requirement class [Searchable Catalogue](https://docs.ogc.org/DRAFTS/20-004.html#clause-searchable-catalogue), but it's not fully defined yet. There are [chances for incompatibilities with STAC](https://github.com/opengeospatial/ogcapi-features/issues/832).
- **Records per Record Collection**: Filter for records in a specific record collection via `GET /collections/{collectionId}/items`  (see [Item List](#item-list))

### Datacubes

OAR1 doesn't prescribes how to provide metadata about datacubes. OGC in general has a couple of related standards, e.g. Coverages and Geodatacube API, but the relation with OAR1 is not clearly defined for any of them.

STAC doesn't provide it either in the core, but it has a [datacube extension](https://github.com/stac-extensions/datacube).