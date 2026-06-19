---
id: concept-byzantine-fault
title: "Byzantine Fault"
type: concept
domain: distributed-systems
sources:
  - note: source-ddia-ch-08
tags: [concept, distributed-systems, faults, security, consensus]
status: evergreen
confidence: medium
updated: 2026-05-19
---

# Definition

A **Byzantine fault** is when a node behaves **arbitrarily badly**: crash, incorrect responses, or **deliberately violating** the protocol (lying to peers). **Byzantine fault tolerance (BFT)** means the system remains correct despite such nodes, up to a bounded fraction of the cluster.

# Why It Matters

DDIA contrasts **crash faults** (node stops or is slow — Ch. 8 focus) with **Byzantine** behavior. Most datacenter systems assume nodes fail but do not **maliciously lie**; full BFT is costly. Still, **untrusted input** from users and the open internet requires validation — a different but related security concern.

# How It Works

- Classic **Byzantine Generals Problem:** agree on action when some generals are traitors.
- BFT protocols (e.g. PBFT) need **≥ 3f + 1** nodes to tolerate **f** Byzantine nodes — more messages, complexity, often hardware support.
- **Crash-only** quorum systems (Raft, most DB replication) assume nodes fail-stop or fail-slow, not strategic lying.

# Tradeoffs

- **Pros:** strongest fault model; relevant for blockchains, multi-party distrust, aerospace.
- **Cons:** **complicated**, higher overhead; overkill for typical internal microservices on trusted hardware.

# When To Use

- **Use** full BFT when participants **mutually distrust** each other.
- **Do not assume** BFT for a normal three-node Mongo replica set — design for crash + **[[concept-quorum-majority-truth]]** instead.
- **Do** validate untrusted client input on web apps (orthogonal to inter-node BFT).

# Example

A “healthy” node returns conflicting data to different peers to break consensus — crash-fault protocols may not detect this; BFT protocols can if fault count stays below threshold.

# Related Concepts

- [[concept-quorum-majority-truth]]
- [[concept-reliability]]
- [[map-distributed-systems]]
