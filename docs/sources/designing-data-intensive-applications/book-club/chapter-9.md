# DDIA Chapter 9 — Book club (Discord, partial)

Paste-ready. Full raw + wiki: [`raw/chapter-9.md`](../raw/chapter-9.md), [`ch-09-consistency-and-consensus.md`](../ch-09-consistency-and-consensus.md).

**Note:** Chapter still in progress — this covers **linearizability** only; will extend after consensus / total order sections.

---

Hi all!!

Still chewing through chapter 9, but here's my take on **linearizability** so far:

### One shared whiteboard

Imagine a single office whiteboard everyone reads. If Alice **finishes** writing "room booked" and Bob looks **after** she's done, Bob must not still see "free." That's the whole vibe: **one logical copy**, operations in some **serial order**, respecting **real time** when one op **ends before** another **starts**.

### Overlap is not "still writing"

If read and write **happen at the same time**, seeing old or new can both be OK — the serial order has wiggle room. The strict rule kicks in when the write **already completed** before the read **began**. I confused those at first.

### Someone has to enforce the line

Clients don't pick the order. A **shared mechanism** does: leader, quorum, consensus log, coordination service (ZooKeeper/etcd). Async replica + stale read = not linearizable even if the write "succeeded" on primary.

### Not the same as serializability (Ch. 7)

**Serializability** = whole **transactions** equivalent to some serial run. **Linearizability** = single-op **recency** with **wall-clock** ordering between non-overlapping ops. Different tools.

### Dev smell test

`POST` returns 201, immediate `GET` from a lagging replica → 404. User thinks the app is broken. That's a linearizability failure, not "eventual consistency being patient."

### One line to remember

> After a write **completes**, a read that **starts after** must see it — the **system** enforces one shared story, not the client.

(More after I finish total order broadcast / Raft…)

---

### Fault-tolerant consensus (why it’s different from 2PC)

**Core idea:** when some nodes fail, the system can still keep making progress by letting a *large enough* subset decide, while preventing different subsets from deciding *different* outcomes.

**Why 2PC can block:** if the **coordinator** dies at the wrong time (after participants are prepared, but before the final decision is communicated), participants can end up waiting indefinitely. The system buys **atomicity** at the cost of **liveness / availability** (locks can stay “in doubt”).

**Why consensus avoids “stuck forever”:** as long as enough nodes remain available, decisions can continue (**termination**). The safety intuition is that decision-making subsets must overlap, so you can’t have two conflicting decisions both “winning”.

**Operational symptoms to remember:**
- **2PC:** timeouts + contention because resources/locks remain held while waiting for the coordinator decision.
- **consensus:** you may see latency during failures/re-elections, but the system keeps moving once a sufficient subset is alive.
