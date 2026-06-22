# Riassunto capitolo 8: The Trouble with Distributed Systems (completo)

> **Wiki (inglese, promosso):** [[source-ddia-ch-08]] — `../ch-08-trouble-with-distributed-systems.md`  
> **Concept estratte:** timeout, time-of-day, monotonic, NTP, leap second, LWW, logical clock, clock confidence interval, process pause, quorum, fencing token, Byzantine fault → [[map-distributed-systems]]  
> **Provenienza:** rilettura completa + appunti a mano (scan, ~34 pagine, mag 2026)

---

## Faults and partial failures

### Cloud computing and Supercomputing

**Key idea:** Pensare e assumere che **se qualcosa può andare storto, andrà storto** — in un sistema distribuito la complessità aumenta.

| PC singolo | Sistema distribuito |
|------------|---------------------|
| Binario: **fully functional** o **entirely broken** | **Partial failures** → comportamento **non deterministico** |
| | Qualcosa è **sempre** rotto (failed nodes, link, overload) |

**Cloud (approccio dominante):** online, low latency, commodity machines, IP/Ethernet, fault tolerance, geographic distribution, condivisione risorse.

**Supercomputing:** hardware e network specializzati — modello diverso.

**Takeaway:** un sistema distribuito deve essere **fault tolerant** assumendo che ci sarà sempre qualcosa di guasto; considerare un **ampio spettro di fault** possibili.

**Open question:** quanto del design “cloud” si applica a un singolo servizio monolitico su una VM?

---

## Unreliable networks

**Key idea:** architettura **shared-nothing** — i nodi comunicano **solo via network** (cheap, cloud, alta reliability come obiettivo).

**Async packet networks:** invii richiesta, aspetti risposta. Cose che possono succedere:

- richiesta persa
- richiesta in coda
- nodo remoto crashato
- nodo remoto temporaneamente non risponde
- risposta persa
- risposta in ritardo

**Problema centrale:** questi casi sono **indistinguibili** dal punto di vista del client → impossibile sapere *perché* non arriva risposta.

**Soluzione pratica usualmente:** **timeout** (euristica, non prova di morte).

---

### Network faults in practice

**Key idea:** i fault di rete sono **rari**, ma il software **deve** gestirli — altrimenti possono succedere cose arbitrariamente gravi.

#### Detecting faults

Feedback parziale possibile (OS reply, script crash, management interface, router ICMP unreachable) — **ma non puoi contare su nessuno**.

**Assumption:** devi assumere che **non arriverà alcuna risposta**.

#### Timeouts and unbounded delays

**Key idea:** il timeout è l’approccio pratico quando non sai se il peer è morto o lento — **non è prova di failure**.

| Long timeout | Short timeout |
|--------------|---------------|
| meno falsi positivi | failover più veloce |
| failover lento, attese lunghe | più rischio di dichiarare morto un nodo solo lento |

I delay sono **unbounded** — la maggior parte dei sistemi **non garantisce** tempi massimi di risposta.

#### Network congestion and queueing

**Key idea:** variabilità del delay dei pacchetti (come il traffico sulle strade).

Cause:

- **Network congestion** — più nodi verso stessa destinazione, code, drop e reinvio
- **CPU cores busy** — richiesta in coda sul destinatario
- **Ambienti virtualizzati** — OS messo in pausa dall’hypervisor
- **TCP flow control** — throttling del sender

L’app spesso **non vede** la perdita del pacchetto — vede solo il **delay risultante**.

**Tradeoff:** non puoi calcolare il timeout in modo teorico; lo scegli **sperimentalmente** (misura response time, conosci caratteristiche dell’app, monitora traffico/risorse). Nei cloud pubblici condividi risorse con altri tenant.

**Example:** nodo overload risponde dopo 60 s; con timeout 5 s il client dichiara failure e ritenta → carico amplificato.

---

### Synchronous versus asynchronous networks

**Key idea:** perché non rendere il network affidabile con delay fisso a livello hardware?

**Telefono (circuit switching):** quando la chiamata inizia, banda **fissa allocata** — nessun altro la usa finché la connessione è attiva. Delay prevedibile, ma **spreco** se il traffico è bursty.

**TCP/IP (packet switching):** usa banda disponibile; se idle non consuma banda. **Nessun circuito.** Code e delay variabili. Ottimizzato per **bursty traffic** — allocare banda fissa come un circuito è difficile (troppo poca → lento, troppa → network non può garantirla).

**Takeaway:** Ethernet/IP non sono come la telefonia; **non aspettarti delay bounded** su reti datacenter/internet.

---

## Unreliable clocks

**Key idea:** clock e tempo servono per **duration** (quanto ha impiegato?) e **points in time** (quando è successo? quando scade?).

**Examples:** response time server, tempo utente sul sito, data pubblicazione articolo, scadenza cache.

In distributed systems il tempo è **tricky**:

- comunicazione non istantanea
- delay variabili in network
- **each machine has its own clock**

### Monotonic versus Time-of-Day clocks

| Time-of-Day (wall clock) | Monotonic clock |
|--------------------------|-----------------|
| Data e ora corrente (epoch) | Misura **durate** |
| Sincronizzato via **NTP** tra macchine | Sempre avanti **su una macchina** |
| Può **saltare** avanti/indietro | Non confrontabile tra macchine diverse |
| Per timestamp umani, expiry | Per timeout locali, latency measurement |

### Clock synchronization and accuracy

**Key idea:** il TOD clock va settato con **NTP**, **ma:**

- quarzo drift (temperatura)
- nodo può rifiutare sync se troppo lontano da NTP
- firewall può bloccare NTP
- delay di rete limita accuratezza NTP
- server NTP misconfigured
- **leap seconds**

### Relying on synchronized clocks

**Key idea:** software **robusto** deve gestire orologi **sbagliati**.

- spesso **non te ne accorgi**
- all’inizio sembra tutto ok
- possibile **clock offset** tra nodi → **monitorare** l’offset ed espellere nodi con clock rotti prima di perdita silenziosa di dati

### Timestamps for ordering events

**Key idea:** **skew** tra nodi rende difficile ordinare eventi con timestamp wall-clock.

**Last Write Wins (LWW):**

- write possono **sparisce** (overwrite)
- non distingue write ravvicinate nel tempo
- timestamp **uguali** → tie-breaker arbitrario, può violare causalità

**Possible solution:** **logical clocks** (contatore incrementale / Lamport; **vector clocks** per concorrenza).

### Clock readings have a confidence interval

**Key idea:** difficile avere precisione elevata; accuratezza spesso **decine di ms**. Meglio pensare a un **range** con **confidence interval** — la maggior parte dei sistemi **non espone** questa incertezza.

### Synchronized clocks for global snapshots

**Key idea (Spanner / TrueTime):** per uno **snapshot globale** o un **commit** coerente su tutto il cluster, devi:

1. **esporre l’intervallo di incertezza** dell’orologio (`[earliest, latest]`)
2. **attendere** che l’intervallo sia passato (**commit wait**) prima di fissare il timestamp

Costoso (GPS, orologi atomici per datacenter) ma evita snapshot incoerenti quando usi wall-clock come “fotografia del mondo a T”.

→ concept [[concept-clock-confidence-interval]]

### Process pauses

**Key idea:** anche con clock corretti, il **processo può fermarsi** mentre il tempo reale scorre.

Scenario: DB **single leader**, lease dagli altri nodi per scrivere.

1. leader controlla lease (clock sync o monotonic — entrambi assumono poco tempo tra check e write)
2. **unexpected pause** (GC stop-the-world, VM suspended, context switch OS, I/O lento)
3. lease scade altrove → **nuovo leader**
4. thread riprende, crede ancora di essere leader → **write unsafe**

**Causes:** GC, virtualizzazione, scheduling OS, disk/network sync I/O, swap, SIGSTOP.

**Mitigations (parziali):** GC coordinato, rolling restart — non eliminano il modello.

→ [[concept-process-pause]]

---

## Response time guarantees

**Key idea:** sistemi **hard real-time** hanno deadline di risposta; per la maggior parte dei **server-side data systems** non è economico né appropriato garantirle.

Thread preempted → riprende dopo → stesso problema di pausa imprevedibile.

---

## Knowledge, truth, and lies

**Key idea:** in un distributed system:

- **no shared memory**
- **unreliable network** con delay variabili
- **no way** of knowing lo stato di un nodo che non risponde
- **process pauses**
- **unreliable clocks**

Non puoi fidarti del giudizio **locale** di un singolo nodo.

---

## The truth is defined by the majority

**Key idea:** un singolo nodo può:

- **ricevere** ma non **inviare** (fault asimmetrico / network partition)
- subire **long stop-the-world GC**

→ un distributed system **non può affidarsi esclusivamente a un singolo nodo**.

**Quorum:** la “verità” (leader, lock, decisione) è ciò che una **maggioranza** dei nodi concorda — non ciò che un nodo crede localmente.

→ [[concept-quorum-majority-truth]]

---

## The leader and the lock

**Key idea:** spesso serve **un solo** nodo leader, **una** transazione col lock, **un** utente che registra una risorsa.

**Pericolo:** un nodo **crede** di essere il chosen one → **non significa** che il quorum sia d’accordo.

→ serve qualcosa oltre al check locale del lease.

---

## Fencing tokens

**Key idea:** assicurare che un nodo con **falsa convinzione** di essere leader **non possa** disturbare il sistema.

**Mechanism:** **fencing token** — numero **monotonicamente crescente** emesso dal servizio di lock/election; **ogni write request** deve includerlo. Lo **storage** rifiuta write con token più basso del massimo già visto.

→ [[concept-fencing-token]]

---

## Byzantine faults

**Key idea:** in teoria un sistema può essere **Byzantine fault tolerant** — continua a operare correttamente anche se nodi:

- malfunzionano
- **non obbediscono** al protocollo
- sono **maliziosi**

**Tradeoff:** molto **complicato**, spesso richiede supporto **hardware**; raro in datacenter commodity.

**Web apps:** aspettarsi input **arbitrari e malevoli** dagli utenti (validazione) ≠ BFT completo tra nodi interni.

→ [[concept-byzantine-fault]]

---

## Filo narrativo del capitolo (memoria)

```text
Partial failure
  → rete inaffidabile, fault indistinguibili → timeout (euristica)
  → delay unbounded (congestion, CPU, VM, flow control)
  → circuit vs packet switching
  → orologi inaffidabili (TOD vs monotonic, NTP limits)
  → LWW pericoloso → logical clocks
  → timestamp = intervallo → global snapshot + commit wait (TrueTime)
  → process pause rompe lease
  → verità = quorum, non nodo locale
  → fencing token contro false leader
  → Byzantine = estremo teorico
```

---

## Domande aperte

- Timeout adattivi in produzione (percentili, Phi accrual)?
- Come etcd/ZooKeeper/Consul combinano lease, fencing e clock?
- Quando vale la pena vector clock vs solo Lamport in app reali?

---

## Book club

Paste-ready copy: [`book-club/chapter-8.md`](../book-club/chapter-8.md)

Hi all!!

This chapter is really hard to resume but..... :grin:

Here's my take on the key concepts in chapter 8:

### Partial failure is the default

On one machine, things either work or they don't. In a distributed system, **something is always broken** — failures are **partial** and **nondeterministic**. The cloud model (commodity hardware, shared networks) assumes fault tolerance; you can't fix that with better hardware alone.

### The network lies (silently)

Async packet networks: no response could mean lost request, queued request, dead node, slow node, lost response, or delayed response — **you can't tell which**. **Timeouts** are the practical tool, but a **heuristic**, not proof of failure.

- Long timeout → fewer false alarms, slower failover
- Short timeout → faster failover, more risk of killing a slow-but-alive node

Delays are **unbounded** (congestion, busy CPU, VM pauses, TCP flow control). Tune timeouts **experimentally**, not from a formula. Phone circuits = fixed bandwidth; TCP/IP = bursty and variable — better for datacenters, worse for assuming fixed latency.

### Clocks are worse than they look

- **Time-of-day:** "What time is it?" — NTP-synced, can **jump**, bad for ordering across nodes
- **Monotonic:** "How long did this take?" — forward on **one** machine only

**Last-write-wins** with timestamps silently **drops writes** when clocks skew. **Logical clocks** order by **causality**, not wall time. For **global snapshots**, a timestamp isn't a precise point — Spanner's **TrueTime** uses a **confidence interval** + **commit wait** (expensive but honest).

### Process pauses break your assumptions

Even perfect clocks fail if the **process stops** (GC, VM suspend, context switch, slow I/O). Leader checks lease → paused 15s → wakes up still "leader" while another was elected. **Monotonic clocks don't save you** — the code wasn't running.

### Truth is quorum-defined, not local

One node can be wrong (partition, long GC, stale lease). **The majority decides.** Stale leaders need **fencing tokens** on every write — storage rejects lower tokens.

**Byzantine faults** (malicious/lying nodes) = extreme case; full BFT is rare in typical datacenters. Validate **untrusted user input** on web apps — different problem, same chapter vibe.

### One line to remember

> Assume partial failure, unbounded delay, unreliable time, and unexpected pauses — design with **quorum**, **fencing**, and **causal ordering**, not local belief and `Date.now()`.
