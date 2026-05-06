# Riassunto capitolo 8: The trouble with Distributed Systems 8/38

## Faults and partial failures

### Cloud computing and Supercomputing

## Unreliable networks

### Network faults in practice

### Detecting faults

### Timeout and UNbounded Delays

**key idea** :
Il timeout è il solo modo per detect dei faults?
Timeouts are a heuristic, not proof of failure.
Un timeout suggerisce un possibile guasto, ma non lo dimostra con certezza.
**Long timeout** --> fewer false positives, slower failover
**Short timeout** --> faster failover, more false positives
I delay dei sistemi sono **Unbounded delays** (no upper limits to packet to arrive)

**example**: long delay - long time for the node to be declared dead - maybe is slow due to an overload
**example**: short delay - detects faults faster - risk to incorrectly declare node dead, maybe only temporary slowdown

**Question**: How do systems choose timeouts in practice?

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
