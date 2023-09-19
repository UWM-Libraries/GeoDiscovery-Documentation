---
title: Rake Tasks
layout: default
nav_order: 4
---

# rake tasks

```ruby
rake about                   # List versions of all Rails frameworks and the environment
rake action_mailbox:ingress:exim                 # Relay an inbound email from Exim to Action Mailbox (URL and INGRESS_PASSWORD required)
rake action_mailbox:ingress:postfix              # Relay an inbound email from Postfix to Action Mailbox (URL and INGRESS_PASSWORD required)
rake action_mailbox:ingress:qmail                # Relay an inbound email from Qmail to Action Mailbox (URL and INGRESS_PASSWORD required)
rake action_mailbox:install  # Installs Action Mailbox and its dependencies
rake action_mailbox:install:migrations           # Copy migrations from action_mailbox to application
rake action_text:install     # Copy over the migration, stylesheet, and JavaScript files
rake action_text:install:migrations              # Copy migrations from action_text to application
rake active_storage:install  # Copy over the migration needed to the application
rake app:template            # Applies the template supplied by LOCATION=(/path/to/template) or URL
rake app:update              # Update configs and some other initially generated files (or use just update:configs or update:bin)
rake assets:clean[keep]      # Remove old compiled assets
rake assets:clobber          # Remove compiled assets
rake assets:environment      # Load asset compile environment
rake assets:precompile       # Compile all the assets named in config.assets.precompile
rake autoprefixer:info       # Show selected browsers and prefixed CSS properties and values
rake blacklight:check:solr[controller_name]      # Check the Solr connection and controller configuration
rake blacklight:delete_old_searches[days_old]    # Removes entries in the searches table that are older than the number of days given
rake blacklight:index:seed   # Index sample data (from FILE, ./spec/fixtures/sample_solr_documents.yml in this application, or the test fixtures from blacklight) into solr
rake blacklight:install:migrations               # Copy migrations from blacklight to application
rake cache_digests:dependencies                  # Lookup first-level dependencies for TEMPLATE (like messages/show or comments/_comment.html)
rake cache_digests:nested_dependencies           # Lookup nested dependencies for TEMPLATE (like messages/show or comments/_comment.html)
rake ci  # Run test suite
rake db:create               # Creates the database from DATABASE_URL or config/database.yml for the current RAILS_ENV (use db:create:all to create all databases in the config). Without RAILS_ENV or when RAILS_ENV is development, it defaults to creating the development and test databases, except when DATABASE_URL is present
rake db:drop                 # Drops the database from DATABASE_URL or config/database.yml for the current RAILS_ENV (use db:drop:all to drop all databases in the config). Without RAILS_ENV or when RAILS_ENV is development, it defaults to dropping the development and test databases, except when DATABASE_URL is present
rake db:encryption:init      # Generate a set of keys for configuring Active Record encryption in a given environment
rake db:environment:set      # Set the environment value for the database
rake db:fixtures:load        # Loads fixtures into the current environment's database
rake db:migrate              # Migrate the database (options: VERSION=x, VERBOSE=false, SCOPE=blog)
rake db:migrate:down         # Runs the "down" for a given migration VERSION
rake db:migrate:redo         # Rolls back the database one migration and re-migrates up (options: STEP=x, VERSION=x)
rake db:migrate:status       # Display status of migrations
rake db:migrate:up           # Runs the "up" for a given migration VERSION
rake db:prepare              # Runs setup if database does not exist, or runs migrations if it does
rake db:reset                # Drops and recreates all databases from their schema for the current environment and loads the seeds
rake db:rollback             # Rolls the schema back to the previous version (specify steps w/ STEP=n)
rake db:schema:cache:clear   # Clears a db/schema_cache.yml file
rake db:schema:cache:dump    # Creates a db/schema_cache.yml file
rake db:schema:dump          # Creates a database schema file (either db/schema.rb or db/structure.sql, depending on `ENV['SCHEMA_FORMAT']` or `config.active_record.schema_format`)
rake db:schema:load          # Loads a database schema file (either db/schema.rb or db/structure.sql, depending on `ENV['SCHEMA_FORMAT']` or `config.active_record.schema_format`) into the database
rake db:seed                 # Loads the seed data from db/seeds.rb
rake db:seed:replant         # Truncates tables of each database for current environment and loads the seeds
rake db:setup                # Creates all databases, loads all schemas, and initializes with the seed data (use db:reset to also drop all databases first)
rake db:version              # Retrieves the current schema version number
rake devise_guests:delete_old_guest_users[days_old]                  # Removes entries in the users table for guest users that are older than the number of days given
rake gblsci:images:harvest_all                   # Harvest all images
rake gblsci:images:harvest_destroy_batch         # Destroy select sidecar AR objects by CSV file
rake gblsci:images:harvest_doc_id                # Harvest image for specific document
rake gblsci:images:harvest_failed_state_inspect  # Inspect failed state objects
rake gblsci:images:harvest_purge_all             # Destroy all harvested images and sidecar AR objects
rake gblsci:images:harvest_purge_orphans         # Destroy orphaned images and sidecar AR objects
rake gblsci:images:harvest_report                # Write harvest state report (CSV)
rake gblsci:images:harvest_retry                 # Re-queues incomplete states for harvesting
rake gblsci:images:harvest_states                # Hash of SolrDocumentSidecar image state counts
rake gblsci:sample_data:seed # Ingests a directory of geoblacklight.json files
rake geoblacklight:application_asset_paths       # Stdout output asset paths
rake geoblacklight:downloads:delete              # Delete all cached downloads
rake geoblacklight:downloads:mkdir               # Create download directory
rake geoblacklight:downloads:precache[doc_id,download_type,timeout]  # Precaches a download
rake geoblacklight:index:ingest[directory]       # Ingests a directory of geoblacklight.json files
rake geoblacklight:index:ingest_all              # Ingests a GeoHydra transformed.json
rake geoblacklight:index:seed[remote]            # Index GBL test fixture metadata into Solr
rake geoblacklight:server[rails_server_args]     # Run Solr and GeoBlacklight for interactive development
rake geoblacklight:solr:seed # Put sample data into solr
rake geoblacklight:webpack   # Run Solr and GeoBlacklight for interactive development with Webpack enabled
rake geoblacklight_sidecar_images:install:migrations                 # Copy migrations from geoblacklight_sidecar_images to application
rake geocombine:clone[repo]  # Clone OpenGeoMetadata repositories
rake geocombine:geoblacklight_harvester:index[site]                  # Harvest documents from a configured GeoBlacklight instance
rake geocombine:index        # Index all JSON documents except Layers.json
rake geocombine:pull[repo]   # "git pull" OpenGeoMetadata repositories
rake importmap:install       # Setup Importmap for the app
rake log:clear               # Truncates all/specified *.log files in log/ to zero bytes (specify which logs with LOGS=test,development)
rake middleware              # Prints out your Rack middleware stack
rake restart                 # Restart app by touching tmp/restart.txt
rake secret                  # Generate a cryptographically secure secret key (this is typically used to generate a secret for cookie sessions)
rake sitemap:clean           # Delete all Sitemap files in public/ directory
rake sitemap:create          # Generate sitemaps but don't ping search engines
rake sitemap:install         # Install a default config/sitemap.rb file
rake sitemap:refresh         # Generate sitemaps and ping search engines
rake sitemap:refresh:no_ping # Generate sitemaps but don't ping search engines
rake standard                # Lint with the Standard Ruby style guide
rake standard:fix            # Lint and automatically make safe fixes with the Standard Ruby style guide
rake standard:fix_unsafely   # Lint and automatically make fixes (even unsafe ones
rake stats                   # Report code statistics (KLOCs, etc) from the application or engine
rake stimulus:install        # Install Stimulus into the app
rake stimulus:install:importmap                  # Install Stimulus on an app running importmap-rails
rake stimulus:install:node   # Install Stimulus on an app running node
rake test                    # Runs all tests in test folder except system ones
rake test:all                # Runs all tests, including system tests
rake test:db                 # Run tests quickly, but also reset db
rake test:system             # Run system tests only
rake time:zones[country_or_offset]               # List all time zones, list by two-letter country code (`bin/rails time:zones[US]`), or list by UTC offset (`bin/rails time:zones[-8]`)
rake tmp:clear               # Clear cache, socket and screenshot files from tmp/ (narrow w/ tmp:cache:clear, tmp:sockets:clear, tmp:screenshots:clear)
rake tmp:create              # Creates tmp directories for cache, sockets, and pids
rake turbo:install           # Install Turbo into the app
rake turbo:install:importmap # Install Turbo into the app with asset pipeline
rake turbo:install:node      # Install Turbo into the app with webpacker
rake turbo:install:redis     # Switch on Redis and use it in development
rake uwm:development         # Start solr server for development
rake uwm:index:delete_all    # Delete all sample data from solr
rake uwm:index:seed          # Put all sample data into solr
rake uwm:index:uwm           # Put uwm sample data into solr
rake uwm:server[rails_server_args]               # Run Solr and GeoBlacklight for interactive development
rake uwm:test                # Start solr server for testing
rake yarn:install            # Install all JavaScript dependencies as specified via Yarn
rake zeitwerk:check          # Checks project structure for Zeitwerk compatibility

```

