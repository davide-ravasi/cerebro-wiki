---
id: concept-mongodb-connection-string
title: "MongoDB Connection String"
type: concept
domain: databases
sources:
  - note: source-mongodb-connecting-section
tags: [concept, databases, mongodb, connection, atlas, uri]
status: evergreen
confidence: medium
updated: 2026-05-11
---

# Definition

A **MongoDB connection string** is a URI that tells a client (driver, `mongosh`, application) **how** and **where** to connect to a MongoDB deployment: protocol, credentials, hosts, default database, and options.

# Why It Matters

It is the single point that controls **authentication**, **topology discovery**, and **client behavior** (retries, write concern, TLS). Most operational and security problems with a Mongo client start here.

# How It Works

Two main URI formats:

- **Standard** — `mongodb://...` with explicit host(s) and port(s).
- **SRV** — `mongodb+srv://...` with a single DNS name; the client resolves **SRV** and **TXT** records to discover hosts and default options. Common on **Atlas**.

Generic template (standard form):

```text
mongodb://[username:password@]host1[:port1],...hostN[:portN]][/[defaultauthdb][?options]]
```

Key parts:

- **`mongodb://` vs `mongodb+srv://`** — protocol and discovery model.
- **`username:password@`** — credentials; special characters must be **URL-encoded**.
- **`host[:port]`** — server address; default port **27017** if omitted.
- **`/defaultauthdb`** — DB used for authentication (often `admin` if omitted).
- **`?options`** — query string (e.g. `retryWrites`, `w`, `tls`, `appName`).

# Tradeoffs

- **SRV (`+srv`)**: simpler URI, dynamic topology updates via DNS; needs proper DNS and outbound DNS access.
- **Standard URI**: explicit and self-contained; requires manual updates when hosts change.

# When To Use

- **`mongodb://`** for local / fixed deployments and when DNS SRV is not viable.
- **`mongodb+srv://`** for **Atlas** and any deployment exposing SRV records.
- Always pass the connection string from **environment variables** or secret managers — never commit credentials.

# Example

Local development:

```text
mongodb://localhost:27017/trackemall
```

Atlas (cloud) with options:

```text
mongodb+srv://<user>:<password>@cluster0.xxxxx.mongodb.net/trackemall?retryWrites=true&w=majority
```

# Related Concepts

- [[concept-mongosh-basics]]
- [[concept-database-index]]
- [[map-mongodb]]
- [[map-databases]]

# Related Patterns

- Configure connection options (`retryWrites`, `w`) per environment (dev/staging/prod) — pattern note optional.
