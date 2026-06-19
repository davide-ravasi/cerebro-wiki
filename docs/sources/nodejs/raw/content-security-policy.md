---
title: Content Security Policy (CSP) — note da Track'em All + Netlify/Vite
type: raw-note
tags: [web, csp, security, netlify, vite, track-em-all]
provenance: Apprendimento con progetto Track'em All (`netlify.toml`, Helmet su Function)
related_concept: docs/concepts/web/content-security-policy.md
see_also_raw: cors-how-it-works.md, express-netlify-security-notes.md, helmet.md
---

# CSP — appunti pratici

Concetto evergreen promosso in **`docs/concepts/web/content-security-policy.md`**. Qui: cosa è successo nel progetto e come leggere gli errori in console.

---

## Idea chiave

La CSP non è “un elenco unico di origin”: sono **direttive per tipo di risorsa** (`script-src`, `connect-src`, …). Il browser chiede: “questo caricamento è ammesso dalla policy del **documento**?”

---

## Dove l’abbiamo messa (Track'em All)

| Layer | File | Ruolo |
|--------|------|--------|
| SPA + static | `netlify.toml` → `[[headers]]` `for = "/*"` | CSP completa + altri header |
| API Function | `functions/express.js` → Helmet | `contentSecurityPolicy: false`; altri header su JSON |

**Perché:** la CSP serve soprattutto all’**HTML** e a cosa la pagina può caricare. Sulle risposte JSON è rumore e rischio di doppia policy.

---

## Direttive usate nel progetto

- **`default-src 'self'`** — fallback restrittivo.
- **`script-src 'self' 'unsafe-inline'`** — necessario con **Vite + `netlify dev`**: React Refresh inietta script inline; senza `'unsafe-inline'` → errore `(index):4 … violates CSP … inline script`.
- **`connect-src`** — TMDB API + **`ws://localhost:5173`** / **`:8888`** per HMR WebSocket.
- **`img-src`** — TMDB + `data:`.
- **`style-src`** — `'unsafe-inline'` + Google Fonts (CSS inline React).
- **`frame-src`** — embed YouTube (`ShowVideo`).
- **`worker-src 'self'`** — service worker PWA.

---

## Dev: tre scenari

| Come avvii | Header CSP da `netlify.toml`? |
|------------|----------------------------------|
| `npm run dev` → `:5173` | Di solito **no** |
| `netlify dev` → `:8888` | **Sì** — serve policy compatibile con Vite |
| Deploy Netlify | **Sì** su static `build/` |

---

## Errore tipico (debug)

```text
Executing inline script violates … 'default-src self' …
script-src was not explicitly set, so default-src is used as a fallback
```

**Causa:** nessun `script-src` + script inline (Vite dev).  
**Fix:** `script-src 'self' 'unsafe-inline'` (tradeoff) oppure dev senza header Netlify / CSP separata prod vs dev.

---

## CSP vs CORS vs Helmet

| Meccanismo | Domanda |
|------------|---------|
| **CSP** | Cosa può **caricare/eseguire** questa pagina? |
| **CORS** | Uno script su origine A può **leggere** la risposta da origine B? |
| **Helmet** | Middleware che imposta header (inclusa CSP opzionale) sulle risposte Express |

---

## Riferimenti repo app

- `react-exp/track-em-all/netlify.toml`
- `react-exp/track-em-all/functions/express.js` (Helmet senza CSP)

---

*Aggiorna la stringa CSP in `netlify.toml` se aggiungi domini (nuova API, analytics, CDN).*
