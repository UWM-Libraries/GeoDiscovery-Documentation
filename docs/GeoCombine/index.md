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

{: .note }
> Rake tasks are run in the application directory. On the Development and Production environments, this is found in:
> `/var/www/rubyapps/uwm-geoblacklight/current`
> In your local environment, you just run it in the GeoDiscovery directory.
>

Index metadata in tmp/OpenGeoMetadata:

```bash
bundle exec rake geocombine:index
```

{: .note }
> Depending on the environment, you may need to specify the SOLR_URL and SCHEMA_VERSION environment variables.
>
> ```bash
>SOLR_URL=http://127.0.0.1:8983/solr/test SCHEMA_VERSION=Aardvark bundle exec rake geocombine:index
>```
>

Delete all sample data from solr:

```bash
bundle exec rake uwm:index:delete_all
```
