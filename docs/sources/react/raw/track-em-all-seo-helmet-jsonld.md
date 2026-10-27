---
title: Track'em All — SEO a layer (Helmet, canonical, OG, JSON-LD)
type: raw-note
tags: [react, seo, helmet, canonical, open-graph, json-ld, schema.org, track-em-all]
provenance: Sessioni track-em-all 2026-09 → 2026-10-26 (Helmet · canonical Episode · JSON-LD Show/Episode/Person)
see_also_app: track-em-all ShowPage / EpisodePage / PersonPage · App.tsx routes · EpisodeCard Link
promote_later: docs/concepts/web/canonical-url.md · json-ld.md · open-graph-and-twitter-cards.md · patterns/web/spa-page-seo-helmet-jsonld.md
---

# Track'em All — SEO a layer

**Idea centrale:** sulla stessa pagina entity hai **quattro layer** che rispondono a domande diverse. Non si sostituiscono.

| Layer | Domanda | Meccanismo |
|-------|---------|------------|
| Title / description | Come mi presento in tab / snippet | `<title>`, `meta name="description"` |
| Canonical (+ `og:url`) | Qual è l’URL ufficiale | `<link rel="canonical">` |
| Open Graph / Twitter | Come appaio nello share | `og:*`, `twitter:*` |
| JSON-LD | *Che tipo di cosa* sono | `<script type="application/ld+json">` + schema.org |

Tutto via **`react-helmet-async`** (`Helmet` nel page component).

---

## Canonical Episode — bug e fix (19/10)

- Route reale: `/episode/:id` (`App.tsx`); `EpisodeCard` linka `/episode/${id}` (id TMDB).
- Bug: canonical usava `episode_number` → collisioni (Pilot di show diversi → `/episode/1`) e **canonical ≠ URL**.
- Fix: `const { id } = useParams<{ id: string }>()` → `canonicalUrl = \`${websiteBase}/episode/${id}\``; stesso valore in `og:url`.
- `location.state` resta per season/overview/showId (dati UI), **non** per l’identità URL.

`websiteBase` = `VITE_WEBSITE_BASE_URL` senza slash finale (fallback prod Netlify).

---

## JSON-LD Show (19/10)

- Tipo: **`TVSeries`** (preferito a `TVShow` per una serie).
- Campi minimi: `name`, `description` (overview), `image` (= `ogImage` con fallback), `url` (= canonical).
- Montare lo script **solo se** `showData` c’è:

```tsx
{showData && (
  <script type='application/ld+json'>
    {JSON.stringify({
      '@context': 'https://schema.org',
      '@type': 'TVSeries',
      name: showData.name,
      description: showData.overview,
      image: ogImage,
      url: canonicalUrl,
    })}
  </script>
)}
```

Evita script vuoti / `null` in loading (`JSON.stringify(undefined)` non è una stringa utile).

---

## Come trovare i `@type` (schema.org)

1. Cerca su [schema.org](https://schema.org) (es. `TVSeries`, `TVEpisode`, `Person`).
2. Leggi la pagina del tipo: proprietà ammesse + tipi più generali/specifici.
3. Se ti interessano i rich result Google → [Search Gallery](https://developers.google.com/search/docs/appearance/structured-data/search-gallery) (non ogni tipo schema.org è documentato lì).
4. Non inventare nomi di tipo; per una serie usa **`TVSeries`**.

## Cosa JSON-LD *non* è

- Non è un’API da chiamare.
- Non sostituisce canonical/OG.
- Non garantisce rich results Google — aumenta chiarezza macchina.

---

## Stato (26/10)

| Pagina | Helmet meta | Canonical corretto | JSON-LD |
|--------|-------------|--------------------|---------|
| Show | ✓ | ✓ `/show/:id` | ✓ `TVSeries` |
| Episode | ✓ | ✓ `/episode/:id` (useParams) | ✓ `TVEpisode` (+ season/series) |
| Person | ✓ | ✓ `/person/:id` | ✓ `Person` |

Prox track: sitemap (surplus).

---

## Check rapido

1. Apri un’entity → DevTools → Elements → `head`: canonical path = `location.pathname`.
2. `script[type="application/ld+json"]` → `JSON.parse` ok; `@type` e `url` attesi.
3. Share: opengraph.to / debugger piattaforma dopo deploy (cache).

---

## Concept / pattern promossi

- [[concept-canonical-url]]
- [[concept-json-ld]]
- [[concept-open-graph-and-twitter-cards]]
- [[pattern-spa-page-seo-helmet-jsonld]]
- Mappa: [[map-frontend-seo]]
