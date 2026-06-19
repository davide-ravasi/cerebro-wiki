---
id: concept-fencing-token
title: "Fencing Token"
type: concept
domain: distributed-systems
sources:
  - note: source-ddia-ch-08
tags: [concept, distributed-systems, locks, leader-election, safety]
status: evergreen
confidence: medium
updated: 2026-05-19
---

# Definition

A **fencing token** is a **monotonically increasing number** issued by a lock service or leader election system. Every **write** to shared storage must include the token; storage **rejects** writes with a token lower than the highest it has already accepted.

# Why It Matters

**Leases** and local leader checks fail when a node has a **false belief** it is still leader after **[[concept-process-pause]]** or clock skew — the lease expired elsewhere but the process resumes and writes anyway. Fencing **blocks stale leaders** from corrupting data even if they still think they are active.

# How It Works

1. Lock service (e.g. ZooKeeper, etcd) grants lock / leadership and returns token **T = 42**.
2. Leader performs work and sends write with **token 42**.
3. Lock expires; new leader gets **token 43**.
4. Old leader wakes up, sends write with **token 42** → storage rejects (43 already seen).

The token must be checked by the **resource being protected** (disk, DB, object store) — not only by the client’s local logic.

# Tradeoffs

- **Pros:** simple, strong protection against stale primary writes.
- **Cons:** storage layer must support it; not all databases expose fencing; token service must remain available and correct.

# When To Use

Shared mutable state with leader + lease pattern (HDFS namenode, distributed locks, primary-backup with manual failover).

# Example

Fig. 8-4 style scenario: delayed former leader writes after new leader committed — fencing token prevents the late write from landing.

# Related Concepts

- [[concept-process-pause]]
- [[concept-quorum-majority-truth]]
- [[concept-timeout-failure-detection]]
- [[pattern-leader-based-replication]]
- [[map-distributed-systems]]
