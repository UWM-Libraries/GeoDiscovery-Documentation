---
title: Release Notes
layout: default
nav_order: 1.3
---

# Release Notes

## v4.5.6 - 2026-03-23

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
