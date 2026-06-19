# Node.js / Express — appunti raw

Note informali da progetti pratici (es. **Track'em All**: Express su Netlify Functions, auth, rate limiting).

Vedi cartella **`raw/`** per i file markdown grezzi. Stile e workflow promozione: analoghi a `docs/sources/mongodb-nodejs-developer-path/raw/README.md`.

Mappa wiki promossa: [`web-security-map.md`](../../maps/web-security-map.md) (CORS, CSP, Helmet, rate limit).

## Indice `raw/` (Node / Express / sicurezza)

| File | Contenuto |
|------|-----------|
| [`rate-limiting-and-client-ip.md`](./raw/rate-limiting-and-client-ip.md) | `express-rate-limit`, `getClientIp`, `trust proxy`, header `RateLimit`, esempio numerico |
| [`express-netlify-security-notes.md`](./raw/express-netlify-security-notes.md) | Difesa in profondità: body limit JSON **e** urlencoded, rate limit in sintesi, error middleware, checklist, **CORS/Helmet/favorites** (§6) |
| [`cors-how-it-works.md`](./raw/cors-how-it-works.md) | CORS generico: same-origin, ruolo browser/server, header, OPTIONS/preflight, esempi HTTP, `credentials`, checklist |
| [`content-security-policy.md`](./raw/content-security-policy.md) | CSP pratica: direttive, Netlify/Vite, errore inline script, vs CORS/Helmet → concept `content-security-policy` |
| [`helmet.md`](./raw/helmet.md) | Helmet.js: header HTTP, moduli (CSP, HSTS, frameguard, …), esempio Express completo |
