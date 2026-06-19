---
id: map-web-security
title: "Web Security Map (browser, HTTP headers, Express/Netlify)"
type: map
domain: web
tags: [map, navigation, web, security, cors, csp, express, netlify]
status: evergreen
updated: 2026-05-19
---

# Purpose

Navigation for **browser security** notes learned while building Track'em All and related Node/Express material: CORS, CSP, rate limiting, Helmet, Netlify headers.

# Core Concepts

- [[concept-content-security-policy]]

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

# Open Threads

- Server-only env names (`JWT_SECRET`, `MONGODB_URI`) vs `VITE_*`
- Stricter **production-only** CSP (`script-src 'self'` without `'unsafe-inline'`)
- Favorites route hardening + rate limit

# Related Maps

- [[map-databases]] (orthogonal; app data layer)
