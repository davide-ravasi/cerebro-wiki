# Node.js / Express — raw notes

Appunti brevi mentre impari Node/Express in contesto reale (API serverless, proxy, sicurezza).

## Indice

- [`rate-limiting-and-client-ip.md`](./rate-limiting-and-client-ip.md) — rate limit + IP + `trust proxy` (dettaglio).
- [`express-netlify-security-notes.md`](./express-netlify-security-notes.md) — body limit (JSON + form), checklist sicurezza Express/Netlify, error middleware.

## Naming

- `topic-slug-kebab-case.md` (es. `rate-limiting-and-client-ip.md`).
- Prefisso numerico opzionale se vuoi ordinare una sequenza: `01-express-middleware.md`.

## Provenienza

Alcuni file sono copia o evoluzione di note nel repo applicativo **track-em-all** (`docs/` lì). Se duplichi, indica in cima al file dove sta la versione “canonica” nel repo app.

## Promote workflow (opzionale)

1. Rivedi la nota in `raw/`.
2. Se diventa materiale generico, estrai concetti in `docs/concepts/` o pattern in `docs/patterns/`.
3. Collega dal `llm-wiki` o dalla mappa tematica se crei una voce stabile.
