---
id: map-web-security
title: "Web Security Map (browser, HTTP headers, Express/Netlify)"
type: map
domain: web
tags: [map, navigation, web, security, cors, csp, express, netlify, fetch, http]
status: evergreen
updated: 2026-07-31
---

# Purpose

Navigation for **browser security** notes learned while building Track'em All and related Node/Express material: CORS, CSP, rate limiting, Helmet, Netlify headers.

# Core Concepts

- [[concept-content-security-policy]]

## HTTP client (fetch)

- [[concept-fetch-response-model]] — why two awaits: `Response` envelope vs body stream
- [[concept-http-errors-in-fetch]] — `fetch` does not reject on 404; check `response.ok`
- [[concept-http2-reason-phrase]] — `statusText` is empty over HTTP/2; use `status`

# Patterns

- [[pattern-csp-netlify-static-express-api]]

# Raw / Source Notes (Node.js path)

| Topic | File |
|-------|------|
| CORS (generic) | `sources/nodejs/raw/cors-how-it-works.md` |
| CSP (project pitfalls) | `sources/nodejs/raw/content-security-policy.md` |
| Express hardening checklist | `sources/nodejs/raw/express-netlify-security-notes.md` |
| Rate limit + client IP | `sources/nodejs/raw/rate-limiting-and-client-ip.md` |
| Helmet (overview) | `sources/nodejs/raw/helmet.md` |
| fetch: two steps, HTTP errors, HTTP/2, `file://` | `sources/nodejs/raw/fetch-two-steps-and-http-errors.md` |

# Open Threads

- Server-only env names (`JWT_SECRET`, `MONGODB_URI`) vs `VITE_*`
- Stricter **production-only** CSP (`script-src 'self'` without `'unsafe-inline'`)
- Favorites route hardening + rate limit

# Related Maps

- [[map-databases]] (orthogonal; app data layer)
