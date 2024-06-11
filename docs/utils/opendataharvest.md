---
title: DCAT Open Data Harvester
layout: default
nav_order: 3
parent: GeoDiscovery Utilities
---

# OpenDataHarvest Tool

[GitHub Repo](https://github.com/UWM-Libraries/GeoDiscovery-Utils/tree/main/opendataharvest)

### Overview
The OpenDataHarvest tool is a component of the GeoDiscovery-Utils repository. This tool is designed to facilitate the harvesting and processing of open geospatial data for integration into the GeoDiscovery portal, a platform aimed at providing access to a wide range of geospatial datasets.

### Features
- **Automated Data Harvesting**: OpenDataHarvest automates the process of collecting geospatial data from various open data sources. This ensures that the GeoDiscovery portal is continually updated with the latest datasets available.
- **Data Transformation**: The tool includes functionalities to transform the harvested data into formats that are compatible with the GeoDiscovery portal, ensuring seamless integration.
- **Metadata Handling**: OpenDataHarvest handles metadata extraction and processing, ensuring that all datasets are accompanied by comprehensive metadata for better discoverability and usability.

### Usage
The tool can be integrated into workflows for regularly updating the GeoDiscovery portal with new and updated datasets. It is suitable for use by libraries, research institutions, and other organizations involved in managing geospatial data.

### Integration
OpenDataHarvest is part of a broader set of utilities in the GeoDiscovery-Utils repository, all of which support the functionalities of the GeoDiscovery portal. The tool is designed to work in conjunction with other components to provide a robust geospatial data management and discovery solution.

### Benefits
- **Efficiency**: Automates the repetitive task of data harvesting, saving time and resources.
- **Up-to-date Data**: Ensures that the GeoDiscovery portal remains current with the latest available geospatial data.
- **Enhanced Discoverability**: Through comprehensive metadata processing, the tool enhances the discoverability of datasets within the portal.

## The [config.yaml](https://github.com/UWM-Libraries/GeoDiscovery-Utils/blob/main/opendataharvest/config.yaml) file.

YAML is a human-readable data serialization language.
It is commonly used for configuration files.
When used in conjunction with a python script, python can fetch these values as a dictionary object, allowing easy
access to the values.

The opendataharvest tool gets both it's configuration parameters (e.g. where to store output and logs),
default values for fields,
and manifests of open data sites to harvest from.

### Configuration Options

```yaml
CONFIG:
  CATALOG: "DCAT_Sites" # TestSites, DCAT_Sites, or CKAN_Sites
  OUTPUTDIR: "opendataharvest/output_md"
  LOGDIR: "opendataharvest/log"
  DEFAULTBBOX: "opendataharvest/default_bbox.csv"
  MAXRETRY: 3
  SLEEPTIME: 2
  SCHEMA: "https://raw.githubusercontent.com/UWM-Libraries/GeoDiscovery/main/schema/geoblacklight-schema-aardvark.json"
```

### Localization and Default Values

```yaml
DEFAULT:
  MEMBEROF:
    - "AGSLOpenDataHarvest"
  RESOURCECLASS:
    - "Datasets"
  ACCESSRIGHTS: "public"
  MDVERSION: "Aardvark"
  LANG:
    - "English"
  PROVIDER: "American Geographical Society Library – UWM Libraries"
  SUPPRESSED: false
  RIGHTS:
    - Although this data is being distributed by the American Geographical Society Library at the University of Wisconsin-Milwaukee Libraries, no warranty expressed or implied is made by the University as to the accuracy of the data and related materials. The act of distribution shall not constitute any such warranty, and no responsibility is assumed by the University in the use of this data, or related materials.
  RESOURCETYPE:
    - "Digital maps"
  FORMAT: None
  DESCRIPTION: This dataset was automatically cataloged from the creator's Open Data Portal. In some cases, publication year and bounding coordinates shown here may be incorrect. Additional download formats may be available on the author's website. Please check the 'More details at' link for additional information.
```

Following a small section of test sites, the rest of the file has nested records for each of the Hubs or portals we harvest from.

### Example of a data portal in the YAML file

Here is an example of a record for the Wisconsin Department of Health Services Data Portal DCAT-compliant portal:

```yaml
  DHS_OpenData:
   CreatedBy: "Wisconsin Department of Health Services"
   SiteURL: "https://data.dhsgis.wi.gov/data.json"
   SiteName: "DHS"
   Spatial: ["Wisconsin", "United States"]
   DefaultBbox: "Wisconsin"
   MapList: ""
   AppList:
    - UUID: "e1ca38bf16f54fb8ac879b386dbce422" # Flood Risk Map
    - UUID: "861fc902539e436ebef7a86a10e9337b" # Immunization Map
    - UUID: "43ed2d88cf1348608230572166d76697" # Radon Map
   SkipList: 
    - UUID: "ca921d70bdd84ae8bc84cd09abd822d7" # link to census geography website 
    - UUID: "00883495714c42a9be53b76b24300c8e" # GIS data disclaimer 
    - UUID: "200036084844418bb3119d963cd7d98c" # OSDP Help?
    - UUID: "29c62b7a834944ef8196573c123d7a9d"
```

This stores the URL where we access the catalog information as SiteURL,
some basic metadata fields that we want to remain consistent such as SiteName and Spatial,
a default bounding box 
(defined in [default_bbox.csv](https://github.com/UWM-Libraries/GeoDiscovery-Utils/blob/main/opendataharvest/default_bbox.csv) by default)
in case the script is unable to parse spatial information from the dataset or the information is missing,
and three lists of Maps, Apps, and Skips.

The DCAT harvest script will assign special metadata attributes to datasets defined in these lists.
Datasets in the AppList will be assigned the Resource Class of "Websites".
Datasets in the MapList will be assigned the Resource Class of "Maps".

As the name implies, the script will skip over datasets listed in the skiplist.
These are typically links to other open data portals, placeholder records, and copies of data from other repositories
including ESRI basemaps. 
We don't want to ingest these into our portal, so we add them to the skiplist.

There are some datasets that have other elements such as `DatasetPrefix` that are not being used at this time.

## Reviewing a DCAT catalog using Open Refine

You can inspect a DCAT JSON api call using Open Refine:

1. Find a DCAT catalog like this one for Milwaukee County LIO:
[https://data-mclio.hub.arcgis.com/api/feed/dcat-us/1.1.json](https://data-mclio.hub.arcgis.com/api/feed/dcat-us/1.1.json)
2. In OpenRefine, under _Create project_, select _Web Addresses (URLs)_
3. Enter the DCAT catalog URL and click _Next_
4. You will be asked to Configure parsing options including specifying a _record path_.
See the image below for an example of such a record path:
![image](https://github.com/UWM-Libraries/GeoDiscovery-Documentation/assets/12561339/2c6f5ec3-1fad-45c5-a91d-124379b539fa)
5. After you click to specify the _dataset_ object as the record path,
the preview will update. If things look as expected in the preview,
create a project with a descriptive title.

{: .highlight }
Since JSON is not a "flat" way of storing data, the _Record_ view capabilities of OpenRefine are particularly useful here.
If you plan to store a CSV, it may be useful to
[Join Multi-Valued Cells](https://openrefine.org/docs/manual/cellediting#join-multi-valued-cells)
for _distribution_ and _keyword_ columns.


## Basic crosswalk mapping:

    Title:
        DCAT: title
        OGM Aardvark: dct_title_s

    Description:
        DCAT: description
        OGM Aardvark: dct_description_sm

    Keywords/Tags:
        DCAT: keyword
        OGM Aardvark: dct_subject_sm

    Publisher:
        DCAT: publisher
        OGM Aardvark: dct_publisher_sm

    Contact Point:
        DCAT: contactPoint
        OGM Aardvark: dct_contributor_sm

    Access Rights:
        DCAT: accessLevel
        OGM Aardvark: dct_accessRights_s

    Temporal Coverage:
        DCAT: temporal
        OGM Aardvark: dct_temporal_sm

    Spatial Coverage:
        DCAT: spatial
        OGM Aardvark: dct_spatial_sm

    Identifier:
        DCAT: identifier
        OGM Aardvark: dct_identifier_s

    Rights:
        DCAT: rights
        OGM Aardvark: dct_rights_sm

    Format:
        DCAT: format
        OGM Aardvark: dct_format_s

    Landing Page:
        DCAT: landingPage
        OGM Aardvark: dct_isPartOf_sm

