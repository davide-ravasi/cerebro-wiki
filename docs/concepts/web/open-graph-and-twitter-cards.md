---
id: concept-open-graph-and-twitter-cards
title: "Open Graph and Twitter Cards"
type: concept
domain: web
sources:
  - note: raw-react-track-em-all-seo-helmet-jsonld
tags: [concept, web, seo, open-graph, twitter, social, meta]
status: evergreen
confidence: medium
updated: 2026-10-19
---

# Definition

**Open Graph** (`og:*` meta tags) and **Twitter Cards** (`twitter:*` meta tags) are HTML metadata conventions that control how a URL looks when shared on social platforms (title, description, image, url).

# Why It Matters

- Search snippet tags (`title`, `meta description`) are not the same contract as share previews.
- A missing or tiny `og:image` produces weak cards (common audit failure).
- `og:url` should match the **canonical** URL so scrapers and search agree.

# How It Works

Typical set for an entity page:

| Tag | Role |
|-----|------|
| `og:title` / `twitter:title` | Headline in the card |
| `og:description` / `twitter:description` | Short blurb (often ≤160 chars like meta description) |
| `og:image` / `twitter:image` | Absolute image URL (prefer ~1200×630) |
| `og:url` | Same as canonical |
| `og:type` | Often `website` for SPA pages |
| `twitter:card` | e.g. `summary_large_image` |

Emitted in `<head>` via Helmet on SPAs. Platforms cache aggressively — after deploy, re-scrape tools (e.g. opengraph.to, platform debuggers).

# Tradeoffs

- Pros: concrete share UX; easy to audit.
- Cons: platform-specific quirks; cache staleness; image size/aspect matter more than perfect copy.

# When To Use

- Use on public pages users share (home, show, episode, person).
- Align image fallbacks with JSON-LD `image` when both exist.

# Related Concepts

- [[concept-canonical-url]]
- [[concept-json-ld]]

# Related Patterns

- [[pattern-spa-page-seo-helmet-jsonld]]
