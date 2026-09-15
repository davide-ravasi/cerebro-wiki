# Riassunto capitolo 9: Consistency and Consensus (rilettura a mano **chiusa** 15/09)

> **Wiki (inglese, promosso):** [[source-ddia-ch-09]] — `../ch-09-consistency-and-consensus.md`  
> **Concept estratte:** linearizability (promossa); total order broadcast (raw IT, lug 2026 — concept EN a fine cap. 9); causal, Raft TBD → [[map-distributed-systems]]  
> **Provenienza:** lettura cap. 9 + sessioni chat; **rilettura a mano** 2026-08-05…11 + **2026-09-08** (cost→TOB + intro dist. tx) + **2026-09-15** (2PC completo: promises, coordinator failure, practice, locks in doubt — scan ~p.32/52)

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

**Bookmark rilettura:** sezione Linearizability (definizione + when useful) ✓ a mano 07/08.

---

## Cost of linearizability (note a mano — 11/08 + rilettura 08/09, p. 18)

### Se c'è un'interruzione di rete

| Setup | Cosa succede |
|-------|---------------|
| **Single leader**, client connesso alla parte **sbagliata** | **non può scrivere** · **niente read linearizable** |
| **Multi leader** | continua a operare (entrambi i lati) |

### CAP theorem (promemoria)

| Scelta | Conseguenza |
|--------|-------------|
| App **richiede** linearizability | alcune repliche **non possono processare richieste** mentre sono disconnesse |
| App **non richiede** linearizability | l'app resta **disponibile** anche davanti a problemi di rete |

**Perché si rinuncia alla linearizability:** per **performance**, non per fault tolerance.

→ trade-off: **linearizability vs performance/disponibilità**, non "linearizability vs fault-tolerance".

---

## Ordering guarantees (note a mano — 11/08 + rilettura 08/09, p. 18)

**Linearizability** implica un **ordine ben definito** (solo una copia dei dati → un solo ordine di eventi possibile).

**L'ordinamento è un tema ricorrente** nel capitolo, non solo della linearizability:

- **Single-leader replication** → ordine delle **write** (definito dal leader)
- **Serializability** → un qualche **ordine sequenziale** (delle transazioni)
- **Timestamp and clocks** → altro tentativo di dare un ordine (cap. 8, inaffidabile)

---

## Ordering and causality (note a mano — 11/08 + rilettura 08/09)

**L'ordinamento preserva la causalità.** Esempi già visti nel libro dove serve rispettare l'ordine causa→effetto:

- **Causal dependency** — domanda / risposta (la risposta non può "precedere" la domanda)
- **Replication** — i **ritardi di rete** possono far arrivare gli eventi fuori ordine
- **Happens-before** — il concetto chiave per definire la causalità
- **Consistent snapshot** — deve essere **consistente con la causalità**
- **Write skew** all'interno delle transazioni (cap. 7)

**Causality impone un ordinamento sugli eventi.**

### Causal order ≠ total order

| Tipo di ordine | Definizione |
|-----------------|-------------|
| **Total order** | permette di **confrontare** una **qualsiasi coppia** di elementi |
| **Partial order** | alcuni elementi sono **incomparabili** |

| Garanzia | Tipo di ordine |
|----------|-----------------|
| **Linearizability** | **total order** |
| **Causality** | **partial order** (esistono operazioni **concorrenti**) |

→ **Conseguenza:** in un datastore **linearizable** non esiste il concetto di operazioni concorrenti — c'è sempre un ordine totale.

### Linearizability è più forte della causal consistency

- **Linearizable ⇒ causale** (implica il rispetto della causalità)
- **Ma** linearizability può **danneggiare le performance** (vedi sezione costo sopra)

### Catturare le dipendenze causali

Per rispettare la causalità serve **sapere quale operazione è avvenuta prima** di un'altra (tracciare le dipendenze) — non basta un orologio/timestamp qualsiasi (cap. 8).

---

## Sequence number ordering (note a mano — 11/08 + rilettura 08/09, p. 23)

> Tenere traccia di **tutte** le dipendenze causali può essere **impraticabile**.

**Modo migliore:** usare **sequence number** o **timestamp** per ordinare gli eventi.

- Forniscono un **ordine totale**
- **E** sono **consistenti con la causalità**

### Non-causal sequence number generators

Se **non c'è un singolo leader**, è **meno chiaro** come generare i sequence number.

**Vari metodi** (non basati su un leader):

- ogni **nodo** ha la sua **propria sequenza**
- **timestamp** attaccato a ogni operazione
- **preallocare** blocchi di sequence number (per nodo)

**MA** → questi metodi **non sono consistenti con la causalità**: non catturano l'**ordinamento delle operazioni tra nodi diversi**.

---

## Lamport timestamps (note a mano — 08/09)

> Generano numeri **consistenti con la causalità** senza tracciare a mano ogni dipendenza.

**Struttura:** ⟨**node id**⟩ + ⟨**counter**⟩

**Regola tipica:** se un nodo **riceve** un messaggio con counter più alto → prende il **max**, poi aggiorna il **proprio** counter.

### Timestamp ordering non basta

I Lamport timestamps **non sono sufficienti** per molti problemi tipici dei sistemi distribuiti.

**Esempio:** due utenti tentano **in concorrenza** di creare un account con lo **stesso username**.  
Per decidere **accept vs fail** non basta il timestamp locale: serve in pratica **confrontarsi con gli altri nodi** (cosa sta succedendo altrove).

**IMP:** un **ordine totale** “ufficiale” emerge solo **dopo** aver **raccolto** le operazioni dagli altri nodi — non dal solo clock logico locale.

→ Da qui il passo successivo: **total order broadcast** (una sequenza unica su cui tutti concordano).

**Bookmark rilettura:** cost → ordering/causality → sequence numbers → **Lamport ✓ 08/09**; TOB a mano sotto.

---

## Total order broadcast

> **Stato:** compreso in chat (`learn-core-idea-first`, lug 2026) + **note a mano 08/09**. Concept EN + sezione wiki **dopo** lettura Raft / fine cap. 9.

### Idea chiave (una frase)

**Tutti i nodi ricevono gli stessi messaggi nello stesso ordine** — una sola sequenza ufficiale condivisa, non solo “prima o poi tutti li hanno”.

In un sistema distribuito è **difficile** far sì che tutti i nodi **concordino** sullo stesso ordine delle operazioni.

### Come si ottiene spesso (intuizione libro)

```text
Total order broadcast
  → spesso via single-leader replication
  → il leader sequenzia (un solo “CPU” / un solo punto che decide l’ordine)
```

### Due proprietà obbligatorie (note 08/09)

1. **Nessun messaggio perso** (reliable delivery)
2. **Messaggi consegnati a ogni nodo nello stesso ordine**

### Usare TOB (note 08/09)

- Per implementare **transazioni serializzabili** (stesso ordine di ops → stesso stato)
- L’ordine è **fissato al momento della delivery** del messaggio
- TOB è **asincrono** (non aspetti che “tutto il mondo” abbia già applicato in sync wall-clock)
- **Metafora log:** è un modo per creare un **log** — tutti i nodi leggono il log e vedono la **stessa sequenza** di messaggi

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
- Collegamento a Lamport: i timestamp logici **ordinano in modo causale**, ma **non** decidono da soli “chi vince” su conflitti globali (es. username unico) senza raccogliere le ops / avere un ordine totale condiviso (TOB/consensus).

### Da completare con il libro

- Raft / Paxos come implementazione
- ZooKeeper / etcd
- Dettagli formalmente nel testo DDIA

**Bookmark rilettura:** TOB a mano ✓ 08/09; prossimo nel libro: resto **distributed transactions / 2PC** (già avviato sotto) → consensus fault-tolerant in dettaglio.

---

## Atomic commit / Two-Phase Commit (2PC)

> **Stato:** chiuso a mano **15/09** (scan) + chat 2026-07-28 + intro 08/09.  
> Copre: perché serve accordo → 2PC → promises → coordinator failure → practice (internal vs heterogeneous) → locks in doubt / recovery.

### Consensus — perché compare qui (note 08/09)

**Consensus** = far sì che **vari nodi concordino su qualcosa**.

**Quando serve:**
- **Leader election**
- **Atomic commit** (tutti i nodi concordano sull’**esito** della transazione)

### Idea chiave (una frase)

**Atomic commit distribuito** = tutti i partecipanti **commit** insieme **oppure** tutti **abort** — nessuno a metà strada.

**Atomicità della transazione** → esito solo **COMMIT** o **ABORT**.  
**Previene:** risultati **a metà** · stato **semi-aggiornato**.

### Da nodo singolo a distribuito

| Nodo singolo | Distribuito |
|--------------|-------------|
| WAL + commit locale = atomico | Più nodi / DB: serve **accordo** tra tutti |
| Un solo decisore | Serve un **coordinatore** + protocollo |

**Ordine tipico su un singolo nodo DB** (note 08/09):

1. Rende le write della transazione **durevoli**
2. **Append** di un **commit record** al log
3. Così può **recuperare** da lì in caso di **crash**

**Se ci sono più nodi:** **non basta** mandare una “commit request” a tutti, né far commitare **in indipendenza** — i nodi diverrebbero inconsistenti. Serve un protocollo (→ **2PC**).

**Perché un commit “semplice” fallisce su alcuni nodi:** vincoli rifiutati · write perse in rete · crash. Un nodo che ha committato **non può tornare indietro**: il risultato è già **visibile**. Quindi: **commit una sola volta, irrevocabile**.

### 2PC — le due fasi

Algoritmo per atomic commit **su più nodi**: **tutti commit** **oppure** **tutti abort**. Serve **coordinatore** + **partecipanti**.

1. **Prepare (voting):** l’app è ready → coordinatore chiede “puoi commitare?” → **yes** o **no**.
2. **Commit / Abort:** **tutti** yes → commit; **un** no → abort.

### “A system of promises” (flusso, note 15/09)

1. App chiede un **transaction ID** al coordinatore
2. App apre una tx **single-node** su ogni partecipante, con quell’ID
3. Ready to commit → **prepare** ai partecipanti
4. I partecipanti verificano di **poter** commitare
5. Il coordinatore raccoglie le risposte e prende la **decisione definitiva** (commit/abort)
6. La decisione è **scritta su disco**
7. Poi **invia** commit o abort a tutti

**Due punti cruciali:**

| Chi | Cosa | Effetto |
|-----|------|---------|
| Partecipante vota **yes** | **Promette** che commiterà se il coordinatore decide commit | Non può cambiare idea |
| Coordinatore **decide** | Decisione **irrevocabile** (dopo il log) | I partecipanti eseguono quella e basta |

### Coordinator failure (note 15/09 — due casi)

| Quando muore il coordinatore | Cosa possono fare i partecipanti |
|------------------------------|----------------------------------|
| **Prima** di mandare la prepare / senza aver preso yes | Possono **abortare in sicurezza** (nessuno ha promesso) |
| **Dopo** che un partecipante ha detto **yes** | **Devono aspettare** — stato **in doubt / uncertain** finché il coordinatore (o un recovery) non torna |

Dopo yes i **lock restano**: nessuna altra tx può modificare quelle righe, finché non arriva commit o abort. Questo è il costo operativo (contesa, latenza), non un dettaglio.

**Recovery:** coordinatore crashato e **riavviato** → rilegge il **log** e risolve le tx in doubt.  
**Orphan in-doubt:** a volte **non** si risolve da sola → **un admin decide a mano** (tanto lavoro).

2PC **non** è fault-tolerant al 100% (può restare bloccato) → motivazione per **consensus** (Raft) dopo nel capitolo.

### Distributed transactions in practice (chat 07/28 + note 15/09)

**Reputazione mista:** problemi operativi · uccidono la performance · promettono più di quanto consegnano.

| Tipo | Idea |
|------|------|
| **DB-internal** | Stesso prodotto/famiglia: protocollo a scelta, ottimizzazioni specifiche della tecnologia. |
| **Heterogeneous** (es. messaggio + write DB) | Commit atomico tra sistemi **diversi**. Possibile solo se **tutti usano lo stesso protocollo** (in pratica: XA / resource managers). |

| Tema | Idea in una riga |
|------|------------------|
| **Exactly-once message processing** | Atomicità messaggio + side effect (es. DB): o entrambi sì o entrambi no. |
| **XA** | Standard 2PC tra risorse eterogenee. |
| **Limitations** | Costo, disponibilità (blocco in doubt), coupling — alternative: sagas, outbox, consensus *ristretto* (meta-stato, non tutte le tx business). |

> **Attenzione:** 2PC (commit distribuito) ≠ **2PL** (two-phase **locking**, cap. 7 — serializzabilità).

### Bookmark lettura

- 2PC + practice **a mano ✓ 15/09** · Fault-tolerant consensus: idea ✓ 29/07 · meccanismo epoch ✓ 15/09
- Membership & coordination ✓ core-idea (2026-07-30); 5 gap tecnici in backlog ripasso

---

## Fault-tolerant consensus (idea 29/07 + meccanismo 15/09, core-idea)

> **Stato:** idea vs 2PC ✓ chat 29/07. **Meccanismo (epoch / majority / fencing)** ✓ core-idea 15/09 **senza** rilettura Raft pagina per pagina. Dettaglio algoritmo (match log, election timeout) resta opzionale.

### Idea chiave

Se resta una **maggioranza**, il sistema può **continuare a decidere** (termination). Due sottoinsiemi che decidono devono **sovrapporsi** → niente decisioni divergenti. 2PC può **bloccarsi** senza coordinatore; consensus no (finché c’è majority).

### Meccanismo (analogia → termini)

| Ristorante (15/09) | Nel sistema |
|--------------------|-------------|
| Foglio turno **numerato** (17, poi 18) | **Epoch / term / generation / ballot** |
| Più della metà dello staff deve firmare lo stesso capo | **Quorum di maggioranza** (due quorum si **intersecano**) |
| Capo 17 che rientra: il forno guarda il numero e ignora | **Fencing**: comandi con epoch **stale** rifiutati |
| Non aspetti il 17 per sempre: eleggi il 18 | **Leader election** su un termine nuovo |

**Vs 2PC:** dopo un *yes* sei **in doubt** finché *quel* coordinatore (o un umano) torna. Qui, se il capo sparisce, una majority **sceglie un capo nuovo** con numero **più alto**; il vecchio non “sovrascrive” perché i follower (e il log) accettano solo l’epoch corrente.

**Log / TOB:** il capo attuale propone voci in **un** ordine; la majority le **accetta**. Tutti i sopravvissuti vedono la **stessa sequenza** → è il modo usuale di **implementare total order broadcast** (e poi linearizability su un registro). Non è un DB per tutte le tx business: è piccolo, critico (meta-stato, ordine delle ops).

**Cosa non è:** 3PC non “sistema” il blocco di 2PC in rete asincrona. FLP: in async puro con anche un crash, il consensus deterministico **non** è sempre possibile — in pratica si usano **timeout** (ipotesi di rete “abbastanza” sincrona).

### Bookmark

- Core-idea meccanismo ✓ 15/09 (3 gate: nuovo numero; majority/overlap; ignore stale)
- Libro Raft passo-passo: solo se vuoi i dettagli (heartbeat, log matching)

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

- [x] Causal consistency / ordering & causality — note a mano ✓ (11/08 + 08/09)
- [x] Lamport timestamps — note a mano ✓ 08/09
- [x] Total order broadcast — chat lug 2026 + **note a mano 08/09**; promuovere concept EN a fine cap. 9
- [x] Atomic commit / **2PC + practice** — a mano ✓ **15/09** (promises, in-doubt, internal vs heterogeneous)
- [x] **Fault-Tolerant Consensus** — idea ✓ 29/07 · **meccanismo epoch/majority/fencing ✓ 15/09** (core-idea; Raft carta opzionale)
- [x] **Membership and coordination** — core-idea ✓ 2026-07-30; simulator / skim libro opzionale
- [x] CAP / tradeoff con disponibilità — note a mano ✓ (cost of lin., 11/08 + 08/09)

> **Attenzione:** 2PC (commit) ≠ 2PL (locking, cap. 7).

---

## Filo narrativo (parziale)

```text
Cap. 7 serializability (transazioni)
  → Cap. 8 tempo/rete/quorum inaffidabili
  → Cap. 9 linearizability (registro singolo, tempo reale)
  → cost of lin. / CAP (perf, non solo fault-tolerance)
  → ordering & causality (total vs partial)
  → sequence numbers → Lamport (causale ma non basta)
  → total order broadcast (log unico; props: no loss + same order)
  → atomic commit / 2PC (+ practice ✓)
  → Fault-Tolerant Consensus (idea ✓ 29/07 · meccanismo epoch ✓ 15/09)
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