# Riassunto capitolo 7: Transactions (completo)

> **Wiki (inglese, promosso):** [[source-ddia-ch-07]] — `../ch-07-transactions.md`  
> **Concept estratte:** snapshot isolation, lost update, write skew, phantom read, 2PL, index-range lock, SSI, deadlock → [[map-databases]]  
> **Provenienza:** rilettura + note esistenti nel repo (promozione mag 2026)

---

## Il concetto di transazione (ACID)

**Key idea:** le transazioni non sono magia — sono un’**astrazione** per raggruppare letture/scritture e gestire errori e concorrenza senza impazzire.

| Lettera | Significato | Nota |
|---------|-------------|------|
| **A**tomicity | tutto o niente (abort → scarta) | |
| **C**onsistency | stato valido → stato valido | soprattutto **responsabilità app** |
| **I**solation | transazioni concorrenti non si pestano i piedi | livello scelto conta |
| **D**urability | dopo commit, sopravvive al crash | WAL, replica, ecc. |

---

## Livelli di isolamento deboli (weak isolation)

**Key idea:** per performance la maggior parte dei DB usa isolamento **sotto** serializable.

### Read committed

- **No dirty reads** — vedi solo dati committati
- **No dirty writes** — non sovrascrivi dati di transazioni aperte

### Snapshot isolation (MVCC)

**Key idea:** ogni transazione legge da uno **snapshot coerente** all’inizio → risolve **read skew**.

- **MVCC:** più versioni dello stesso oggetto; lettori non bloccano scrittori
- **Non basta** per write skew o tutti i fantasmi da solo

→ [[concept-snapshot-isolation-mvcc]]

---

## Lost updates

**Key idea:** ciclo **leggi → modifica → scrivi**; due transazioni, un aggiornamento **perso**.

Soluzioni:

- **Scritture atomiche** (`$inc` MongoDB)
- **Lock esplicito** (`SELECT FOR UPDATE`)
- **Compare-and-set** (aggiorna solo se il valore è ancora quello letto)

→ [[concept-lost-update]]

---

## Anomalie avanzate: write skew e phantoms

### Write skew

Due transazioni leggono dati sovrapposti, scrivono **righe diverse**, insieme violano un invariante (es. due medici di turno che si tolgono entrambi).

Possibile con **snapshot isolation** — ogni transazione passa i controlli **locali**.

→ [[concept-write-skew]]

### Phantom read (fantasmi)

Una transazione **ripete una ricerca** e trova **righe diverse** perché un’altra ha inserito/cancellato righe che **matchano il predicato**.

Il lock su righe **esistenti** non basta.

→ [[concept-phantom-read]]

---

## Serializzabilità

**Key idea:** il risultato è come se le transazioni fossero eseguite **una alla volta** (in qualche ordine seriale).

| Metodo | Stile | Idea |
|--------|-------|------|
| **Actual serial execution** | pessimismo estremo | un thread, una transazione alla volta |
| **Two-Phase Locking (2PL)** | pessimistico | lock shared/exclusive; hold fino a commit |
| **SSI** | ottimistico | snapshot senza lock; abort al commit se conflitto |

### Two-Phase Locking (2PL)

**Key idea:** fase 1 acquisisci lock, fase 2 rilasci (strict 2PL: rilascio solo a fine transazione).

- Chi **legge** con shared lock; chi **scrive** con exclusive
- Se A ha letto e B vuole scrivere → B aspetta (e viceversa)
- **Performance:** meno concorrenza, rischio **deadlock**

→ [[concept-two-phase-locking]], [[concept-deadlock]]

### Predicate lock vs index-range lock

**Key idea:** il 2PL su righe singole **non ferma i fantasmi** quando la tabella era “vuota” nella ricerca — nessuna riga su cui mettere il lucchetto.

- **Predicate lock:** blocca tutto ciò che matcha una condizione WHERE (anche righe non ancora esistenti) — preciso ma **lento**
- **Index-range lock:** blocca un **intervallo nell’indice** (gap inclusi) — approssimazione **pragmatica** (next-key locking in InnoDB)

**Takeaway:** gli **indici** servono anche alla **sicurezza** delle transazioni, non solo alla velocità delle SELECT. Senza indice adatto → lock più grossolani (fino a tabella intera).

→ [[concept-index-range-lock]], [[concept-database-index]]

**Example:** prenotazione sala 14:00–15:00; tabella vuota nella ricerca; B inserisce alle 14:30 — senza range lock sul gap, fantasma e possibile doppia prenotazione.

### Serializable Snapshot Isolation (SSI)

**Key idea:** **ottimistico** — niente lucchetti durante l’esecuzione; il DB osserva letture obsolete e scritture che invalidano letture altrui; al **commit** abort se servirebbe serializzabilità vera.

- Combina velocità snapshot + rilevamento write skew / conflitti serializzazione
- **Svantaggio:** sotto carico, più **abort** → **retry** nel backend (Node.js)

→ [[concept-serializable-snapshot-isolation]]

**Tradeoff:**

| 2PL | SSI |
|-----|-----|
| pessimistico, blocca subito | ottimistico, aborta al commit |
| predicate/range lock pesanti | letture veloci |
| deadlock | retry applicativo |

---

## Applicazione MERN / MongoDB

**Key idea:** MongoDB multi-documento (v4.0+) usa **snapshot isolation** — non serializable completo di default.

- Preferire **operatori atomici** su singolo documento quando basta
- Transazioni complesse → gestire **retry** su conflitto

---

## Filo narrativo del capitolo (memoria)

```text
ACID → weak isolation (read committed, snapshot/MVCC)
  → lost update (read-modify-write)
  → write skew + phantoms (snapshot non basta)
  → serializability: serial execution | 2PL + index-range | SSI (abort + retry)
```

---

## Domande aperte

- Comportamento serializable/snapshot su MongoDB sharded in produzione?
- Quando `$inc` basta vs transazione multi-documento?

---

## Book club

Paste-ready copy: [`book-club/chapter-7.md`](../book-club/chapter-7.md)

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
