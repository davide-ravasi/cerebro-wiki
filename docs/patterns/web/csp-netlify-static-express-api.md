---
id: pattern-csp-netlify-static-express-api
title: "CSP on Netlify static; Helmet without CSP on Express API"
type: pattern
domain: web
sources:
  - note: raw-nodejs-content-security-policy
tags: [pattern, web, security, netlify, express, helmet, csp]
status: evergreen
confidence: medium
updated: 2026-05-19
---

# Problem

You have a **Vite SPA** on Netlify static hosting and an **Express** Netlify Function for JSON API. Where do you put **Content-Security-Policy** and **Helmet** without duplicating rules or breaking Vite dev?

# Decision

1. **Full CSP** (and page-level headers: HSTS, Permissions-Policy, etc.) on **static assets** via `netlify.toml` `[[headers]]` `for = "/*"` (or `public/_headers` for paths you control).
2. **Helmet on the Function** with **`contentSecurityPolicy: false`**; keep lightweight headers (`noSniff`, `frameguard`, `hsts`, `referrerPolicy`) on API responses.
3. Tune CSP for **`netlify dev`**: `script-src` must allow Vite inline refresh; `connect-src` must allow HMR WebSocket ports.

# Rationale

- CSP applies to the **document** and what the page loads; the built `index.html` and assets are served from Netlify CDN, not from every JSON response.
- One CSP source of truth avoids drift between Helmet and Netlify.
- API JSON does not need a TMDB/YouTube CSP on every response.

# Tradeoffs

- Same `netlify.toml` CSP runs in **production and `netlify dev`** — dev-friendly rules (e.g. `'unsafe-inline'`, localhost `ws:`) may carry into production unless you split contexts later.
- Pure `npm run dev` on Vite bypasses Netlify headers — team must know which command they use.

# Example

See Track'em All: `netlify.toml` + `functions/express.js` Helmet block.

# Related

- [[concept-content-security-policy]]
- `sources/nodejs/raw/express-netlify-security-notes.md` (§6.2)
