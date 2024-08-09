---
title: GBL Sidecar Images
layout: default
nav_order: 11
---

# GeoBlacklight Sidecar Images

## Harvest Thumbnails - GeoBlacklight Sidecar Images

This project uses the 
[GeoBlacklight Sidecar Images](https://github.com/geoblacklight/geoblacklight_sidecar_images)
gem to collect search result thumbnails.

Ensure Sidekiq and Redis are working as expected by running Sidekiq Monitor:

```ruby
bundle exec sidekiqmon

#output:
Sidekiq 7.3.0
2024-08-09 20:05:07 UTC

---- Overview ----
  Processed: 80
     Failed: 1
       Busy: 0
   Enqueued: 0
    Retries: 0
  Scheduled: 0
       Dead: 0

---- Processes (0) ----

---- Queues (1) ----
NAME       SIZE  LATENCY
default       0     0.00
```

That gem includes several helpful rake tasks to manage thumbnail images:

```ruby
bundle exec rake gblsci:images:harvest_all
bundle exec rake gblsci:images:harvest_states
bundle exec rake gblsci:images:harvest_failed_state_inspect
bundle exec rake gblsci:images:harvest_report
bundle exec rake gblsci:images:harvest_retry
bundle exec rake gblsci:images:harvest_purge_all
bundle exec rake gblsci:images:harvest_purge_orphans
DOC_ID='stanford-cz128vq0535' bundle exec rake gblsci:images:harvest_doc_id
```

Read more about each rake task on the gem
[README](https://github.com/geoblacklight/geoblacklight_sidecar_images)
file.
