---
id: source-mongodb-connecting-section
title: "MongoDB — Connecting with mongosh (course section)"
type: source
domain: databases
sources:
  - kind: course
    title: "MongoDB Node.js Developer Path"
    section: "Connecting to MongoDB / mongo shell"
tags: [source, mongodb, course, mongosh, atlas, connection-string]
status: draft
confidence: medium
updated: 2026-05-11
---

# Summary

Normalized notes from the **Connecting to MongoDB / mongo shell** section of the MongoDB Node.js Developer Path. Two main topics:

1. Building and reading a **MongoDB connection string** (standard vs SRV, components, examples).
2. Using **`mongosh`** (commands, installation on Ubuntu, connecting to Atlas).

# Key Concepts

- [[concept-mongodb-connection-string]]
- [[concept-mongosh-basics]]

# Important Tradeoffs

- **SRV** (`mongodb+srv://`) simplifies Atlas URIs and topology discovery but depends on DNS; **standard** URIs are explicit and portable.
- **`mongosh`** is great for interactive admin and exploration; avoid using it as the main automation channel — prefer scripts or app code in source control.

# What Changed My Mental Model

- The connection string is a **single configuration surface**: protocol, auth, host(s), default DB, and behavior options all live there.
- Atlas labs reinforce that **credentials and host** must come from environment / secrets — never committed.

# Open Questions

- Production defaults for `retryWrites`, `w`, TLS in Node.js drivers.
- How does the Stable API version (`--apiVersion 1`) interact with driver versions over time?

# Related Notes

- Raw lessons:
  - `raw/mongodb-document-model/section- connecting-mongodb-mongo-shell/lesson-1-connection-string.md`
  - `raw/mongodb-document-model/section- connecting-mongodb-mongo-shell/lesson-2-mongosh.md`
- Maps: [[map-mongodb]], [[map-databases]]
