# Riassunto capitolo 8: The trouble with Distributed Systems 12/38

## Faults and partial failures

### Cloud computing and Supercomputing

## Unreliable networks

### Network faults in practice

### Detecting faults

### Timeout and Unbounded Delays

**key idea** :
Il timeout è il solo modo per detect dei faults?
Timeouts are a heuristic, not proof of failure.
Un timeout suggerisce un possibile guasto, ma non lo dimostra con certezza.
**Long timeout** --> fewer false positives, slower failover
**Short timeout** --> faster failover, more false positives
I delay dei sistemi sono **Unbounded delays** (no upper limits to packet to arrive)

**example**:
long delay - long time for the node to be declared dead - maybe is slow due to an overload
**example**:
short delay - detects faults faster - risk to incorrectly declare node dead, maybe only temporary slowdown

**Question**: How do systems choose timeouts in practice?

#### Network congestion and queueing

**Key idea**
Il ritardo nell'arrivo dei pacchetti è spesso dovuto al traffico nel network (così come nei viaggi il tempo dipende dal traffico)
Il delay può essere dovuto a:

- **Network congestion** (molte richieste inserite in una coda. se no spazio il pacchetto viene abbandonato e bisogna reinviarlo)
- **CPU cores** del destinatario molto occupate (richiesta messa in coda)
- negli **ambienti virtualizzati** un sistema operativo può essere messo temporanemanete in pausa
- **flow control** - un node può limitare il suo rate per evitare overload

Tutto questo contrbuisce a aumentare la variabilità del ritardo.

**Tradeoff**
Il delay **non può essere quantificato con regolarità**, si può solo settare in modo sperimentale con analisi sull'utlizzo del network. Si può fare anche in modo variabile con monitoraggio delle risorse e del traffico.

**Example**
Nei cloud pubblici bisogna condividere le risorse con molti utenti.

**Open question** none

### Synchronous Versus Asynchronous Networks

**Key idea**
Perchè non possiamo semplicemente risolvere il problema a un livello hardware e rendere il network affidabile con delay fisso e senza perdere pacchetti?

**Esempio**: Il telefono è molto efficiente e ha una banda fissa allocata per ogni conversazione e riesce a mantenere un'elevata qualità della conversazione senza perdita di informazioni

#### Can we not simply make network delays predictable?

**Key idea**

**Tradeoff**
**Example**
**Open question**

---

#### Note

**Key idea**
**Tradeoff**
**Example**
**Open question**

Idea chiave
(1 frase: cosa devo ricordare davvero)

Trade-off
(vantaggio vs costo / rischio)

Esempio
(caso concreto, anche breve)

Domanda aperta
(cosa non mi è ancora chiarissimo)
