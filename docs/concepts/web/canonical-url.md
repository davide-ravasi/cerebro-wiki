---
id: concept-canonical-url
title: "Canonical URL"
type: concept
domain: web
sources:
  - note: raw-react-track-em-all-seo-helmet-jsonld
tags: [concept, web, seo, html, url, helmet]
status: evergreen
confidence: medium
updated: 2026-10-19
---

# Definition

A **canonical URL** is the single preferred public address of a page, declared with `<link rel="canonical" href="…">` (and usually mirrored in `og:url`). It tells search engines which URL to treat as the primary identity when the same content could appear under more than one address.

# Why It Matters

- Avoids **duplicate-content** signals when query params, trailing slashes, or alternate paths exist.
- Must match the **real** route users (and crawlers) land on; a wrong canonical invents a false identity.
- Social previews (`og:url`) should use the same value so share scrapers and search agree on one URL.

# How It Works

1. Build the absolute URL: site base (no trailing slash) + path that matches the router.
2. Emit `<link rel="canonical" href="{absoluteUrl}">` in `<head>` (e.g. via `react-helmet-async`).
3. Prefer the same string for `og:url`.

The path segment must be the **stable unique id** of the resource (e.g. TMDB episode id in `/episode/:id`), not a display field that collides across entities (e.g. `episode_number` = 1 on every show).

# Tradeoffs

- Pros: clear identity for crawlers; cheap to emit on SPAs with Helmet.
- Cons: wrong value is worse than none; SPA still needs crawlable/shareable URLs (client-only state does not fix SEO by itself).

# When To Use

- Use when pages are indexable or shared (show, episode, person, listing).
- Avoid inventing paths that the app does not actually serve.

# Example

Track'em All episode route is `/episode/:id` (TMDB id). Canonical must be `{base}/episode/{id}` from `useParams`, not `{base}/episode/{episode_number}`.

# Related Concepts

- [[concept-json-ld]]
- [[concept-open-graph-and-twitter-cards]]

# Related Patterns

- [[pattern-spa-page-seo-helmet-jsonld]]
