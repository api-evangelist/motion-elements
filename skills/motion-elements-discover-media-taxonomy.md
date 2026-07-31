---
name: Discover MotionElements media taxonomy
description: Enumerate MotionElements media types and their categories, software versions, and music facets to build valid search filters.
api: openapi/motion-elements-openapi.yml
operations: [listMediaTypes, listMediaTypeCategories, listMediaTypeSoftwareVersions, listMediaTypeInstruments, listMediaTypeMusicalKeys]
---

# Discover MotionElements media taxonomy

Before filtering searches, enumerate the reference taxonomy so filter values are
valid.

## Auth
- HTTP Basic; API secret key as the username, empty password. HTTPS only.

## Steps
1. **List media types** — call `listMediaTypes` (`GET /v2/media-types`) to get
   every media type (`id`, `code`, `group`, `sub_media_types`, `video`/`audio`
   flags).
2. **List categories** — call `listMediaTypeCategories`
   (`GET /v2/media-types/{mediaType}/categories`) with a `mediaType` code (e.g.
   `video`); optionally filter with `type` (e.g. `style`). Use returned category
   `id`s as the `category` filter on search.
3. **Software versions** (templates) — call `listMediaTypeSoftwareVersions`
   (`GET /v2/media-types/{mediaType}/software-versions`) to map editing apps
   (After Effects, Premiere Pro, DaVinci Resolve, Apple Motion) and versions.
4. **Music facets** — for audio, call `listMediaTypeInstruments` and
   `listMediaTypeMusicalKeys` (and `listMediaTypeEditTypes`) to build
   music-specific filters.

## Conventions
- Localize labels with `language`; all endpoints are `GET` and idempotent.
- Reference lists are stable enough to cache between searches.
