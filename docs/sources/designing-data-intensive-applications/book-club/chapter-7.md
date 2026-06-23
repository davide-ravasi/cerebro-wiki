# DDIA Chapter 7 — Book club (Discord)

Paste-ready. Full raw + wiki: [`raw/chapter-7.md`](../raw/chapter-7.md), [`ch-07-transactions.md`](../ch-07-transactions.md).

---

Here's my take on the key concepts in chapter 7:

### Transactions are not magic

They bundle reads and writes so your app can **abort on error** and reason about concurrency. **ACID** names the goals — but **consistency** is mostly **your** job (valid application states).

### Weak isolation is the default

**Read committed:** no dirty reads/writes.

**Snapshot isolation (MVCC):** each transaction sees a **frozen snapshot** — fixes read skew, readers don't block writers. Fast and common — but **not full serializability**.

### Lost updates still happen

Classic **read–modify–write** race (two counters, two profile saves). Fix with **atomic ops** (`$inc`), **row locks**, or **compare-and-set** — don't assume "I'm in a transaction" alone fixes it.

### Write skew and phantoms

**Write skew:** two txs read the same facts, update **different rows**, together break a rule (on-call doctors). Snapshot isolation can still allow this.

**Phantoms:** your **search** returns different rows because someone **inserted** a match. Row locks on existing data aren't enough.

### Serializable = "as if one at a time"

Three paths:

1. **Serial execution** — one thread; simple, limited scale
2. **2PL (pessimistic)** — locks until commit; blocks; **deadlocks**; use **index-range locks** for phantoms
3. **SSI (optimistic)** — run free on snapshots; **abort at commit** if conflicts; app must **retry**

### Indexes aren't just for speed

**Index-range / gap locks** approximate predicate locks — they block inserts into a searched range. No index → coarser locks (sometimes whole table).

### MERN takeaway

MongoDB multi-doc transactions ≈ **snapshot isolation**. Prefer atomic single-doc updates when you can; design **retries** when you need stronger isolation elsewhere.

### One line to remember

> **Snapshot isolation is fast but not serializable** — know lost updates, write skew, and phantoms before you pick an isolation level.
