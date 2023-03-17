---
title: Stack and Dependencies
layout: default
nav_order: 1.1
---
# Stack and Dependencies

## GeoBlacklight

[geoblacklight.org](https://geoblacklight.org/)

Open-source, Ruby on Rails software application for discovery geospatial content based on the open-source software project Blacklight.

{: .note }
Blacklight and GeoBlacklight are unlike many Ruby on Rails applications in that they use Apache Solr as a data store rather than a relational database.

## Blacklight 

[projectblacklight.org](http://projectblacklight.org/)

An open-source, Ruby on Rails engine that provides a basic discovery interface for searching an Apache Solr index, including fielded searching, applyying and removing facet constraints, sorting and paginating through search results, and more.

## Ruby on Rails

[rubyonrails.org](https://rubyonrails.org/)

Sever-side web application framework written in Ruby that uses the MVC Framework (model-view-controller). Popular websites like GitHub, Twitch, and AirBNB are built using Ruby on Rails.


## Apache Solr

[solr.apache.org](https://solr.apache.org/)

An open-source enterprise-search platform written in Java. Features include full-text search, hit highlighting, faceted search, real time indexing, dynamic clustering, database integration, and rich document handling.

Blacklight and GeoBlacklight use Solr as a data store. Metadata is ingested and indexed in Solr.

## GeoCombine

[GeoCombine Repo](https://github.com/OpenGeoMetadata/GeoCombine)

A Ruby toolkit for managing geospatial metadata, including:
* tasks for cloning, updating, and indexing OpenGeoMetdata metadata
* library for converting metadata between standards

{: .note }
> AGSL Geodiscovery is using [UWM's fork of GeoCombine](https://github.com/UWM-Libraries/GeoCombine)
> 
> This can be modified by editing the Gemfile:
> 
> ```ruby
> # GeoCombine via the UWM Libraries Fork
> gem "geo_combine", git: "https://github.com/UWM-Libraries/GeoCombine.git", branch: "main"
> ```
> 

## Bootstrap

[getbootstrap.com](https://getbootstrap.com)

Open-source CSS framework directed at responsive, mobile-first front-end web development. It contains HTML, CSS and JavaScript-based design templates for typography, forms, buttons, navigation, and other interface components.

## Capistrano

[https://capistranorb.com/](https://capistranorb.com/)

A remote server automation tool that supports the scripting and execution of arbitrary tasks and includes a set of sane-default deployment workflows.

## Sprockets

[Sprockets GitHub Repo](https://github.com/rails/sprockets)

Sprockets is a Ruby library for compiling and serving web assets. It features declarative dependency management for JavaScript and CSS assets, as well as a powerful preprocessor pipeline that allows you to write assets in languages like CoffeeScript, Sass and SCSS.

## sqlite3

[sqlite.org/](https://sqlite.org/index.html)

A small, fast, self-contained SQL database engine. Used in the dev and test environment as a database.

## Puma

[Puma GitHub Repo](https://github.com/puma/puma)

Web Server used for the dev and test environments

## Phusion Passenger

[physionpassenger.com](https://www.phusionpassenger.com/)

A.K.A mod_rails and mod_rack

A free web server and application server with support for Ruby, Python, and Node.js. Used in the production environment.

## GeoBlacklight Sidecar Images

[Repo](https://github.com/geoblacklight/geoblacklight_sidecar_images)

This GeoBlacklight plugin captures remote images from geographic web services and saves them locally. It borrows the concept of a SolrDocumentSidecar from Spotlight, to have an ActiveRecord-based "sidecar" to match each non-AR SolrDocument. This allows us to use ActiveStorage to attach images to our solr documents.

See [ActiveJob](#activejob)
,
[Sidekiq](#sidekiq)
, and
[Redis](#redis)
below.

## ActiveJob

[see docs](https://guides.rubyonrails.org/active_job_basics.html)

Active Job is a framework for declaring jobs and making them run on a variety of queuing backends. These jobs can be everything from regularly scheduled clean-ups, to billing charges, to mailings. Anything that can be chopped up into small units of work and run in parallel, really.

## Sidekiq

[sidekiq.org](https://sidekiq.org)

An open-source job scheduler written in ruby. 

## Redis

[redis.io](https://redis.io/)

An in-memory data structure store, used as a distributed, in-memory-key-value database, cache, and message broker.

## Minitest

[Minitest Docs](https://docs.seattlerb.org/minitest/)

A complete suite of testing facilities supporting TDD, BDD, mocking, and benchmarking. 

## Capybara

[Capybara Docs](teamcapybara.github.io/capybara/)

System tests run via Capybara and a headless web browser, essentially loading a webpage in
the background and ensuring text or CSS selectors or XPATH expressions exist on the page.

