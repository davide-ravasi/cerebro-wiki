---
id: concept-mongodb-insert-operations
title: "MongoDB Insert Operations (insertOne / insertMany)"
type: concept
domain: databases
sources:
  - note: source-mongodb-connecting-section
tags: [concept, databases, mongodb, crud, write, mongosh]
status: evergreen
confidence: medium
updated: 2026-05-19
---

# Definition

**`insertOne`** adds a **single document** to a collection; **`insertMany`** adds **multiple documents** in one operation. MongoDB generates an **`_id`** automatically if you omit it.

# Why It Matters

Creates are the entry point for new users, orders, logs, and imported data. Return values include **`insertedId`** / **`insertedIds`** and **`acknowledged`** — needed for redirects, idempotency, and error handling in Node.js APIs.

# How It Works

```javascript
db.collection.insertOne({ ... })    // one document
db.collection.insertMany([ ... ])   // array of documents
```

**mongosh:**

```javascript
use trackemall

// single document
db.users.insertOne({
  name: 'Alice',
  email: 'alice@example.com',
  createdAt: new Date(),
})

// batch
db.movies.insertMany([
  { title: 'Film A', year: 2020 },
  { title: 'Film B', year: 2021 },
])
```

**Node.js driver:**

```javascript
const users = db.collection('users');

const result = await users.insertOne({
  name: 'Alice',
  email: 'alice@example.com',
  createdAt: new Date(),
});

console.log(result.insertedId);   // ObjectId
console.log(result.acknowledged); // true if write concern satisfied

const batch = await users.insertMany([
  { name: 'Bob', email: 'bob@example.com' },
  { name: 'Carol', email: 'carol@example.com' },
]);

console.log(batch.insertedIds); // { '0': ObjectId(...), '1': ObjectId(...) }
```

### Return shape (driver)

| Field | `insertOne` | `insertMany` |
|-------|-------------|--------------|
| `acknowledged` | boolean | boolean |
| `insertedId` | single `ObjectId` | — |
| `insertedIds` | — | map index → `ObjectId` |

### Optional `_id`

You may set `_id` yourself (string, UUID, etc.). It must be **unique** in the collection — duplicate `_id` causes **duplicate key error**.

```javascript
await users.insertOne({ _id: 'user-42', name: 'Dave' });
```

### `insertMany` options

```javascript
await users.insertMany(docs, { ordered: false });
```

- **`ordered: true`** (default) — stop on first error.
- **`ordered: false`** — continue inserting other docs after an error (useful for bulk imports).

# Tradeoffs

- **insertOne** — simple, clear semantics, one round trip.
- **insertMany** — fewer round trips for batches; large arrays increase memory and single-operation size limits (BSON document limit **16 MB** per doc; batch size matters for throughput).
- Unique indexes (e.g. on `email`) make inserts fail on duplicates — handle **E11000** in app code.

# When To Use

- **insertOne** — user signup, single resource create, one event log entry.
- **insertMany** — seed scripts, imports, bulk analytics ingest (consider `ordered: false` for partial success).

# Example

After `insertOne` in an Express route, return `201` with `{ id: result.insertedId }` and use that id in a subsequent [[concept-mongodb-find-queries]] `findOne({ _id })`.

# Related Concepts

- [[concept-mongodb-find-queries]]
- [[concept-mongosh-basics]]
- [[concept-mongodb-connection-string]]
- [[concept-database-index]]
- [[map-mongodb]]
