---
title: Stack and Dependencies
layout: default
nav_order: 1.1
---
# Stack and Dependencies

## GeoBlacklight

see [geoblacklight.org](https://geoblacklight.org/)

Open-source, Ruby on Rails software application for discovery geospatial content based on the open-source software project Blacklight.

{: .note }
Blacklight and GeoBlacklight are unlike many Ruby on Rails applications in that they use Apache Solr as a data store rather than a relational database.

## Blacklight 

see [projectblacklight.org](http://projectblacklight.org/)

An open-source, Ruby on Rails engine that provides a basic discovery interface for searching an Apache Solr index, including fielded searching, applyying and removing facet constraints, sorting and paginating through search results, and more.

## Ruby on Rails

see [rubyonrails.org](https://rubyonrails.org/)

Sever-side web application framework written in Ruby that uses the MVC Framework (model-view-controller). Popular websites like GitHub, Twitch, and AirBNB are built using Ruby on Rails.


## Apache Solr

see [solr.apache.org](https://solr.apache.org/)

An open-source enterprise-search platform written in Java. Features include full-text search, hit highlighting, faceted search, real time indexing, dynamic clustering, database integration, and rich document handling.

Blacklight and GeoBlacklight use Solr as a data store. Metadata is ingested and indexed in Solr.

## Capistrano

see [https://capistranorb.com/](https://capistranorb.com/)

A remote server automation tool that supports the scripting and execution of arbitrary tasks and includes a set of sane-default deployment workflows.

## Sprockets

see [GitHub Repo](https://github.com/rails/sprockets)

Sprockets is a Ruby library for compiling and serving web assets. It features declarative dependency management for JavaScript and CSS assets, as well as a powerful preprocessor pipeline that allows you to write assets in languages like CoffeeScript, Sass and SCSS.

## sqlite3

see [sqlite.org/](https://sqlite.org/index.html)

A small, fast, self-contained SQL database engine. Used in the dev and test environment as a database.

## Puma

see [GitHub Repo](https://github.com/puma/puma)

Web Server used for the dev and test environments

## Phusion Passenger

see [physionpassenger.com](https://www.phusionpassenger.com/)

A.K.A mod_rails and mod_rack

A free web server and application server with support for Ruby, Python, and Node.js. Used in the production environment.

## sidekiq

see [sidekiq.org](https://sidekiq.org)

An open-source job scheduler written in ruby. 

## redis

see [redis.io](https://redis.io/)

An in-memory data structure store, used as a distributed, in-memory-key-value database, cache, and message broker.


