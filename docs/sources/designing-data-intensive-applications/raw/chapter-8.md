# Riassunto capitolo 8: The trouble with Distributed Systems 8/38

## Faults and partial failures

### Cloud computing and Supercomputing

## Unreliable networks

### Network faults in practice

### Detecting faults

### Timeout and UNbounded Delays

**key idea** :Il timeout è il solo modo per detect dei faults?
Long timeout vs short timeout - difficile quantificare o prevedere se c'è veramente faults di un node
I delay dei sistemi sono **Unbounded delays** (no upper limits to packet to arrive )

**example**: long delay - long time for the node to be declared dead - maybe is slow due to an overload
**example**: short delay - detects faults faster - risk to incorrectly declare node dead, maybe only temporary slowdown

**Question**: none
