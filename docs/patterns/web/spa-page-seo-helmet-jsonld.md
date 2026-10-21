---
id: pattern-spa-page-seo-helmet-jsonld
title: "SPA page SEO: Helmet layers + JSON-LD"
type: pattern
domain: web
tags: [pattern, decision, web, seo, react, helmet, json-ld, track-em-all]
status: draft
confidence: medium
updated: 2026-10-19
---

# Context

A React SPA (Track'em All) has entity routes (`/show/:id`, `/episode/:id`, `/person/:id`) rendered client-side. You need share/search metadata without a separate SSR stack.

# Problem

- Meta tags in static `index.html` are global → every route looks the same to scrapers that execute little or no JS, and even with Helmet you must keep **four layers** consistent.
- Using a non-unique path field (e.g. `episode_number`) as canonical creates collisions and mismatches the real URL.
- Structured data without a load guard emits `null` graphs or empty `<script>` tags.

# Decision

Per entity page, emit via `react-helmet-async`:

1. `<title>` + `meta name="description"`
2. `<link rel="canonical">` + `og:url` = `{VITE_WEBSITE_BASE_URL}{routerPath}`
3. Open Graph + Twitter cards (shared `ogImage` with fallback)
4. Optional JSON-LD (`TVSeries` / `TVEpisode` / `Person`) only when entity data is loaded; `url` = canonical

Canonical path id comes from **`useParams`**, not from display fields in `location.state`.

# Why

- One composition of signals: label → identity → share card → typed entity.
- Reuses data already fetched for the UI.
- Matches what the app actually routes.

# Tradeoffs

- Benefit: incremental SEO without SSR; easy DevTools checks.
- Cost: scrapers that do not run JS still see only `index.html` defaults; full SEO may later need prerender/SSR.
- Cost: must keep base URL env correct (local vs production).

# Alternatives Considered

- SSR / prerender for meta only — stronger for non-JS crawlers; heavier ops.
- Sitemap-only without per-page Helmet — insufficient for share cards and entity typing.
- JSON-LD without fixing canonical — structured data points at the wrong identity.

# Signals To Revisit

- Major share platforms still show wrong cards after env/base URL change.
- Moving to nested URLs (`/show/:showId/season/…`) → update routes, links, canonical, and JSON-LD together.
- Need guaranteed rich results → verify types against Google docs and consider SSR.

# Related

- [[concept-canonical-url]]
- [[concept-json-ld]]
- [[concept-open-graph-and-twitter-cards]]
- Raw: `sources/react/raw/track-em-all-seo-helmet-jsonld.md`
