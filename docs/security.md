---
title: Security
layout: default
nav_order: 12
---

# Security

## Combating web crawling bots

GeoDiscovery uses [`bot_challenge_page`](https://github.com/samvera-labs/bot_challenge_page/) with Cloudflare Turnstile to reduce abusive crawling on search and index pages while keeping normal discovery workflows usable.

The current implementation follows the gem's 1.x controller and initializer patterns instead of the older inline enforcement style.

### Exemption model

Challenge exemptions are intentionally narrow:

- Facet-fetch requests can skip the challenge so the search interface remains responsive.
- IP safelists are configured as CIDR ranges and parsed in the initializer.
- Session state still prevents users from repeatedly seeing the challenge after a successful pass.

For implementation details, see [Bot Challenge Page](utils/bot_challenge_page).

