# Node.js / Express — raw notes

Appunti brevi mentre impari Node/Express in contesto reale (API serverless, proxy, sicurezza).

## Indice

- [`rate-limiting-and-client-ip.md`](./rate-limiting-and-client-ip.md) — rate limit + IP + `trust proxy` (dettaglio).
- [`express-netlify-security-notes.md`](./express-netlify-security-notes.md) — body limit (JSON + form), checklist sicurezza Express/Netlify, error middleware, **CORS allowlist, Helmet, hardening favorites** (§6).
- [`cors-how-it-works.md`](./cors-how-it-works.md) — CORS generico: origine, header, preflight, esempi HTTP, credenziali, checklist, snippet Express.
- [`content-security-policy.md`](./content-security-policy.md) — CSP: direttive, Track'em All / `netlify.toml`, Vite dev; promosso in `concepts/web/content-security-policy.md`.
- [`helmet.md`](./helmet.md) — Helmet.js: moduli header, personalizzazione, esempio Express; collegato a CSP / `express-netlify-security-notes.md` (§6.2).
- [`fetch-two-steps-and-http-errors.md`](./fetch-two-steps-and-http-errors.md) — `fetch` lato browser: perché due passaggi asincroni, `fetch` non rifiuta su 404, `throw` → `catch`, `statusText` vuoto in HTTP/2, `file://` senza origine, `npx`; promosso in `concepts/web/` (3 note).
- [`origin-cors-and-the-five-layers.md`](./origin-cors-and-the-five-layers.md) — origine (schema+host+porta), "incorporare è libero / leggere è vietato", i font come eccezione per licenza, il meccanismo CORS e `vary: Origin`, custom properties + `getComputedStyle`, i **5 strati** (DNS/TCP/HTTP/policy/uso) e i **4 errori di metodo** che ne derivano; promosso in `concepts/web/origin-as-security-boundary.md`.
- [`gitlab-pages-model.md`](./gitlab-pages-model.md) — GitLab Pages: i 4 pezzi del modello (`public/` · `artifacts` · nome job `pages` · ogni run), le 3 funzioni della riga `artifacts`, il guardrail `rules` e il suo lato buio (silenzio, non errore), deploy = snapshot immutabile, `$CI_PAGES_URL` e domini unici, artefatto ≠ deployment, accesso `private` e il 404 deliberato.

## Naming

- `topic-slug-kebab-case.md` (es. `rate-limiting-and-client-ip.md`).
- Prefisso numerico opzionale se vuoi ordinare una sequenza: `01-express-middleware.md`.

## Provenienza

Alcuni file sono copia o evoluzione di note nel repo applicativo **track-em-all** (`docs/` lì). Se duplichi, indica in cima al file dove sta la versione “canonica” nel repo app.

## Promote workflow (opzionale)

1. Rivedi la nota in `raw/`.
2. Se diventa materiale generico, estrai concetti in `docs/concepts/` o pattern in `docs/patterns/`.
3. Collega dal `llm-wiki` o dalla mappa tematica se crei una voce stabile.
