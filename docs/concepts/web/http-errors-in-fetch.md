---
id: concept-http-errors-in-fetch
title: "HTTP errors in fetch (fetch does not reject on 404)"
type: concept
domain: web
sources:
  - note: raw-nodejs-fetch-two-steps-and-http-errors
tags: [concept, web, http, fetch, error-handling, promises, browser]
status: evergreen
confidence: high
updated: 2026-07-31
---

# Definition

`fetch()` rejects only when the **request** fails — DNS failure, dropped connection, blocked origin.
An HTTP `404` or `500` is a **successful** fetch: the Promise resolves normally, carrying a `Response`
whose `ok` is `false`. Detecting HTTP errors is the caller's job.

# Why It Matters

Code that omits the check does not fail loudly — it fails **confusingly**. A 404 typically returns an
HTML error page, so the next step (`.json()`) throws a JSON parse error. The user sees
`Unexpected token '<'` and debugs the parser instead of the missing file.

# How It Works

The check belongs in the **first** `.then()` (or right after the first `await`), the only place where
the `Response` exists *before* the body is consumed:

```js
fetch(url)
  .then(response => {
    if (response.ok) {
      return response.json();                                   // chain continues with data
    }
    throw new Error(`HTTP ${response.status} ${response.statusText}`.trim());
  })
  .then(data => render(data))
  .catch(error => showError(error));
```

Two mechanics make this work:

- **`throw` inside a `.then()` callback rejects that step**, and the rejection skips every later
  `.then()` until the first `.catch()`. Returning a value instead would let the chain continue as if
  nothing had happened.
- Because all three failure families — transport, HTTP status, malformed body — converge on the same
  rejection path, a single `.catch()` covers them.

`response.ok` is true for any status in the 200–299 range; it is shorthand, not a substitute for
reading `status` when the distinction matters (401 vs 404 vs 500).

# Tradeoffs

- **Pros:** one error path for three failure kinds; the `Response` stays available for richer
  handling (retry on 503, redirect to login on 401).
- **Cons:** it is opt-in, so every call site must remember it. Wrapping `fetch` in a small helper that
  throws on `!ok` removes the repetition — at the cost of hiding a standard API behind a local one.

# When To Use

- Always, for any `fetch` whose failure should be visible to the user or to logs.
- Place the check **before** consuming the body. After `.json()` the body is gone (it can be read
  once) and the status check comes too late.
- Do not rely on `statusText` for the message — see [[concept-http2-reason-phrase]].

# Example

Two mistakes worth recognising, both from a real session:

```js
// ✗ response is the FIRST callback's parameter — not in scope here → ReferenceError
fetch(url).then(r => r.json()).then(data => { if (response.ok) { … } })

// ✗ even in scope, this runs after .json() already tried to parse the error page
```

# Related Concepts

- [[concept-fetch-response-model]]
- [[concept-http2-reason-phrase]]
