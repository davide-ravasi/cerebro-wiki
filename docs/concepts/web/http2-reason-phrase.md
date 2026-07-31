---
id: concept-http2-reason-phrase
title: "HTTP/2 dropped the reason phrase (statusText is empty)"
type: concept
domain: web
sources:
  - note: raw-nodejs-fetch-two-steps-and-http-errors
tags: [concept, web, http, http2, fetch, error-handling, browser]
status: evergreen
confidence: high
updated: 2026-07-31
---

# Definition

HTTP/1.1 status lines carry a human-readable **reason phrase** (`404 Not Found`). HTTP/2 removed it
from the protocol. On an HTTP/2 response, `response.statusText` in the browser is an **empty string**,
while `response.status` still carries the number.

# Why It Matters

This is a defect that **passes every local test and only appears in production**, because local dev
servers and production servers usually speak different protocol versions. An error message built from
`statusText` alone reads `Error fetching data: ` — informative locally, blank once deployed.

# How It Works

The reason phrase was always advisory: HTTP/1.1 clients were told to ignore it and rely on the status
code. HTTP/2's binary framing simply stopped carrying it, and browsers surface that as `statusText === ""`.

The asymmetry that hides the bug:

| Where | Protocol | `statusText` |
|---|---|---|
| `npx serve`, `python -m http.server` | HTTP/1.1 | `"Not Found"` |
| GitLab Pages, most CDNs, most HTTPS hosts | HTTP/2 | `""` |

Verify with `curl -sI <url>` — the first line names the version.

# Tradeoffs

- **Pros of relying on `status`:** it is the value the spec guarantees, in every version, and it is
  what code should branch on anyway.
- **Cons:** the numeric code alone is less readable in logs and UI, so dropping `statusText` entirely
  loses a little clarity where it *is* present.

# When To Use

- Build error messages from `status`, and treat `statusText` as an optional extra.
- Never branch logic on `statusText` — not even a string comparison in a test.

# Example

```js
throw new Error(`HTTP ${response.status} ${response.statusText}`.trim());
// HTTP/1.1 → "HTTP 404 Not Found"
// HTTP/2   → "HTTP 404"          (.trim() removes the dangling space)
```

The same idea generalises: when a value is optional in the transport, compose the message so it
degrades instead of collapsing.

# Related Concepts

- [[concept-http-errors-in-fetch]]
- [[concept-fetch-response-model]]
