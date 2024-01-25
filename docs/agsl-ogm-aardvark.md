---
title: AGSL Aardvark
layout: default
nav_order: 6
---

# OGM Aardvark

AGSL Geodiscovery uses the [OpenGeoMetadata Aardvark Schema](https://opengeometadata.org/about-ogm-aardvark/).
The schema is based on Dublin Core with custom elements for GeoBlacklight geoportals and is focused on Discovery rather than description/documentation.
The GeoBlacklight application uses Solr to ingest JSON formatted metadata.

Some institutions, notably UW-Madison, have not yet moved to the Aardvark Schema and are instead using GeoBlacklight Schema 1.0.
Madison has plans to move to GeoBlacklight version 4.x and to the Aardvark Schema in March 2024.

## AGSL Implementation of the OGM Aardvark Metadata Schema

The full OGM Aardvark schema description can be accessed [here](https://opengeometadata.org/ogm-aardvark/).

The purpose of the remainder of this page is to document the AGSL's implementation of the schema for our Digital Spatial Data Collection,
for indexing of metadata harvested from open data portals,
and potentially how we manipulate and ingest metadata from OpenGeoMetadata.

### Required Fields

<!--- Generate a table using this tool: https://jakebathman.github.io/Markdown-Table-Generator/. Use agsl-ogm-aardvark.csv to edit. -->

**Name (bold: required by GBL)**|**Field Name**|**Implementation**
-----|-----|-----
**[Title](https://opengeometadata.org/ogm-aardvark/#title)**|dct\_title\_s|We use Madison's filenaming convention, e.g. "Parks Milwaukee County, Wisconsin 2009". Some use a date range. *Geography\_Theme\_Year*
**[Resource Class](https://opengeometadata.org/ogm-aardvark/#resource-class)**|gbl\_resourceClass\_sm|Controlled vocab, e.g. Collections, Datasets, Imagery, Maps, Web services, Websites, Other. We mostly use Datasets. Web services and Maps will likley be used too.
**[Access Rights](https://opengeometadata.org/ogm-aardvark/#access-rights)**|dct\_accessRights\_s|Public or Restricted. How can we use to differentiate between UW and UWM restriction?? Or should we use "rights" for that?
**[ID](https://opengeometadata.org/ogm-aardvark/#id)**|id|This field makes up the URL for the resource in GeoBlacklight. It is visible to the user and is used to create permalinks. If having a readable slug is desired, it is common to use the form institution-keyword1-keyword2.
**[Modified](https://opengeometadata.org/ogm-aardvark/#modified)**|gbl\_mdModified\_dt|This value should indicate when the metadata (not the resource itself) was last modified.
**[Metadata Version](https://opengeometadata.org/ogm-aardvark/#metadata-version)**|gbl\_mdVersion\_s|There have been two metadata schema versions for GeoBlacklight applications: GeoBlacklight 1.0 (abbreviated to GBL 1.0) and OpenGeoMetadata Aardvark (abbreviated to Aardvark).
[Description](https://opengeometadata.org/ogm-aardvark/#description)|dct\_description\_sm|second-most prominent value (after Title). User readable text that describes the dataset.
[Language](https://opengeometadata.org/ogm-aardvark/#language)|dct\_language\_sm|"eng" for most, we get this from the ISO.
[Creator](https://opengeometadata.org/ogm-aardvark/#creator)|dct\_creator\_sm|We use the ISO "source" for this. For example, Metro Data Resource Center. Can be multivalued.
[Provider](https://opengeometadata.org/ogm-aardvark/#provider)|schema\_provider\_s|Always "American Geographical Society Library – UWM Libraries", this is attached to our little SVG icon that displays in other portals!
[Resource Type](https://opengeometadata.org/ogm-aardvark/#resource-type)|gbl\_resourceType\_sm|Line data, Polygon Data, etc.
[Theme](https://opengeometadata.org/ogm-aardvark/#theme)|dcat\_theme\_sm|Transportation, Society, etc.
[Temporal Coverage](https://opengeometadata.org/ogm-aardvark/#temporal-coverage)|dct\_temporal\_sm|We're storing an ISO date here, but it is intedned to be human readable. Maybe we should rethink this one?
[Date Issued](https://opengeometadata.org/ogm-aardvark/#date-issued)|dct\_issued\_s|This fills the "Date Issued" field on the show page.
[Index Year](https://opengeometadata.org/ogm-aardvark/#index-year)|gbl\_indexYear\_im|The "main" year. Doesn't appear to the user except in the Year facet. Should be the same as year in the title (not for range?)
[Spatial Coverage](https://opengeometadata.org/ogm-aardvark/#spatial-coverage)|dct\_spatial\_sm|Spatial Keywords, e.g. "Milwaukee", "Wisconsin", "Portland Metro", etc. These get shown to the user and linked to the "place" facet.
[Geometry](https://opengeometadata.org/ogm-aardvark/#geometry)|locn\_geometry|Powers the map search. ENVELOPE or WKT syntax ENVELOPE(W,E,N,S) Wrong on some records!
[Bounding Box](https://opengeometadata.org/ogm-aardvark/#bounding-box)|dcat\_bbox|Should also be WENS, overlap ratio boosting in spatial searches.
[Rights](https://opengeometadata.org/ogm-aardvark/#rights\_1)|dct\_rights\_sm|We use our standard "please see" link and standard language about digital colelctions and copyright. OGM specifies it should be used for more free text description of access rights, etc. Could expand beyond current boilerplate use.
[Format](https://opengeometadata.org/ogm-aardvark/#format)|dct\_format\_s|[Controlled vocab](https://opengeometadata.org/ogm-aardvark/#format-values), e.g. shapefile, GeoJSOB, GeoTIFF, etc.
[References](https://opengeometadata.org/ogm-aardvark/#references)|dct\_references\_s|defines external services and references using the CatInterOp approach,  serialized JSON array of key/value pairs. [List of Reference URIs supported](https://opengeometadata.org/reference-uris/)
[Identifier](https://opengeometadata.org/ogm-aardvark/#identifier)|dct\_identifier\_sm|We display our ArkID here
[Supressed](https://opengeometadata.org/ogm-aardvark/#suppressed)|gbl\_suppressed\_b|We have this as "false" for everything right now, but could use it to hide records.