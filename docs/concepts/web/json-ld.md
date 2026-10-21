---
id: concept-json-ld
title: "JSON-LD (schema.org)"
type: concept
domain: web
sources:
  - note: raw-react-track-em-all-seo-helmet-jsonld
tags: [concept, web, seo, structured-data, schema.org, json-ld]
status: evergreen
confidence: medium
updated: 2026-10-19
---

# Definition

**JSON-LD** (*JavaScript Object Notation for Linked Data*) is a way to embed a machine-readable description of a page as a JSON object inside `<script type="application/ld+json">`, usually using the **schema.org** vocabulary (`@context`, `@type`, properties).

# Why It Matters

- Title, description, and Open Graph answer *how the page is labeled/shared*.
- JSON-LD answers *what kind of thing* the page is (TV series, episode, person) and how entities relate.
- Helps crawlers and validators; may enable richer search features — **not guaranteed**.

It does **not** replace HTML content, canonical URLs, or Open Graph. It complements them.

# How It Works

1. Choose a schema.org type (`TVSeries`, `TVEpisode`, `Person`, …).
2. Fill properties you already have in the UI (`name`, `description`, `image`, `url`).
3. Set `url` to the same absolute URL as the **canonical**.
4. `JSON.stringify` the object into a Helmet `<script type="application/ld+json">`.
5. Mount the script only when data is loaded (avoid `null` fields / empty scripts).

`@context: "https://schema.org"` means property names follow that vocabulary. Linked Data means types and relations are shared identifiers, not ad-hoc keys.

## Finding `@type` values

1. Search [schema.org](https://schema.org) for the entity (e.g. `TVSeries`, `TVEpisode`, `Person`).
2. Open the type page: definition, properties, more general / more specific types.
3. Optionally cross-check [Google’s structured data gallery](https://developers.google.com/search/docs/appearance/structured-data/search-gallery) if you care about Search features — not every schema.org type is documented there.

Do not invent type names; prefer the most specific *accurate* type (a series page → `TVSeries`, not a vague custom label).

# Tradeoffs

- Pros: explicit typing; cheap once page data exists; inspectable in DevTools / Rich Results Test.
- Cons: invalid or stale markup misleads tools; empty/null graphs while loading are noise; rich results are optional.

# When To Use

- Use on entity pages (show, episode, person) once canonical/OG exist.
- Prefer well-known types (`TVSeries` for a series page, not a vague custom label).
- Avoid inventing types or claiming rich-result eligibility as a ranking hack.

# Example

Show page → `TVSeries` with `name`, `description`, `image`, `url` = canonical. Render the script only when `showData` is present.

# Related Concepts

- [[concept-canonical-url]]
- [[concept-open-graph-and-twitter-cards]]

# Related Patterns

- [[pattern-spa-page-seo-helmet-jsonld]]
