---
title: GeoCombine
layout: default
nav_order: 8
has_children: true
---

# Documentaiton for Geocombine

For the most up-to-date information, see the
[GeoCombine Repository](https://github.com/OpenGeoMetadata/GeoCombine).

Technical Documnetation for the 
[GeoCombine Gem](https://www.rubydoc.info/gems/geo_combine/0.1.0/GeoCombine)

GeoCombine is a Ruby toolkit for managing geospatial metadata.

It include tasks for cloning, updating, and indexing OpenGeoMetadata metadata.

It has a library for converting metadata between standards.

## Main Rake Tasks:

List rake tasks:

```bash
rake -T
```

Index metadata in tmp/OpenGeoMetadata:

```bash
bundle exec rake geocombine:index
```


Delete all sample data from solr:

```bash
bundle exec rake uwm:index:delete_all
```
