---
id: concept-reliability
title: "Reliability"
type: concept
domain: distributed-systems
sources:
  - note: source-ddia-ch-01
tags: [concept, distributed-systems, reliability]
status: evergreen
confidence: medium
updated: 2026-04-28
---

# Definition

A system is reliable when it continues to deliver correct behavior even when faults occur.

# Why It Matters

Reliability protects user trust and reduces operational risk in production environments.

# How It Works

Reliability comes from fault detection, fault isolation, and fault tolerance mechanisms across hardware, software, and operational processes.

# Tradeoffs

- More redundancy improves resilience but increases cost and complexity.
- Stronger safeguards reduce outages but can slow down delivery.

# When To Use

- Always relevant for user-facing systems and critical internal platforms.
- Especially important when failures have high business impact.

# Example

A service with replicated storage and automated failover can survive a single node crash without interrupting user requests.

# Related Concepts

- [[concept-replication]]
- [[concept-partitioning]]

# Related Patterns

- [[pattern-leader-based-replication]]
