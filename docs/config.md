---
title: Application Configuration
layout: default
nav_order: 5
---
# Configure the GeoDiscovery Application

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

* Initial bounds and basemap of map is set in
[application.js](https://github.com/UWM-Libraries/GeoDiscovery/blob/main/app/assets/javascripts/application.js)
* [uwm.js](https://github.com/UWM-Libraries/GeoDiscovery/blob/main/app/assets/javascripts/uwm.js)
contains some UWM customizations like the scrolltop feature

## [./app/assets/stylesheets](https://github.com/UWM-Libraries/GeoDiscovery/tree/main/app/assets/stylesheets)

Various stylesheets to set color defaults, link colors, etc.
