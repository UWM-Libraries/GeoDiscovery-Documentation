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
gem "bot_challenge_page", "~> 0.4.0"
gem "rack-attack", "~> 6.7"  # present but unused
```

## ApplicationController Integration

The Turnstile challenge is enforced in `ApplicationController` via a `before_action`:

```ruby
# app/controllers/application_controller.rb
before_action do |controller|
  BotChallengePage::BotChallengePageController.bot_challenge_enforce_filter(controller, immediate: true)
end
```

This filter checks the session and the exempt logic to determine whether a Turnstile challenge is required.

## Configuration

All challenge behavior is configured in:

[`config/initializers/bot_challenge_page.rb`](https://github.com/UWM-Libraries/GeoDiscovery/blob/main/config/initializers/bot_challenge_page.rb)

Key settings include:

- `config.enabled` — Uses `Settings.turnstile.enabled` (disabled in test env)
- `config.cf_turnstile_sitekey` and `..._secret_key` — Stored in `Settings.turnstile`
- `config.redirect_for_challenge = false` — Render challenge inline
- `config.rate_limited_locations = []` — No rate-limiting paths configured

### Session Exemptions

We track exemptions using session state. If a user passes a Turnstile challenge, they are considered exempt for the rest of the session.

Additional exemption logic is defined in the `allow_exempt` lambda:

```ruby
config.allow_exempt = lambda do |controller, _|
  exempt =
    (controller.is_a?(CatalogController) &&
     controller.params[:action].in?(%w[facet]) &&
     controller.request.headers["sec-fetch-dest"] == "empty") ||
    ip_safelist.map { |cidr| IPAddr.new(cidr) }.any? { |range| range.include?(controller.request.remote_ip) }
  Rails.logger.warn "[Turnstile‑EXEMPT] IP: #{controller.request.remote_ip}, Exempt: #{exempt}"
  exempt
end
```

This ensures:
- Search interface components (like facets) are not interrupted
- Known IP ranges (set via `TURNSTILE_IP_SAFELIST`) are exempt

## Testing and Local Development

See [Cloudflare’s testing guide](https://developers.cloudflare.com/turnstile/troubleshooting/testing/) for keys that always pass, challenge, or fail.

In `development` and `test`, the feature is typically disabled:

```ruby
config.enabled = !Rails.env.test? && Settings.turnstile.enabled
```

## Related Links

- [bot_challenge_page GitHub](https://github.com/samvera-labs/bot_challenge_page)
- [Cloudflare Turnstile Docs](https://developers.cloudflare.com/turnstile/)
- [GeoDiscovery GitHub](https://github.com/UWM-Libraries/GeoDiscovery)

---
