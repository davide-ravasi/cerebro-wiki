---
id: map-reading
title: "Reading Map"
type: map
domain: meta
tags: [map, navigation, books, reading-queue]
status: evergreen
updated: 2026-10-21
---

# Purpose

Single place to see **what to read next**, what is **in progress**, and what **feeds** `sources/` and `concepts/`.

Index: [`reading/README.md`](../reading/README.md) · Template: [`templates/reading-queue-entry.md`](../templates/reading-queue-entry.md)

# Active (reading now)

| Book | Queue note | Source notes | Progress |
|------|------------|--------------|----------|
| Refactoring (Fowler, 2nd) | [[reading-refactoring-fowler]] | `sources/refactoring/` (TBD) | Started 21/10 — metodo → smells → catalog |

# Queue (want to read)

| Priority | Book | Why (short) | Queue note |
|----------|------|-------------|------------|
| medium | The Pragmatic Programmer (Thomas & Hunt, 2019) | Tracer bullets, small tasks — with Fowler in AI workflow | [[reading-pragmatic-programmer]] |
| medium | Head First Software Architecture (Gandhi, Richards, Ford, 2024) | Architectural thinking, tradeoffs, presentations + tracking-ds | [[reading-head-first-software-architecture]] |
| medium | Web Performance Fundamentals (Makarevich, 2025) | Core Web Vitals, profiling, React perf journey | [[reading-web-performance-fundamentals]] |
| optional | Software Architecture Monday (Richards, video series) | **Optional:** 10-15 cherry-picked videos if curious; overlaps with Head First SA | [[reading-software-architecture-monday]] |
| surplus | DDIA ch. 11–12 | Only if needed later | [[reading-ddia]] |

# Paused

*(none)*

# Done

| Book | Queue note | Notes |
|------|------------|--------|
| Designing Data-Intensive Applications (path) | [[reading-ddia]] | Path chiuso 21/10 (cap.9–10 core). Sources: [[source-ddia-index]] |

# By domain (cross-link)

| Domain | Active sources | Related maps |
|--------|----------------|--------------|
| Distributed systems | [[source-ddia-index]] | [[map-distributed-systems]], [[map-databases]] |
| Databases / MongoDB | [[source-mongodb-connecting-section]] | [[map-mongodb]] |
| Software craft / refactoring | — | *(after Fowler: link concepts)* |
| Software architecture | — | [[reading-head-first-software-architecture]] → Fundamentals of SA (later); [[reading-software-architecture-monday]] optional |
| Web / frontend performance | — | [[map-web-security]] *(perf concepts TBD)* |
| AI / agents (surplus Ven) | [[source-ai-agents-index]] | [[map-agents-lab]] |

# Open threads

- Add Clean Code / Clean Architecture / DDD when you pick them from the Matt Pocock `agent-rules-books` list
- After starting Fowler: extract `concept-refactoring`, `concept-code-smell` into `concepts/software-engineering/`
- **Learning path priority** (agg. 21/10): DDIA path ✓ → **Fowler** (now) → Pragmatic → Head First SA. SAM optional. DDIA 11–12 surplus.
