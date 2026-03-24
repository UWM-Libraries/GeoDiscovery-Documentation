---
title: Bot Challenge Page
layout: default
nav_order: 4
parent: GeoDiscovery Utilities
---

# Bot Challenge Page

This page documents the implementation of [**bot_challenge_page**](https://github.com/samvera-labs/bot_challenge_page)
in [GeoDiscovery](https://github.com/UWM-Libraries/GeoDiscovery),
including rationale, configuration, and usage details.

## Overview

**bot_challenge_page** provides Turnstile-based bot protection with session awareness. It was chosen to replace `Rack::Attack` due to its greater resilience against evasive bot tactics (e.g., IP rotation).

Our deployment uses [Cloudflare Turnstile](https://www.cloudflare.com/application-services/products/turnstile/) to issue JavaScript-based challenges and track exemption status via session cookies.

## Why We Removed Rack::Attack

The previous setup used [Rack::Attack](https://github.com/rack/rack-attack) to rate-limit suspicious requests. However, this approach proved ineffective against rotating IPs and occasionally blocked legitimate traffic.

Rack::Attack remains in the Gemfile but is not initialized.

## Dependencies

```ruby
# Gemfile
gem "bot_challenge_page", "~> 1.1.0"
gem "rack-attack", "~> 6.7"  # present but unused
```

## Controller Integration

As of GeoDiscovery `v4.5.6`, the app follows the gem's 1.x integration style:

- `ApplicationController` includes the shared bot-challenge behavior
- `CatalogController` applies protection to search and index traffic
- Challenge exemptions are expressed through `skip_when` rules instead of the older inline `before_action` example

## Configuration

All challenge behavior is configured in:

[`config/initializers/bot_challenge_page.rb`](https://github.com/UWM-Libraries/GeoDiscovery/blob/main/config/initializers/bot_challenge_page.rb)

Key settings include:

- `config.enabled` — Uses `Settings.turnstile.enabled` (disabled in test env)
- `config.cf_turnstile_sitekey` and `..._secret_key` — Stored in `Settings.turnstile`
- `config.redirect_for_challenge = false` — Render challenge inline
- `config.skip_when` — Defines requests that should bypass the challenge flow
- IP safelists parsed from `TURNSTILE_IP_SAFELIST` as CIDR ranges

### Session Exemptions

We track exemptions using session state. If a user passes a Turnstile challenge, they are considered exempt for the rest of the session.

Additional exemption logic is now centered on `skip_when` conditions in the initializer. In practice this ensures:

- Search interface components (like facets) are not interrupted
- Known IP ranges (set via `TURNSTILE_IP_SAFELIST`) are exempt

## Testing and Local Development

See [Cloudflare’s testing guide](https://developers.cloudflare.com/turnstile/troubleshooting/testing/) for keys that always pass, challenge, or fail.

In `development` and `test`, the feature is typically disabled:

```ruby
config.enabled = !Rails.env.test? && Settings.turnstile.enabled
```

When documenting or debugging the feature, treat the initializer as the source of truth for:

- `skip_when` exemptions
- Turnstile site and secret keys
- CIDR safelist parsing
- Inline challenge rendering behavior

## Related Links

- [Production Turnstile Widget Dashboard](https://dash.cloudflare.com/708515ae3fa6ac65284fbb4fc4a3d81a/turnstile/widget/0x4AAAAAABg46vO9T6nWu7EG)
- [Uptime Robot status page](https://stats.uptimerobot.com/cb4RfX9aFf)
- [bot_challenge_page GitHub](https://github.com/samvera-labs/bot_challenge_page)
- [Cloudflare Turnstile Docs](https://developers.cloudflare.com/turnstile/)
- [GeoDiscovery GitHub](https://github.com/UWM-Libraries/GeoDiscovery)

---
