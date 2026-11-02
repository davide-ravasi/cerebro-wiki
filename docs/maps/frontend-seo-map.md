---
id: map-frontend-seo
title: "Frontend SEO Map (Helmet, canonical, OG, JSON-LD)"
type: map
domain: web
tags: [map, navigation, web, seo, react, helmet, json-ld, open-graph, track-em-all]
status: evergreen
updated: 2026-11-02
---

# Purpose

Navigation for **page-level SEO** learned on Track'em All (SPA + `react-helmet-async`): how title/description, canonical, Open Graph/Twitter, and JSON-LD fit together.

# Core Concepts

- [[concept-canonical-url]] — preferred URL; must match the real route id
- [[concept-open-graph-and-twitter-cards]] — share previews (`og:*` / `twitter:*`)
- [[concept-json-ld]] — schema.org typed entity in `<script type="application/ld+json">`

# Patterns

- [[pattern-spa-page-seo-helmet-jsonld]] — four layers per entity page; load-guard JSON-LD; `useParams` for canonical

# Raw / Source Notes

| Topic | File |
|-------|------|
| Track'em All Helmet → JSON-LD (IT) | `sources/react/raw/track-em-all-seo-helmet-jsonld.md` |

# How to find schema.org types

1. Browse / search [schema.org](https://schema.org) (e.g. `TVSeries`, `TVEpisode`, `Person`).
2. Read the type page for allowed properties and parent types.
3. If you care about Google rich results, cross-check [Search Gallery](https://developers.google.com/search/docs/appearance/structured-data/search-gallery).

# Track'em All status (2026-10-26)

| Page | Meta Helmet | Canonical | JSON-LD |
|------|-------------|-----------|---------|
| Show | ✓ | ✓ | ✓ `TVSeries` |
| Episode | ✓ | ✓ (`useParams` id) | ✓ `TVEpisode` (+ season/series) |
| Person | ✓ | ✓ | ✓ `Person` |

Also backlog: ~~sitemap~~ ✓ 02/11. GSC/index rimandato.

# Open Threads

- Sitemap ✓ 02/11 — `public/sitemap.xml` (/, /about, /login, /register) + `Sitemap:` in `robots.txt`. Live ok (Chrome/curl; Opera a volte no). Dynamic entity URL = un giorno.
- **GSC / indicizzazione: no per ora** — migliorare il sito prima; poi Search Console + submit sitemap
- Nested episode URLs (`/show/…/season/…`) would require route + canonical + JSON-LD together
- Non-JS crawlers vs SPA (prerender/SSR if share/index quality stalls)
- Trap SPA: URL senza file statico (es. `/sitemap`) → `_redirects` → app; file vero = `/sitemap.xml`

## JSON-LD — arricchire un giorno (non path attivo)

Path entity **chiuso** (minimi Show/Episode/Person ✓). Solo se serve rich result / chiarezza extra:

| Dove | Add-on |
|------|--------|
| Show | `genre` · `startDate`/`datePublished` · `numberOfSeasons` · `actor`/`creator` (lista corta) |
| Episode | `actor` cast · `sameAs` TMDB |
| Person | `sameAs` (IMDb/TMDB) · `jobTitle` / known-for se in UI |
| Home | `WebSite` / `Organization` (non entity page) |

Non tutto schema.org. Mer 4/11 = ripasso layer + campi minimi, non questi add-on.

# Related Maps

- [[map-web-security]] — Helmet *security* headers (Express) ≠ `react-helmet-async` meta
