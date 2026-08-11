---
id: reading-ddia
title: "Designing Data-Intensive Applications"
type: reading-queue
authors: ["Martin Kleppmann"]
domain: distributed-systems
tags: [reading-queue, books, ddia, distributed-systems, databases]
status: reading
priority: high
updated: 2026-07-30
found_via: "Primary cerebro study track"
why: "Foundation for distributed data, transactions, consistency, and operations at scale."
related_skills:
  - learn-core-idea-first
  - learn-error-simulator
related_concepts:
  - concept-write-skew
  - concept-linearizability
  - concept-quorum-majority-truth
source_slug: sources/designing-data-intensive-applications/
---

# Progress snapshot

| Chapters | State |
|----------|--------|
| 7–8 | Promoted source + concepts + book club |
| 9 | Partial — lin. + TOB + 2PC ✓ · consensus idea ✓ · **Membership core-idea ✓ (Jul 30)**; Raft detail / CAP TBD |
| 1–6, 10–12 | Not promoted yet |

**Source index:** [[source-ddia-index]]  
**Raw notes:** `sources/designing-data-intensive-applications/raw/`

## Session log (resume hints)

- **2026-08-11** — Rilettura a mano pp. 18–23: cost of linearizability (CAP, perf vs fault-tolerance), ordering guarantees, ordering & causality (total vs partial order), sequence number ordering + non-causal generators → raw aggiornato. Next: Lamport timestamps / TOB in dettaglio.
- **2026-08-07** — Linearizability a mano ✓ digitata (cosa rende lin. + when useful: locks/leader→consensus, uniqueness, cross-channel races).
- **2026-08-05** — Rilettura a mano: intro cap. 9 (~5 pp) + inizio Linearizability → raw aggiornato.
- **2026-07-30** — Membership & coordination (ZK/etcd): `@learn-core-idea-first` → raw § Membership. Optional: `@learn-error-simulator`.
- **2026-07-29** — 2PC ripasso + fault-tolerant consensus idea (vs 2PC blocking).
- **2026-07-28** — *Distributed transactions in practice* ✓. Next macros were Fault-Tolerant Consensus → Membership.
- **2026-07-27** — 2PC ~20 min: single node→distributed, intro 2PC, system of promises, coordinator failure.
- **2026-07-24** — bookmark: § *Distributed transactions and consensus* — **atomic commit / 2PC** (started, after TOB).
- **Jul 2026** — total order broadcast: `@learn-core-idea-first` → raw § TOB. Promote `concept-total-order-broadcast` a fine blocco consensus.

## Chat hook

> "Riprendo DDIA — [[reading-ddia]] — sono al cap. N / tema X."
