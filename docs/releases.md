---
title: Release Notes
layout: default
nav_order: 1.3
---

# Release Notes

## v4.5.6 - 2026-03-24

GeoDiscovery `v4.5.6` is a maintenance, UX, and operations release focused on polish and hardening.

### Highlights

- Split-view search results again display sidecar thumbnails.
- Turnstile protection was migrated to `bot_challenge_page` 1.x patterns while preserving facet-request and safelisted-IP exemptions.
- Shared-record emails now include richer metadata and a permalink.
- Accessibility coverage now includes automated `axe-core` system tests.
- Static `404`, `422`, and `500` pages were redesigned with AGSL branding and clearer user guidance.
- Production mailer settings are standardized around `ACTION_MAILER_FROM` and `ACTION_MAILER_HOST`.
- Local setup guidance now explicitly includes `yarn install`, and `bin/setup` installs JavaScript dependencies automatically.

### Dependency updates

- Rails and related gems updated to `7.2.3.1`
- `bot_challenge_page` updated from `0.4.0` to `1.1.0`
- `geoblacklight_sidecar_images` updated from `~> 1.0` to `~> 1.1`
- `vite_rails` pinned to `~> 3.0.20`
- `vite_ruby` added at `~> 3.9.3`

Source: [GitHub release](https://github.com/UWM-Libraries/GeoDiscovery/releases/tag/v4.5.6)

## v4.5.5 - 2026-03-05

GeoDiscovery `v4.5.5` was an accessibility and dependency-maintenance release.

### Highlights

- Improved WCAG support across search and browse views.
- Removed the Leaflet geosearch plugin that introduced unlabeled controls.
- Improved contrast for active facets and pagination.
- Fixed empty icon-only pagination links and improved heading structure.
- Marked decorative thumbnails for accessible presentation.

### Dependency updates

- Rails updated to `7.2.3`
- Rack updated to `3.1.18`
- Rack::Attack updated to `6.8.0`
- Puma updated to `7.1.0`
- Haml updated to `7.0.2`
- Whenever updated to `1.1.0`
- `solr_wrapper` updated to `4.3.0`
- Frontend updates included `vite`, `lodash-es`, and `jquery-rails`

Source: [GitHub release](https://github.com/UWM-Libraries/GeoDiscovery/releases/tag/v4.5.5)

## v4.5.0 - 2025-09-26

GeoDiscovery `v4.5.0` was the GeoBlacklight `4.5` upgrade release and was published as a pre-release.

### Highlights

- Adopted the BTAA-style bot challenge setup.
- Upgraded GeoBlacklight to `4.5`.

Source: [GitHub release](https://github.com/UWM-Libraries/GeoDiscovery/releases/tag/v4.5.0)

## v4.4.11 - 2025-07-11

GeoDiscovery `v4.4.11` focused on stabilizing background jobs and softening the initial Turnstile rollout.

### Highlights

- Rolled Sidekiq back from `~> 8.0` to `~> 7.3` to resolve deployment issues.
- Disabled rate-limited bot challenge settings.
- Removed Rack::Attack initialization from the active bot-defense flow.
- Kept IP-based and request-pattern exemptions in place for Turnstile.

### Dependency updates

- Sidekiq and logger updates
- `debug` updated to `1.11.0`
- `bootsnap` updated to `1.18.6`

Source: [GitHub release](https://github.com/UWM-Libraries/GeoDiscovery/releases/tag/v4.4.11)

## v4.4.10 - 2025-06-16

GeoDiscovery `v4.4.10` introduced Turnstile bot protection with `bot_challenge_page`.

### Highlights

- Added `bot_challenge_page` and `rack-attack`.
- Configured the challenge flow in `ApplicationController`.
- Added an initializer for Turnstile keys, rate limiting, and exemptions.
- Exempted facet JavaScript requests and safelisted IPs.
- Updated deployment configuration to symlink `.env.production` from `shared/`.

Source: [GitHub release](https://github.com/UWM-Libraries/GeoDiscovery/releases/tag/v4.4.10)

## v4.4.8 - 2024-12-30

GeoDiscovery `v4.4.8` combined dependency refreshes with record-display refinements.

### Highlights

- Reordered show-page fields in `catalog_controller.rb`.
- Switched DESCRIPTION rendering to `truncate_render_html_value`.
- Renamed the REFERENCES label from "Resource Link" to "More details at".
- Added conditional display logic for REFERENCES based on `external_url`.

### Dependency updates

- Updated gems including `bigdecimal`, `csv`, `debug`, `erubi`, `importmap-rails`, `net-imap`, `nokogiri`, `rack-test`, `regexp_parser`, `tilt`, and `view_component`

Source: [GitHub release](https://github.com/UWM-Libraries/GeoDiscovery/releases/tag/v4.4.8)

## v4.4.5 - 2024-09-16

GeoDiscovery `v4.4.5` focused on operations and runtime modernization.

### Highlights

- Added rate limiting for exception emails.
- Added `sidekiq.yml` for clearer job-processing control.
- Switched Sidekiq logging to the Rails logger and tuned production log levels.
- Discarded `NoMethodError` and `DeserializationError` jobs instead of retrying them.
- Stopped tracking SQLite files in Git.

### Dependency updates

- `turbo-rails` updated from `2.0.5` to `2.0.6`
- `sprockets-rails` updated from `3.5.1` to `3.5.2`
- Rails updated from `7.0.8.4` to `7.2.1`

Source: [GitHub release](https://github.com/UWM-Libraries/GeoDiscovery/releases/tag/v4.4.5)

## v4.4.3 - 2024-08-05

GeoDiscovery `v4.4.3` is documented as a debugging release for the `gbl1` to Aardvark script.

Source: [GitHub release](https://github.com/UWM-Libraries/GeoDiscovery/releases/tag/v4.4.3)

## v4.4.1 - 2024-06-13

GeoDiscovery `v4.4.1` brought production up to GeoBlacklight `4.4`.

### Highlights

- Added support for the `Blacklight::Allmaps` plugin.
- Added support for Cloud Optimized GeoTIFF and PMTiles.
- Updated homepage buttons and social media icons.
- Reordered facets in search results.

Source: [GitHub release](https://github.com/UWM-Libraries/GeoDiscovery/releases/tag/v4.4.1)
