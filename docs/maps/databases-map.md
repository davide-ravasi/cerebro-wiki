---
id: map-databases
title: "Databases & Transactions Map"
type: map
domain: databases
tags: [map, navigation, databases, transactions, ddia]
status: evergreen
updated: 2026-05-19
---

# Purpose

Navigation for **transaction**, **locking**, and **isolation** notes (aligned with DDIA Chapter 7 and similar material).

# Core Concepts

## Isolation and MVCC

- [[concept-snapshot-isolation-mvcc]]

## Anomalies

- [[concept-lost-update]]
- [[concept-write-skew]]
- [[concept-phantom-read]]

## Serializable mechanisms

- [[concept-two-phase-locking]]
- [[concept-index-range-lock]]
- [[concept-serializable-snapshot-isolation]]
- [[concept-deadlock]]

## General

- [[concept-database-index]]
- [[concept-mongodb-connection-string]]
- [[concept-mongosh-basics]]

# Source Notes

- [[source-ddia-ch-07]] — DDIA Ch. 7 (transactions, isolation, serializability)
- [[source-mongodb-connecting-section]]

# Open Threads

- Read committed vs snapshot vs serializable — naming across databases
- MongoDB sharded transactions in production

# Related Maps

- [[map-distributed-systems]]
- [[map-mongodb]]
