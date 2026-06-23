---
id: source-mongodb-node-path-index
title: "MongoDB Node.js Developer Path — Source Index"
type: source-index
domain: databases
tags: [source, mongodb, course, index]
status: draft
updated: 2026-05-11
---

# Scope

Raw and formal **source** notes for the MongoDB learning path (Node.js developer track). Same layout as DDIA: working notes live under `raw/`; promoted English `source` files sit beside this README.

# Layout

- `raw/<module>/lesson-NN-topic.md` — quick captures while studying (any language).
- `<section-slug>.md` here — promoted source notes with YAML frontmatter (English).

# Modules (raw)

| Module | Path |
|--------|------|
| Document model | `raw/mongodb-document-model/` |

# Promoted source notes

- [[source-mongodb-connecting-section]] — `connecting-mongodb-mongo-shell.md`

# Related wiki

- [[map-mongodb]] — MongoDB-specific map
- [[map-databases]] — shared database concepts (indexes, transactions, etc.)
- [[concept-partitioning]] — ties to sharding when you reach cluster topics
- [[concept-database-index]] — ties to MongoDB indexes

# Checklist

- [x] `raw/README.md` with naming conventions
- [x] Create `maps/mongodb-map.md`
- [x] Promote "Connecting to MongoDB / mongo shell" section to formal source + concepts
- [ ] Promote `lesson-05-embedding-referencing.md`
- [ ] Extract more concept notes as lessons grow (aggregation, indexes, replica, transactions)
- [x] CRUD basics: [[concept-mongodb-find-queries]], [[concept-mongodb-insert-operations]]
