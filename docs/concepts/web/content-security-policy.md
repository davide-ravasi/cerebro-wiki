---
id: concept-content-security-policy
title: "Content Security Policy (CSP)"
type: concept
domain: web
sources:
  - note: raw-nodejs-content-security-policy
  - note: raw-nodejs-cors-how-it-works
tags: [concept, web, security, http, csp, browser, xss]
status: evergreen
confidence: medium
updated: 2026-05-19
---

# Definition

**Content Security Policy (CSP)** is a browser-enforced rule set, delivered via the HTTP header `Content-Security-Policy` (or a `<meta>` tag), that limits **what a web page may load or execute**: scripts, styles, images, `fetch`/WebSocket targets, iframes, workers, and more.

It is a **whitelist per resource type** (directives), not a single global list of domains.

# Why It Matters

- Reduces impact of **XSS**: even if attacker-controlled HTML/JS is injected, the browser may refuse to run scripts or load assets from disallowed sources.
- Documents explicitly which third parties the app trusts (APIs, CDNs, embeds).
- Complements but does **not** replace: input validation, auth, **CORS**, or server-side checks.

CSP is enforced by the **browser** when rendering or running code **in the page context**. It does not block `curl` or server-to-server calls.

# How It Works

1. The server (or CDN) sends `Content-Security-Policy: directive1 value; directive2 value; …`.
2. The browser applies each **directive** when the page (or worker) tries to load a resource.
3. On violation: request is blocked; DevTools shows a CSP error.

## Common directives

| Directive | Controls |
|-----------|----------|
| `default-src` | Fallback when a more specific directive is missing |
| `script-src` | JavaScript (external files **and** inline scripts) |
| `style-src` | CSS (files and inline styles) |
| `img-src` | Images |
| `connect-src` | `fetch`, XHR, `EventSource`, **WebSocket** |
| `font-src` | Web fonts |
| `frame-src` | `<iframe>` embeds |
| `worker-src` | Service workers / dedicated workers |

## Source values

- **`'self'`** — same origin as the document (scheme + host + port).
- **Host URLs** — e.g. `https://api.themoviedb.org`, `https://image.tmdb.org`.
- **Keywords** — `'unsafe-inline'`, `'unsafe-eval'`, `data:`, `blob:` (use sparingly; each weakens the policy).
- **Hashes / nonces** — allow specific inline scripts without full `'unsafe-inline'` (stricter; harder with dev tooling).

If `script-src` is **omitted**, the browser falls back to **`default-src`**. Inline scripts are **not** allowed unless you add `'unsafe-inline'`, a hash, or a nonce.

# Tradeoffs

- **Strict CSP** (`script-src 'self'` only, no inline): strong XSS mitigation; may break dev tools (Vite React Refresh injects inline script) or legacy inline snippets.
- **`'unsafe-inline'` on `script-src`**: easier local dev and some frameworks; weaker against inline XSS.
- **Broad `connect-src`** (e.g. many `ws://localhost` ports): needed for HMR; avoid carrying unnecessary localhost rules into production if you split policies.
- **CSP on API JSON responses**: usually low value; policy matters most on the **HTML document** and static app shell.

# When To Use

- **Use** on the **SPA shell** (static hosting, `netlify.toml` `[[headers]]`, `_headers`, or reverse proxy).
- **Use** explicit directives per resource type your app actually needs (TMDB, fonts, YouTube embeds, your API origin).
- **Avoid** duplicating the same CSP on every JSON API response if static hosting already sets it once.
- **Split dev vs prod** when possible: strict production CSP; relaxed or no CSP when running `vite` alone on `:5173`.

# Example

Track'em All (Netlify static + Vite SPA): CSP in `netlify.toml` for `/*`:

```text
default-src 'self';
script-src 'self' 'unsafe-inline';
connect-src 'self' https://api.themoviedb.org ws://localhost:5173 …;
img-src 'self' data: https://image.tmdb.org;
style-src 'self' 'unsafe-inline' https://fonts.googleapis.com;
font-src 'self' https://fonts.gstatic.com;
frame-src https://www.youtube.com https://www.youtube-nocookie.com;
worker-src 'self';
```

- **TMDB** → `connect-src` + `img-src`
- **YouTube trailer iframe** → `frame-src`
- **Vite + `netlify dev`** → `script-src 'unsafe-inline'` (React Refresh) + `connect-src` WebSocket ports for HMR
- **PWA service worker** → `worker-src 'self'`

Pattern with Express: CSP on **Netlify static**; **Helmet** on the Function with `contentSecurityPolicy: false` to avoid two sources of truth.

# Related Concepts

- CORS (cross-origin **read** of responses by scripts) — see `sources/nodejs/raw/cors-how-it-works.md`
- Security headers (`X-Frame-Options`, `nosniff`, HSTS) — overlap with Helmet / `netlify.toml`; CSP is the fine-grained load policy

# Related Patterns

- [[pattern-csp-netlify-static-express-api]]

# Related Sources

- `sources/nodejs/raw/content-security-policy.md` (project notes + pitfalls)
- `sources/nodejs/raw/express-netlify-security-notes.md` (§6.2 Helmet vs Netlify)
