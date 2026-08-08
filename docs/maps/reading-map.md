---
id: map-reading
title: "Reading Map"
type: map
domain: meta
tags: [map, navigation, books, reading-queue]
status: evergreen
updated: 2026-07-20
---

# Purpose

Single place to see **what to read next**, what is **in progress**, and what **feeds** `sources/` and `concepts/`.

Index: [`reading/README.md`](../reading/README.md) · Template: [`templates/reading-queue-entry.md`](../templates/reading-queue-entry.md)

# Active (reading now)

| Book | Queue note | Source notes | Progress |
|------|------------|--------------|----------|
| Designing Data-Intensive Applications | [[reading-ddia]] | [[source-ddia-index]] | Ch. 7–9 promoted; rest TBD |

# Queue (want to read)

| Priority | Book | Why (short) | Queue note |
|----------|------|-------------|------------|
| high | Refactoring (Fowler) | Code structure + smells; pairs with TDD / Matt Pocock skills | [[reading-refactoring-fowler]] |
| medium | The Pragmatic Programmer (Thomas & Hunt, 2019) | Tracer bullets, small tasks — with Fowler in AI workflow | [[reading-pragmatic-programmer]] |
| medium | Web Performance Fundamentals (Makarevich, 2025) | Core Web Vitals, profiling, React perf journey | [[reading-web-performance-fundamentals]] |
| medium | Head First Software Architecture (Gandhi, Richards, Ford, 2024) | Architectural thinking, tradeoffs, presentations + tracking-ds | [[reading-head-first-software-architecture]] |
| low | Software Architecture Monday (Richards, video series) | 10-min architecture patterns — companion to DDIA; start after Ch. 9-12 | [[reading-software-architecture-monday]] |

# Paused

*(none)*

# Done

*(move entries here when finished; keep links to source indexes)*

# By domain (cross-link)

| Domain | Active sources | Related maps |
|--------|----------------|--------------|
| Distributed systems | [[source-ddia-index]] | [[map-distributed-systems]], [[map-databases]] |
| Databases / MongoDB | [[source-mongodb-connecting-section]] | [[map-mongodb]] |
| Software craft / refactoring | — | *(after Fowler: link concepts)* |
| Software architecture | — | [[reading-software-architecture-monday]] (video) → [[reading-head-first-software-architecture]] → Fundamentals of SA (later) |
| Web / frontend performance | — | [[map-web-security]] *(perf concepts TBD)* |

# Open threads

- Add Clean Code / Clean Architecture / DDD when you pick them from the Matt Pocock `agent-rules-books` list
- After starting Fowler: extract `concept-refactoring`, `concept-code-smell` into `concepts/software-engineering/`
- **Software Architecture Monday**: Start after DDIA Ch. 9-12 complete; use Friday slot (1-2 videos/week); creates bridge to Head First SA book
