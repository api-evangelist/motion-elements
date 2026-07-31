---
name: Search and retrieve a MotionElements asset
description: Search the MotionElements marketplace for a stock asset and retrieve its licensing details by id.
api: openapi/motion-elements-openapi.yml
operations: [searchVideos, searchMusic, getElement, getAccount]
---

# Search and retrieve a MotionElements asset

Use the MotionElements Marketplace API v2 to find a royalty-free asset and read
its price/license before downloading.

## Auth
- HTTP Basic. Send your API **secret key** (from the Developer Console) as the
  Basic-auth **username**; leave the password empty. HTTPS only.

## Steps
1. **Confirm the account** — call `getAccount` (`GET /v2/account`) to verify the
   key works and check subscription/credit status and `currency`.
2. **Search** — call the media-specific search operation for what you need, e.g.
   `searchVideos` (`GET /v2/search/video?q=waterfall`) or `searchMusic`
   (`GET /v2/search/music?q=cinematic`). Filter with `category`, `artist`,
   `price_range`, `license`, `subscription`; sort with `sort` (e.g.
   `sort=-created_at`); page with `page` / `per_page` (max 100).
3. **Read the envelope** — the response is a list: `{object:"list", page,
   per_page, total_count, data:[...]}`. Each `data[]` item is an `element` with
   `id`, `name`, `price`, `currency`, `credits`, `subscription`, `usage_rights`.
4. **Fetch one asset** — call `getElement` (`GET /v2/elements/{id}`) with the
   `id` from search to get the full element detail and licensing terms.

## Conventions
- Localize with `language` (en, ja, ko, zh-hant) and price with `currency`
  (USD, EUR, JPY, KRW, TWD).
- No idempotency key is needed — searches and reads are safe to retry.
- Errors: `401` = bad/missing secret key; `404` = unknown element id.
