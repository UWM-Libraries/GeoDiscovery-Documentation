---
title: Application Configuration
layout: default
nav_order: 5
---
# Configure the GeoDiscovery Application

## [./app/controllers/catalog_controller.rb](https://github.com/UWM-Libraries/GeoDiscovery/blob/main/app/controllers/catalog_controller.rb)

Probably the file we would use the most for simple configurations.

`configure_blacklight` block is where we make most configuration settings such as critical Solr settings and paramters.

Default rows returned from Solr.

* Default Facets (e.g. `config.add_facet_field Settings.Fields.INDEX_YEAR, label: "Year", limit: 10`)
    * See [settings.yml](#configsettingsyml) to see where these are coming from.
    * Decide if facets are collapsed by default
    * What component will render component?
    * The order they appear is the order they will render on the UI
* Item Relationships
* Search Results Field
* Item Show page/View page display
    * Most Aardvark fields shown by default, we may decide we want to add/remove
* Sort values
* Show tools (SMS, Email)
* Basemap for the map

These things might be rendered through a helper method. Examples including truncating abstracts or the helper that renders HTML in the rights field (the link to our Copyright and Digital Collections policy page).

## [./config/settings.yml](https://github.com/UWM-Libraries/GeoDiscovery/blob/main/config/settings.yml)

* Set the metadata schmea version, currently _Aardvark_
* solr field mappings
* Institutional text
* Relationship field management
* Leaflet Settings

## [./config/schedule.rb](https://github.com/UWM-Libraries/GeoDiscovery/blob/main/config/schedule.rb)

[Redis](https://uwm-libraries.github.io/GeoDiscovery-Documentation/docs/dependencies.html#redis)
and
[Sidekiq](https://uwm-libraries.github.io/GeoDiscovery-Documentation/docs/dependencies.html#sidekiq)
use this file to run scheduled tasks.

* Harvest thumbnail images for search results
* Build the sitemap
* Clean up anonymous user accounts created by search sessions
* Clean up recent anonymous search records

## [./app/assets/images](https://github.com/UWM-Libraries/GeoDiscovery/tree/main/app/assets/images)

Storage for images used in the app:

* favicons
* UWM Logos
* UWM SVG

## [./app/assets/javascripts](https://github.com/UWM-Libraries/GeoDiscovery/tree/main/app/assets/javascripts)

* Initial bounds of map is set in
[application.js](https://github.com/UWM-Libraries/GeoDiscovery/blob/main/app/assets/javascripts/application.js)
* [uwm.js](https://github.com/UWM-Libraries/GeoDiscovery/blob/main/app/assets/javascripts/uwm.js)
contains some UWM customizations like the scrolltop feature

## [./app/assets/stylesheets](https://github.com/UWM-Libraries/GeoDiscovery/tree/main/app/assets/stylesheets)

Various stylesheets to set color defaults, link colors, etc.












