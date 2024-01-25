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

| Name        | Field Name | Implementation |
| ----------- | ----------- | ----------- |
| [Title](https://opengeometadata.org/ogm-aardvark/#title) | `dct_title_s` | The title is the most prominent metadata field that users see when browsing or scanning search results. Since many datasets are created with ambiguous or non-unique titles, it may be worth the effort to improve or enhance them. The ideal sequence of a title is something akin to "Topic of Layer: Place, Year." Putting the year at the end of a title produces better search results, since titles are left-anchored.
| [Resource Class](https://opengeometadata.org/ogm-aardvark/#resource-class)   | `gbl_resourceClass_sm` | This field is intended to help users sort between significantly different types of resources.
| [Access Rights](https://opengeometadata.org/ogm-aardvark/#access-rights) | `dct_accessRights_s` | This field can be set to "Public", which allows users to view and download an item, or "Restricted", which requires a user to log in to an authentication service.
| [ID](https://opengeometadata.org/ogm-aardvark/#id) | `id` | This field makes up the URL for the resource in GeoBlacklight. It is visible to the user and is used to create permalinks. If having a readable slug is desired, it is common to use the form institution-keyword1-keyword2.
| [Modified](https://opengeometadata.org/ogm-aardvark/#modified) | `gbl_mdModified_dt` | This value should indicate when the metadata (not the resource itself) was last modified.
| [Metadata Version](https://opengeometadata.org/ogm-aardvark/#metadata-version) | `gbl_mdVersion_s` | There have been two metadata schema versions for GeoBlacklight applications: GeoBlacklight 1.0 (abbreviated to GBL 1.0) and OpenGeoMetadata Aardvark (abbreviated to Aardvark).

