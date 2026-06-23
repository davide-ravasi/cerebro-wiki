---
id: concept-mongodb-find-queries
title: "MongoDB Find Queries (find / findOne)"
type: concept
domain: databases
sources:
  - note: source-mongodb-connecting-section
tags: [concept, databases, mongodb, queries, crud, mongosh]
status: evergreen
confidence: medium
updated: 2026-05-19
---

# Definition

**`find`** and **`findOne`** read documents from a **collection** that match a **filter** (a BSON/JSON predicate). `findOne` returns a single document or `null`; `find` returns a **cursor** over zero or more documents.

# Why It Matters

Almost every read path in a MongoDB app starts here — API handlers, auth lookups, lists, detail pages. The same filter syntax works in **mongosh**, the **Node.js driver**, and other official drivers.

# How It Works

```javascript
// filter = "documents where this is true"
db.collection.findOne({ email: 'alice@example.com' })
db.collection.find({ year: { $gte: 2020 } })
```

| Method | Returns | Typical use |
|--------|---------|-------------|
| `findOne(filter)` | One doc or `null` | Login, fetch by unique key |
| `find(filter)` | Cursor | Lists, search, iteration |

**mongosh:**

```javascript
use trackemall

db.users.findOne({ email: 'alice@example.com' })

db.movies.find({ year: 2020 }).limit(10).sort({ title: 1 })

db.movies.find(
  { genre: 'drama' },
  { title: 1, year: 1, _id: 0 }  // projection
)
```

**Node.js driver:**

```javascript
const users = db.collection('users');

const user = await users.findOne({ email: 'alice@example.com' });

const movies = await users
  .find({ active: true })
  .sort({ createdAt: -1 })
  .limit(20)
  .toArray();
```

### Common filter operators

| Operator | Meaning | Example |
|----------|---------|---------|
| (field equality) | equals | `{ status: 'active' }` |
| `$eq`, `$ne` | equal / not equal | `{ age: { $ne: 0 } }` |
| `$gt`, `$gte`, `$lt`, `$lte` | comparisons | `{ price: { $lt: 100 } }` |
| `$in`, `$nin` | in / not in array | `{ tag: { $in: ['js', 'node'] } }` |
| `$and`, `$or` | logic | `{ $or: [{ a: 1 }, { b: 2 }] }` |
| dot notation | nested field | `{ 'address.city': 'Rome' }` |

**By `_id`:**

```javascript
const { ObjectId } = require('mongodb');

await users.findOne({ _id: new ObjectId('664a1b2c3d4e5f6a7b8c9d0') });
```

### Cursor options (chain on `find`)

- `.sort({ field: 1 })` — `1` asc, `-1` desc
- `.limit(n)` — cap results
- `.skip(n)` — pagination (prefer range queries on indexed fields at scale)
- `.project({ field: 1 })` — include/exclude fields

# Tradeoffs

- **Pros:** flexible document-shaped filters; same API in shell and app.
- **Cons:** no SQL-style joins in one `find` — embed, `$lookup`, or multiple queries; unindexed filters scan the collection.

# When To Use

- **findOne** when you expect at most one match (unique key, `_id`).
- **find** for lists and batch reads; always **limit** unbounded queries in APIs.

# Example

Track'em All: load a user profile by `_id`, list movies with `year` and `genre` filters, paginate with `.sort()` + `.limit()`.

# Related Concepts

- [[concept-mongosh-basics]]
- [[concept-mongodb-connection-string]]
- [[concept-mongodb-insert-operations]]
- [[concept-database-index]]
- [[map-mongodb]]
