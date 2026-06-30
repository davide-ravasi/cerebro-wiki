# Riassunto capitolo 9: Consistency and Consensus (in corso — linearizability)

> **Wiki (inglese, promosso):** [[source-ddia-ch-09]] — `../ch-09-consistency-and-consensus.md`  
> **Concept estratte:** linearizability (altre TBD: causal consistency, total order, Raft…) → [[map-distributed-systems]]  
> **Provenienza:** lettura cap. 9 in corso + sessione `learn-core-idea-first` (lavagna, mag 2026)

---

## Linearizability

### Idea chiave (una frase)

**Dopo che una modifica è davvero finita, chi guarda i dati dopo non deve ancora vedere la versione vecchia** — come se esistesse **una sola copia** del dato e tutti leggessero lo stesso ordine di eventi.

### Analogia: la lavagna in ufficio

- Mario scrive *“giovedì — Alice”* e **finisce** (penna giù).
- Sara guarda la lavagna **dopo** → deve vedere la prenotazione, non *“libero”*.
- Se Mario e Sara scrivono **insieme** (overlap), alla fine il sistema deve mettere le operazioni **in fila** e mostrare **un solo** risultato a tutti.
- **Non** è il client che decide: serve un **meccanismo condiviso** (protocollo del DB / servizio di coordinamento).

→ [[concept-linearizability]]

### Regole operative (memoria)

| Situazione | Cosa deve succedere |
|------------|---------------------|
| Write **finita**, poi read | Read vede la write |
| Read **finita**, poi write | Read vede valore **prima** della write |
| Write e read **sovrapposte** | Read può vedere vecchio **o** nuovo (ordine seriale flessibile) |
| Due write **sovrapposte** | Un ordine W1→W2 o W2→W1; **un** stato finale |

**Errore comune:** confondere “read durante write” con “write non ancora finita” quando il testo dice che la write **è già completata**.

### Chi linearizza i dati

- **Non** il client.
- **Non** il singolo nodo senza quorum ([[concept-quorum-majority-truth]]).
- **Sì** il backend: leader, quorum, consensus, total order broadcast (da leggere nel resto del cap.).

Collegamento **cap. 8:** rete/orologi inaffidabili → non basta NTP o replica async “sperando”.

### vs serializability (cap. 7)

| Linearizability | Serializability |
|-----------------|-----------------|
| Operazione singola / registro | **Transazione** intera |
| Ordine **tempo reale** | Ordine seriale equivalente |
| Lock, leader, unicità | DB transazioni |

### Esempio dev

`POST /users` → 201, `GET` da replica in ritardo → 404 = **non** linearizable. Primary / `readConcern: "majority"` (Mongo).

---

## Sezioni da completare (resto cap. 9)

- [ ] Causal consistency
- [ ] Total order broadcast
- [ ] Consensus (Raft / Paxos)
- [ ] ZooKeeper / etcd
- [ ] CAP / tradeoff con disponibilità

---

## Filo narrativo (parziale)

```text
Cap. 7 serializability (transazioni)
  → Cap. 8 tempo/rete/quorum inaffidabili
  → Cap. 9 linearizability (registro singolo, tempo reale)
  → (da leggere) come costruire ordine condiviso: broadcast + consensus
```

---

## Domande aperte

- Come total order broadcast si collega a linearizability formalmente?
- MongoDB: quali read sono linearizable su sharded cluster?

---

## Book club

Paste-ready copy: [`book-club/chapter-9.md`](../book-club/chapter-9.md)

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