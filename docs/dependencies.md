---
title: Stack and Dependencies
layout: default
nav_order: 1.1
---
# Stack and Dependencies

* [ActiveJob](#activejob)
* [Advanced Search Plugin](#advanced-search-plugin-for-blacklight)
* [Apache Solr](#apache-solr)
* [Blacklight](#blacklight)
* [Bootstrap](#bootstrap)
* [Bundler](#bundler)
* [Capistrano](#capistrano)
* [Capybara](#capybara)
* [Dependabot](#dependabot)
* [Exception Notification](#exception-notification)
* [GeoBlacklight](#geoblacklight)
* [GeoBlacklight Sidecar Images](#geoblacklight-sidecar-images)
* [GeoCombine](#geocombine)
* [MariaDB](#mariadb)
* [Minitest](#minitest)
* [Passenger](#phusion-passenger)
* [Puma](#puma)
* [Redis](#redis)
* [Ruby on Rails](#ruby-on-rails)
* [Sidekiq](#sidekiq)
* [Sprockets](#sprockets)
* [sqlite3](#sqlite3)
* [Whenever](#whenever)

## GeoBlacklight

[geoblacklight.org](https://geoblacklight.org/)

Open-source, Ruby on Rails software application for discovery geospatial content based on the open-source software project Blacklight.

{: .note }
Blacklight and GeoBlacklight are unlike many Ruby on Rails applications in that they use Apache Solr as a data store rather than a relational database.

[Top](#stack-and-dependencies)

## Blacklight 

[projectblacklight.org](http://projectblacklight.org/)

An open-source, Ruby on Rails engine that provides a basic discovery interface for searching an Apache Solr index, including fielded searching, applyying and removing facet constraints, sorting and paginating through search results, and more.

[Top](#stack-and-dependencies)

## Ruby on Rails

[rubyonrails.org](https://rubyonrails.org/)

Sever-side web application framework written in Ruby that uses the MVC Framework (model-view-controller). Popular websites like GitHub, Twitch, and AirBNB are built using Ruby on Rails.

[Top](#stack-and-dependencies)

## Bundler

[bundler.io](https://bundler.io/)

Bundler manages an application's dependencies through its entire life, across many machines, systematically and repeatably. 
[Read more](https://rubygems.org/gems/bundler)

[Top](#stack-and-dependencies)

## Apache Solr

[solr.apache.org](https://solr.apache.org/)

An open-source enterprise-search platform written in Java. Features include full-text search, hit highlighting, faceted search, real time indexing, dynamic clustering, database integration, and rich document handling.

Blacklight and GeoBlacklight use Solr as a data store. Metadata is ingested and indexed in Solr.

[Top](#stack-and-dependencies)

## MariaDB

[mariadb.org](https://mariadb.org/)

Our production database for the application. MariaDB is a community-developed, commercially supported fork of the MySQL relational database management system.

[Top](#stack-and-dependencies)

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

[Top](#stack-and-dependencies)

## Bootstrap

[getbootstrap.com](https://getbootstrap.com)

Open-source CSS framework directed at responsive, mobile-first front-end web development. It contains HTML, CSS and JavaScript-based design templates for typography, forms, buttons, navigation, and other interface components.

[Top](#stack-and-dependencies)

## Capistrano

[https://capistranorb.com/](https://capistranorb.com/)

A remote server automation tool that supports the scripting and execution of arbitrary tasks and includes a set of sane-default deployment workflows.

Since we are deploying from a local computer, capistrano is not actually installed on the production machine.

[Top](#stack-and-dependencies)

## Sprockets

[Sprockets GitHub Repo](https://github.com/rails/sprockets)

Sprockets is a Ruby library for compiling and serving web assets. It features declarative dependency management for JavaScript and CSS assets, as well as a powerful preprocessor pipeline that allows you to write assets in languages like CoffeeScript, Sass and SCSS.

Sprockets is a gem from the RoR community that helps you to bundle your JS and CSS together in the project.
It's an old gem and it's as of Rails version 7 being phased out. It's the one GeoBlacklight currently uses.
See the Ruby on [Rails Asset Pipeline](https://guides.rubyonrails.org/asset_pipeline.html) guides for more info.

[Top](#stack-and-dependencies)

## sqlite3

[sqlite.org/](https://sqlite.org/index.html)

A small, fast, self-contained SQL database engine. Used in the dev and test environment as a database.

[Top](#stack-and-dependencies)

## Puma

[Puma GitHub Repo](https://github.com/puma/puma)

Web Server used for the dev and test environments

[Top](#stack-and-dependencies)

## Phusion Passenger

[physionpassenger.com](https://www.phusionpassenger.com/)

A.K.A mod_rails and mod_rack

A free web server and application server with support for Ruby, Python, and Node.js. Used in the production environment.

[Top](#stack-and-dependencies)

## Advanced Search Plugin for Blacklight

[Advanced Search Plugin for Blacklight Repo](https://github.com/projectblacklight/blacklight_advanced_search)

This is an advanced search plugin for Blacklight. 

{: .note }
> We're currently using a forked version of this plugin.
> 
> ```ruby
> gem "blacklight_advanced_search", git: "https://github.com/ewlarson/blacklight_advanced_search.git", branch: "bl7-fix-gentle-hands"
> ```
>
> We can use the main gem once [this pull request](https://github.com/projectblacklight/blacklight_advanced_search/pull/128) is incorporated into the main repo.

[Top](#stack-and-dependencies)

## GeoBlacklight Sidecar Images

[Repo](https://github.com/geoblacklight/geoblacklight_sidecar_images)

This GeoBlacklight plugin captures remote images from geographic web services and saves them locally. It borrows the concept of a SolrDocumentSidecar from Spotlight, to have an ActiveRecord-based "sidecar" to match each non-AR SolrDocument. This allows us to use ActiveStorage to attach images to our solr documents.

See [ActiveJob](#activejob)
,
[Sidekiq](#sidekiq)
, and
[Redis](#redis)
below.

[Top](#stack-and-dependencies)

## ActiveJob

[see docs](https://guides.rubyonrails.org/active_job_basics.html)

Active Job is a framework for declaring jobs and making them run on a variety of queuing backends. These jobs can be everything from regularly scheduled clean-ups, to billing charges, to mailings. Anything that can be chopped up into small units of work and run in parallel, really.

[Top](#stack-and-dependencies)

## Sidekiq

[sidekiq.org](https://sidekiq.org)

An open-source job scheduler written in ruby.

[Top](#stack-and-dependencies)

## Redis

[redis.io](https://redis.io/)

An in-memory data structure store, used as a distributed, in-memory-key-value database, cache, and message broker.

[Top](#stack-and-dependencies)

## Whenever

[repo](https://github.com/javan/whenever)

Whenever is a Ruby gem that provides a clear syntax for writing and deploying cron jobs.

[Top](#stack-and-dependencies)

## Minitest

[Minitest Docs](https://docs.seattlerb.org/minitest/)

A complete suite of testing facilities supporting TDD, BDD, mocking, and benchmarking. 

[Top](#stack-and-dependencies)

## Capybara

[teamcapybara.github.io/capybara](https://teamcapybara.github.io/capybara/)

System tests run via Capybara and a headless web browser, essentially loading a webpage in
the background and ensuring text or CSS selectors or XPATH expressions exist on the page.

Capybara is only used in the test environment.

[Top](#stack-and-dependencies)

## Exception Notification

[Exception Noticiation Repo](https://github.com/smartinez87/exception_notification)

The Exception Notification gem provides a set of notifiers for sending notifications when errors occur in a Rack/Rails application. The built-in notifiers can deliver notifications by email, HipChat, Slack, Mattermost, Teams, IRC, Amazon SNS, Google Chat, Datadog or via custom WebHooks.

The Gem in Geodiscovery is currently configured to e-mail digilib@uwm.edu

[Top](#stack-and-dependencies)

## Dependabot

[Dependabot docs](https://docs.github.com/en/code-security/dependabot/dependabot-security-updates/configuring-dependabot-security-updates)

Dependabot will analyze and alert the repository administrators should any code dependencies experience a
security vulnerability. It will also draft pull requests for simple security fixes in the application.

Pull requests drafted by Dependabot should be carefully tested before incorporating into the application. Certain packages (e.g. Bootstrap) can be ignored via the [configuration file](https://github.com/UWM-Libraries/GeoDiscovery/blob/main/.github/dependabot.yml).

[Top](#stack-and-dependencies)
