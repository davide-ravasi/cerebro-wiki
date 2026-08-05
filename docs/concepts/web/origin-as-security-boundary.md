---
id: concept-origin-as-security-boundary
title: "The origin as security boundary (embed freely, read never)"
type: concept
domain: web
sources:
  - note: raw-nodejs-origin-cors-and-the-five-layers
tags: [concept, web, security, cors, same-origin, browser, http]
status: evergreen
confidence: high
updated: 2026-08-04
---

# Definition

The browser's unit of trust is the **origin** — the triple `scheme + host + port`. Anything from
another origin may be **embedded** (rendered, executed, applied) but its **content may not be read**
by JavaScript. CORS is the opt-in protocol by which a server grants an origin the right to read.

# Why It Matters

Almost every cross-origin surprise reduces to this one rule, and the rule is not intuitive: a
resource can be delivered successfully, appear in the network panel with status 200, and still be
unusable. Knowing where the boundary sits tells you which failures are configurable (the server can
grant access) and which are not (the client cannot ever bypass it).

# How It Works

The boundary is drawn around **reading**, not using:

| Cross-origin resource | Allowed without CORS | What the boundary protects |
|---|:--:|---|
| `<img>` | yes | rendered, but `canvas.getImageData()` taints and throws |
| `<script src>` | yes | executed, but source unreadable and errors sanitised to `"Script error"` |
| `<link rel="stylesheet">` | yes | applied, but `styleSheet.cssRules` throws `SecurityError` |
| `fetch` / `XHR` | **no** | the body is handed to JS by definition |
| `@font-face` | **no** | the deliberate exception — see below |

The protocol itself is three steps, and the decision belongs to the **server**:

1. the browser attaches `Origin: <caller>` to the request
2. the server may answer with `Access-Control-Allow-Origin`
3. the browser compares them and decides whether to hand the bytes to the code

Two consequences that mislead people. First, **the request is still sent and served** — CORS
withholds delivery to the code, not the network round trip, so a 200 proves nothing about
permission. Second, **only browsers enforce it**: `curl` has no origin to protect, so a successful
`curl` is not evidence that a page will work.

When a response carries `Vary: Origin`, the server is announcing that its answer **differs per
caller** — which also means a measurement taken from one origin does not describe another.

# Tradeoffs

- **Pros:** hostile pages cannot read your data or your users' authenticated responses; the legacy
  embedding web keeps working; servers stay in control of who may read them.
- **Cons:** the embed/read split looks arbitrary until you know the rule, and failures surface far
  from their cause — a missing header on a font server appears as unstyled text in someone else's
  page. Fonts break the rule for licensing rather than security reasons, which makes the model
  harder to teach.

# When To Use

- Reach for CORS when JS must **read** a cross-origin response, and remember only the *target*
  server can grant it.
- When you cannot get the header added, **move the resource to your own origin** — served
  same-origin, CORS never applies at all. This is the reliable fix for third-party fonts.
- Never conclude from `localhost` about production: origin-dependent behaviour (CORS, cookies, CSP,
  `Secure`, service workers) differs by definition.

# Example

A static page on `https://site.gitlab.io` links a design-system stylesheet from a corporate CDN.
The stylesheet applies correctly — `<link>` needs no CORS. The `@font-face` files it references all
fail with CORS errors, because the CDN's allowlist covers the development origin but not the
published one. The stylesheet is fine, the typography silently falls back.

The fix that cannot fail: copy the handful of font files actually rendered into the site's own output
directory and declare `@font-face` with relative URLs. Same origin, no permission needed.

# Related Concepts

- [[concept-http-errors-in-fetch]] — the same "delivered ≠ successful" gap, one layer up
- [[concept-fetch-response-model]] — why `file://` cannot fetch: no usable origin
- [[concept-content-security-policy]] — the other origin-based control, restricting what *you* load
