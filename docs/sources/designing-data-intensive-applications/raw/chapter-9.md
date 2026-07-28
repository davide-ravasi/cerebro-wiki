# Riassunto capitolo 9: Consistency and Consensus (in corso — linearizability + TOB)

> **Wiki (inglese, promosso):** [[source-ddia-ch-09]] — `../ch-09-consistency-and-consensus.md`  
> **Concept estratte:** linearizability (promossa); total order broadcast (raw IT, lug 2026 — concept EN a fine cap. 9); causal, Raft TBD → [[map-distributed-systems]]  
> **Provenienza:** lettura cap. 9 in corso + sessioni `learn-core-idea-first` (linearizability mag 2026; total order broadcast lug 2026)

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

## Total order broadcast

> **Stato:** compreso in chat (`learn-core-idea-first`, lug 2026). Concept EN + sezione wiki **dopo** lettura Raft / fine cap. 9.

### Idea chiave (una frase)

**Tutti i nodi ricevono gli stessi messaggi nello stesso ordine** — una sola sequenza ufficiale condivisa, non solo “prima o poi tutti li hanno”.

### Analogia: il cancelliere in tribunale

- Il cancelliere annuncia gli eventi **in un ordine**; tutta la sala sente la stessa sequenza.
- Se tutti sentono “obiezione” **prima** di “respinta”, nessun nodo può avere l’ordine invertito.
- Due annunci **indipendenti** restano comunque in **un** ordine globale scelto una volta (spesso leader / consensus).

### Regole operative (memoria)

| Situazione | Cosa implica |
|------------|----------------|
| Nodo A vede M1 → M2 → M3 | Nodo B **deve** vedere M1 → M2 → M3 |
| Due `+1` sul contatore, stesso ordine ovunque | Stato finale **2** su tutti (state machine replication) |
| vs **causal order** | Causal: messaggi **indipendenti** possono arrivare in ordine diverso; **total order**: no |

### Collegamento a linearizability

- **Linearizability** = garanzia osservabile (“una lavagna”, recency).
- **Total order broadcast** = **meccanismo** per far applicare le operazioni **nella stessa sequenza** su tutte le repliche (log ordinato → stesso stato).
- Flusso tipico: write → entry nel log totalmente ordinato → repliche applicano → read coerente.

### Da completare con il libro

- Raft / Paxos come implementazione
- ZooKeeper / etcd
- Dettagli formalmente nel testo DDIA

---

## Atomic commit / Two-Phase Commit (2PC)

> **Stato:** quasi chiuso (2026-07-28). Mancano ~8 pagine del blocco; poi consensus/Raft.  
> Letto: intro 2PC → *Distributed transactions in practice* (exactly-once, XA, locks in doubt, recovering coordinator, limitations).

### Idea chiave (una frase)

**Atomic commit distribuito** = tutti i partecipanti **commit** insieme **oppure** tutti **abort** — nessuno a metà strada.

### Da nodo singolo a distribuito

| Nodo singolo | Distribuito |
|--------------|-------------|
| WAL + commit locale = atomico | Più nodi / DB: serve **accordo** tra tutti |
| Un solo decisore | Serve un **coordinatore** + protocollo |

### 2PC — le due fasi

1. **Prepare (voting):** coordinatore chiede ai partecipanti “puoi commitare?” → ogni nodo risponde **yes** o **no** (e tiene risorse/lock in attesa).
2. **Commit / Abort:** se **tutti** yes → coordinatore manda **commit**; altrimenti **abort**. I partecipanti eseguono la decisione.

### “A system of promises”

- In prepare ogni partecipante **promette**: “se decidi commit, io commito.”
- Il coordinatore **decide una volta** dopo aver raccolto i voti.
- I partecipanti **non** possono cambiare idea dopo il yes in prepare.

### Coordinator failure (base)

- Coordinatore muore **prima** della decisione → partecipanti **bloccati** in attesa (incertezza).
- Coordinatore muore **dopo** commit scritto ma prima che tutti lo sappiano → serve recovery / log del coordinatore.
- Problema noto: 2PC **non** è fault-tolerant al 100% → motivazione per **consensus** (Raft) dopo nel capitolo.

### Distributed transactions in practice (2026-07-28)

| Tema | Idea in una riga |
|------|------------------|
| **Exactly-once message processing** | Atomicità messaggio + side effect (es. DB): o entrambi sì o entrambi no — evita “processato due volte” / “perso”. |
| **XA transactions** | Standard 2PC tra risorse eterogenee (DB, queue, …) via coordinatore XA / resource managers. |
| **Holding locks while in doubt** | Dopo *prepare* (yes), i lock restano tenuti finché arriva commit/abort — se il coordinatore è in dubbio, **contesa e latenza**. |
| **Recovering from coordinator failure** | Nuovo coordinatore (o recovery) legge il log: se la decisione era già stata scritta → ripeti commit/abort; altrimenti spesso **abort** o resta bloccato. |
| **Limitations of distributed transactions** | Costo, disponibilità (blocco in doubt), ops complesse, coupling — non “gratis”; molti sistemi preferiscono alternative (sagas, outbox, consensus ristretto). |

> **Attenzione:** 2PC (commit distribuito) ≠ **2PL** (two-phase **locking**, cap. 7 — serializzabilità).

### Bookmark lettura

- **Prossime macro:** Fault-Tolerant Consensus → Membership and coordination services
- 2PC blocco practice chiuso (2026-07-28)

---

## Sezioni da completare (resto cap. 9)

- [ ] Causal consistency
- [x] Total order broadcast — **compreso in chat** (lug 2026); promuovere concept EN a fine cap. 9
- [x] Atomic commit / **2PC + practice** — letto 2026-07-28 (exactly-once, XA, locks in doubt, recovery, limits); ripasso Mer/Gio
- [ ] **Fault-Tolerant Consensus** (Raft/Paxos/Zab, TOB, epoch/quorum) — **prossima priorità**
- [ ] **Membership and coordination services** (ZooKeeper / etcd) — dopo consensus; ok più leggero
- [ ] CAP / tradeoff con disponibilità

> **Attenzione:** 2PC (commit) ≠ 2PL (locking, cap. 7).

---

## Filo narrativo (parziale)

```text
Cap. 7 serializability (transazioni)
  → Cap. 8 tempo/rete/quorum inaffidabili
  → Cap. 9 linearizability (registro singolo, tempo reale)
  → total order broadcast (stessa sequenza di eventi su tutti i nodi)
  → atomic commit / 2PC + practice ✓
  → Fault-Tolerant Consensus  ← prossimo
  → Membership & coordination (ZooKeeper / etcd)
```

---

## Domande aperte

- ~~Come total order broadcast si collega a linearizability formalmente?~~ → vedi sezione TOB sopra (meccanismo vs garanzia); approfondire con Raft
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