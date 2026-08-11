# Riassunto capitolo 9: Consistency and Consensus (in corso — rilettura a mano)

> **Wiki (inglese, promosso):** [[source-ddia-ch-09]] — `../ch-09-consistency-and-consensus.md`  
> **Concept estratte:** linearizability (promossa); total order broadcast (raw IT, lug 2026 — concept EN a fine cap. 9); causal, Raft TBD → [[map-distributed-systems]]  
> **Provenienza:** lettura cap. 9 + sessioni chat; **rilettura a mano** 2026-08-05…07 (intro + Linearizability: def + when useful)

---

## Intro capitolo (appunti a mano — 2026-08-05)

> Prime ~5 pagine: perché il capitolo esiste, prima di entrare in linearizability.

### Idea chiave

Serve **tollerare i fault** per costruire sistemi distribuiti fault-tolerant. Il modo migliore: trovare **astrazioni general-purpose** (come le **transazioni**) con garanzie chiare — poi chiedere: *quali garanzie/astrazioni esistono per i sistemi distribuiti?*

### Consistency guarantees (da replica)

| Tema | Nota tua |
|------|----------|
| DB replicati | **Timing issues** — due DB nello stesso istante possono avere dati diversi (write non “allo stesso tempo”) |
| Eventual consistency | Convergenza sì, ma **non sappiamo quando** |
| Metafora | Il DB *sembra* una variabile read/write — in realtà è **molto più complicato** |

---

## Linearizability

> **Rilettura a mano:** 2026-08-05 (definizione) + **2026-08-07** (cosa rende linearizable + *when is it useful*). Note chat (lavagna, overlap) fuse sotto.

### Dalle note a mano — definizione (05–07/08)

**Linearizability** fa apparire il sistema come se:

1. ci fosse **una sola replica** / **una sola copia** del dato
2. tutte le operazioni fossero **atomiche**

**Esempio:** appena un client **completa con successo** una write → **tutti** i client che leggono dopo devono poter vedere il valore appena scritto.

### Cosa rende il sistema linearizable? (note 07/08)

Deve **apparire** come se esistesse **una sola copia**.

Rispetto a una **WRITE**:

| Timing della read | Effetto |
|-------------------|---------|
| **Before** la write | legge il valore **prima** |
| **After** la write | legge il valore **dopo** |
| **Concurrent** con la write | può sembrare che il valore “flippi” avanti/indietro |

**Vincolo:** deve esistere **un punto nel tempo** in cui il valore **flippa**; da lì in poi **tutte** le read successive devono restituire **quel** valore (non tornare al vecchio).

### Relying on linearizability — quando serve? (note 07/08)

#### (1) Locking and leader election

```text
Single-leader replication → only one leader
  → serve un LOCK
  → il lock deve essere LINEARIZABLE
  → all nodes must agree
  → CONSENSUS
```

#### (2) Constraints and uniqueness guarantees

Unicità tipica nei DB: **email**, **username**, **saldo conto** (hard uniqueness).  
Per questo serve linearizability: **lock + un solo valore aggiornato** (nessuna “due verità”).

#### (3) Cross-channel timing dependencies

Esempio libro: **resizer** + **image storage** su **due canali di comunicazione diversi** → senza linearizability (o equivalenti) possibili **race conditions**.

### Idea chiave (una frase)

**Dopo che una modifica è davvero finita, chi guarda i dati dopo non deve ancora vedere la versione vecchia** — una sola copia logica, ops atomiche, flip unico nel tempo.

### Analogia: la lavagna in ufficio

- Mario scrive *“giovedì — Alice”* e **finisce** (penna giù).
- Sara guarda la lavagna **dopo** → deve vedere la prenotazione, non *“libero”*.
- Se Mario e Sara scrivono **insieme** (overlap), alla fine il sistema mette le operazioni **in fila** e mostra **un solo** risultato.
- **Non** è il client che decide: serve un **meccanismo condiviso**.

→ [[concept-linearizability]]

### Regole operative (memoria)

| Situazione | Cosa deve succedere |
|------------|---------------------|
| Write **finita**, poi read | Read vede la write |
| Read **finita**, poi write | Read vede valore **prima** della write |
| Write e read **sovrapposte** | Read può vedere vecchio **o** nuovo (fino al flip) |
| Dopo il **flip** | **Tutte** le read successive vedono il nuovo |
| Due write **sovrapposte** | Un ordine W1→W2 o W2→W1; **un** stato finale |

**Errore comune:** confondere “read durante write” con “write non ancora finita” quando la write **è già completata**.

### Chi linearizza i dati

- **Non** il client.
- **Non** il singolo nodo senza quorum ([[concept-quorum-majority-truth]]).
- **Sì** il backend: leader, quorum, consensus, total order broadcast.

Collegamento **cap. 8:** rete/orologi inaffidabili → non basta NTP o replica async “sperando”.

### vs serializability (cap. 7)

| Linearizability | Serializability |
|-----------------|-----------------|
| Operazione singola / registro | **Transazione** intera |
| Ordine **tempo reale** | Ordine seriale equivalente |
| Lock, leader, unicità | DB transazioni |

### Esempio dev

`POST /users` → 201, `GET` da replica in ritardo → 404 = **non** linearizable. Primary / `readConcern: "majority"` (Mongo).

**Bookmark rilettura:** sezione Linearizability (definizione + when useful) ✓ a mano 07/08; prossimo nel libro: implementazione / costi / vs causal, poi TOB.

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

- 2PC practice ✓ · Fault-tolerant consensus (idea) ✓ in chat (2026-07-29)
- Membership & coordination ✓ core-idea (2026-07-30); simulator opzionale; libro: skim se serve dettaglio

---

## Fault-tolerant consensus (idea — chat 2026-07-29)

> **Stato:** compreso in chat (core-idea + error-simulator vs 2PC). Dettaglio Raft/epoch nel libro ancora da leggere/skimmare.

### Idea chiave

Se resta una **maggioranza**, il sistema può **continuare a decidere** (termination). Due sottoinsiemi che decidono devono **sovrapporsi** → niente decisioni divergenti. 2PC può **bloccarsi** senza coordinatore; consensus no (finché c’è majority).

---

## Membership and coordination services (ZooKeeper / etcd)

> **Stato:** compreso in chat (`learn-core-idea-first`, 2026-07-30). Concept EN opzionale a fine cap. 9.

### Idea chiave (una frase)

**Non reinventare consensus in casa per il meta-stato:** un servizio esterno (ZK / etcd / Consul) tiene la **verità di coordinamento** (leader, config, lock, membership); le app chiedono e rispettano.

### Analogia: il centralino ufficiale

Tanti team → un solo centralino per “chi è di turno”, “quale foglio regole vale”, “risorsa libera?”. Senza centralino: due verità e comportamenti diversi.

### A cosa serve (tipico)

| Uso | Perché |
|-----|--------|
| Leader election | Un solo primario attivo |
| Config / service discovery | Fonte ufficiale di impostazioni o indirizzi |
| Distributed locks / fencing | Chi può fare un’operazione esclusiva |
| Membership | Chi è nel gruppo / vivo |

### Collegamenti

- Sotto il cofano: **fault-tolerant consensus** (majority).
- **≠ 2PC**: 2PC = atomicità tra *tue* risorse di business; ZK/etcd = coordinamento / meta-stato.
- **Errore tipico:** usarli come database generale → no; solo stato piccolo e critico.

### Da completare col libro (skim ok)

- Dettagli API / watch / ephemeral nodes (ZK)
- Come Kafka / DB usano coordination services in pratica

---

## Sezioni da completare (resto cap. 9)

- [ ] Causal consistency
- [x] Total order broadcast — **compreso in chat** (lug 2026); promuovere concept EN a fine cap. 9
- [x] Atomic commit / **2PC + practice** — letto 2026-07-28; ripasso Mer 2026-07-29 ✓
- [~] **Fault-Tolerant Consensus** — idea ✓ chat; dettaglio Raft/Paxos nel libro TBD
- [x] **Membership and coordination** — core-idea ✓ 2026-07-30; simulator / skim libro opzionale
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
  → Fault-Tolerant Consensus (idea ✓; dettaglio libro TBD)
  → Membership & coordination (ZK/etcd) ✓ core-idea
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