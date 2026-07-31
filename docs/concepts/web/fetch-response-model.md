---
id: concept-fetch-response-model
title: "fetch Response model (why two awaits)"
type: concept
domain: web
sources:
  - note: raw-nodejs-fetch-two-steps-and-http-errors
tags: [concept, web, http, fetch, promises, streams, browser]
status: evergreen
confidence: high
updated: 2026-07-31
---

# Definition

A browser `fetch()` needs **two** asynchronous steps to produce data: the first resolves to a
`Response` — the envelope of the HTTP response (status, headers, and the body as an unread stream) —
and the second reads that body to the end and parses it.

# Why It Matters

The two-step shape is the single most common source of confusion when learning `fetch`, because it
looks like ceremony. It is not: it mirrors how HTTP actually arrives on the wire. Understanding it
explains why `await fetch(...)` alone never gives you data, and why the `Response` is the only place
where status and headers can be inspected before the body is consumed.

# How It Works

1. `fetch(url)` returns a Promise **immediately** — never the data.
2. That Promise resolves as soon as the **headers** arrive. Its value is a `Response`: status,
   headers, and a body that is still an **unread stream**. The body may still be in flight.
3. `response.json()` reads the stream to completion and runs `JSON.parse` on it. Reading a stream is
   itself asynchronous, hence a second Promise.

So the two Promises mean **"the response has started"** and **"the response has finished and been
parsed"**.

`.json()` is not a format conversion. It is *finish downloading, then parse*. The same holds for
`.text()`, `.blob()` and `.arrayBuffer()` — each consumes the same single stream.

# Tradeoffs

- **Pros:** headers are actionable before the payload lands (redirect, content type, auth challenge,
  early abort on a wrong status). Streaming stays possible for large bodies.
- **Cons:** the extra step reads as boilerplate and invites the mistake of inspecting the `Response`
  after the body has already been consumed. The body can be read **once** — a second `.json()` on the
  same `Response` throws.

# When To Use

- Use the `Response` step to check `status`/`ok` and headers — see [[concept-http-errors-in-fetch]].
- Avoid reaching for `FileReader` here: `FileReader` reads a `Blob`/`File` you **already hold** in
  memory (file input, drag & drop, constructor) and never performs a network request. Network and
  in-memory bytes are different sources, not two styles of the same call.

# Example

```js
fetch("./latest.json")
  .then(response => response.json())   // headers in hand → start reading the body
  .then(data => render(data))          // body read and parsed
```

Note that a page opened over `file://` cannot fetch at all: the browser's security model works per
**origin** (scheme + host + port), and `file://` has no usable one. Requests are blocked before any
of the above happens, which surfaces as `TypeError: Failed to fetch` and looks like a bug in the
calling code. Serving the folder over HTTP (any static server) is the fix.

# Related Concepts

- [[concept-http-errors-in-fetch]]
- [[concept-http2-reason-phrase]]
- [[concept-content-security-policy]]
