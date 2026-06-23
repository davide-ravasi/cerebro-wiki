---
id: concept-mongosh-basics
title: "mongosh Basics"
type: concept
domain: databases
sources:
  - note: source-mongodb-connecting-section
tags: [concept, databases, mongodb, mongosh, cli, shell]
status: evergreen
confidence: medium
updated: 2026-05-11
---

# Definition

**`mongosh`** is the official **MongoDB Shell**: an interactive JavaScript REPL used to **connect**, **inspect**, and **operate** MongoDB deployments (local instances and Atlas clusters) from the terminal.

# Why It Matters

It is the standard way to **verify connectivity**, run **ad-hoc queries**, manage **databases and collections**, and validate **operations** before wiring them into application code or scripts.

# How It Works

`mongosh` reads a [[concept-mongodb-connection-string]] and opens a session against the target deployment. Inside the shell, common navigation commands are:

| Command            | Effect                       |
| ------------------ | ---------------------------- |
| `db`               | Show the current database    |
| `use <name>`       | Switch to another database   |
| `show dbs`         | List databases               |
| `show collections` | List collections in `db`     |
| `help`             | Show available commands      |
| `exit`             | Close the shell              |

Connect with an inline URI:

```bash
mongosh "<connection-string>"
```

Atlas cluster (Stable API):

```bash
mongosh "mongodb+srv://<cluster-host>" --apiVersion 1 --username <db_username>
```

# Tradeoffs

- **Pros:** quick feedback loop, scripting in JS, same query language as the drivers.
- **Cons:** not a substitute for application-level testing; destructive commands run with the same privileges as the connected user.

# When To Use

- Verify cluster reachability and credentials.
- Explore data and indexes interactively.
- Run small admin scripts; avoid for repeated production tasks (prefer scripts in source control or proper tooling).

# Example

Quick check against a local instance:

```bash
mongosh "mongodb://localhost:27017/trackemall"
> show collections
> db.users.findOne()
> exit
```

# Related Concepts

- [[concept-mongodb-connection-string]]
- [[concept-mongodb-find-queries]]
- [[concept-mongodb-insert-operations]]
- [[concept-database-index]]
- [[map-mongodb]]
- [[map-databases]]

# Related Patterns

- Add a `pattern-` note when you capture your team's preferred way to install / pin `mongosh` (Docker image, package, etc.).
