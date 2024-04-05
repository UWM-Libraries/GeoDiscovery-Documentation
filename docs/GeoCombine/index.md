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

## CRON job rake tasks related to GeoCombine:

GeoCombine rake tasks are scheduled via the [Whenever](https://uwm-libraries.github.io/GeoDiscovery-Documentation/docs/dependencies.html#whenever)
gem in [config/schedule.rb](https://github.com/UWM-Libraries/GeoDiscovery/blob/main/config/schedule.rb).

```ruby
# Updates the UWM OpenGeoMetadata directory (git pull) and re-index
every :monday, at: "4:00 am", roles: [:app] do
  # Ours
  rake "geocombine:pull[edu.uwm]"
  # Direct from GeoCombine
  rake "geocombine:pull[edu.uchicago]"
  rake "geocombine:pull[edu.illinois]"
  rake "geocombine:pull[edu.indiana]"
  rake "geocombine:pull[edu.uiowa]"
  rake "geocombine:pull[edu.umd]"
  rake "geocombine:pull[edu.msu]"
  rake "geocombine:pull[edu.umn]"
  rake "geocombine:pull[edu.unl]"
  rake "geocombine:pull[edu.nyu]"
  rake "geocombine:pull[edu.osu]"
  rake "geocombine:pull[edu.psu]"
  rake "geocombine:pull[edu.purdue]"
  rake "geocombine:pull[edu.rutgers]"
  rake "geocombine:pull[edu.umich]"
  # Metadata We've Converted
  rake "geocombine:pull[edu.wisc.aardvark]"
  rake "geocombine:pull[edu.uwm.converted]"
  # Index
  rake "geocombine:index"
end
```

## Main Rake Tasks:

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
