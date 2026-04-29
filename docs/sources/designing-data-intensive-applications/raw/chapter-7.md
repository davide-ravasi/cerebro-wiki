## Two-Phase Locking (2PL)

Two-phase locking is similar, but makes the lock requirements much stronger. Sev‐
eral transactions are allowed to concurrently read the same object as long as nobody
is writing to it. But as soon as anyone wants to write (modify or delete) an object,
exclusive access is required:
• If transaction A has read an object and transaction B wants to write to that
object, B must wait until A commits or aborts before it can continue. (This
ensures that B can’t change the object unexpectedly behind A’s back.)
• If transaction A has written an object and transaction B wants to read that object,
B must wait until A commits or aborts before it can continue. (Reading an old
version of the object, like in Figure 7-1, is not acceptable under 2PL)

### Implementation of two-phase locking

If a transaction wants to read an object, it must first acquire the lock in shared
mode.

If a transaction wants to write to an object, it must first acquire the lock in exclu‐
sive mode.

If a transaction first reads and then writes an object, it may upgrade its shared
lock to an exclusive lock.

After a transaction has acquired the lock, it must continue to hold the lock until
the end of the transaction (commit or abort). This is where the name “two-
phase” comes from: the first phase (while the transaction is executing) is when
the locks are acquired, and the second phase (at the end of the transaction) is
when all the locks are released

What application use it?

### Performance of two-phase locking

The big downside of two-phase locking, and the reason why it hasn’t been used by
everybody since the 1970s, is performance: transaction throughput and response
times of queries are significantly worse under two-phase locking than under weak
isolation.

This is partly due to the overhead of acquiring and releasing all those locks, but more
importantly due to reduced concurrency. By design, if two concurrent transactions
try to do anything that may in any way result in a race condition, one has to wait for
the other to complete.

### Predicate locks

Questa è l'evoluzione "estrema" del concetto di Two-Phase Locking (2PL) che serve a risolvere l'unico problema che il 2PL normale non riesce a gestire: i Fantasmi (Phantoms).

Se nel 2PL normale mettiamo un lucchetto su un oggetto specifico (una riga o un documento), il Predicate Lock mette un lucchetto su una condizione (un predicato).

1. Il limite del 2PL classico
   Immagina di voler prenotare una sala prove tra le 14:00 e le 15:00.

La Transazione A controlla se ci sono prenotazioni in quell'orario. Il database dice: "No, la tabella è vuota".

La Transazione A decide di prenotare.

Ma visto che la tabella era vuota, il 2PL non ha "oggetti" fisici su cui mettere un lucchetto!

La Transazione B può infilarsi e inserire una prenotazione per lo stesso orario prima che A finisca. Ecco il Fantasma.

2. Cos'è il Predicate Lock?
   Invece di bloccare un record esistente, il database blocca tutti gli oggetti che corrispondono a una ricerca, anche quelli che non esistono ancora!

Un lucchetto di predicato non appartiene a un oggetto, ma a una query, ad esempio:
SELECT \* FROM prenotazioni WHERE sala_id = 123 AND inizio < '15:00' AND fine > '14:00';

Se la Transazione A ha un Predicate Lock su questa query, la Transazione B non può inserire una nuova riga che corrisponda a quei criteri finché A non ha finito.

Il lucchetto agisce come un "campo di forza" su una zona del database definita dalla tua condizione WHERE.

3. Il problema del Predicate Lock (e la soluzione: Index Range Locks)
   Il problema è che controllare ogni singola scrittura rispetto a tutti i Predicate Lock attivi è lentissimo (richiede molta CPU). Per questo, quasi nessun database usa i Predicate Lock puri. Usano invece gli Index Range Locks (Blocchi sugli intervalli degli indici):

Se hai un indice sulla colonna orario, il database blocca un intervallo nell'indice (es. tutto ciò che sta tra le 14 e le 15).

È un'approssimazione del Predicate Lock: è un po' meno preciso (potrebbe bloccare qualche riga in più del necessario), ma è molto più veloce da gestire per il database.

#### Index-range locks

L'Index-range locking (chiamato anche Next-key locking) è la versione "pragmatica" e semplificata dei Predicate Locks.

Kleppmann spiega che i Predicate Locks sono bellissimi in teoria, ma in pratica le prestazioni sono disastrose perché il database dovrebbe controllare ogni singola operazione contro una lista potenzialmente infinita di condizioni complesse.

Ecco come il database "bara" (in modo intelligente) per ottenere lo stesso risultato senza uccidere la CPU:

1. Dall'astratto al concreto (L'Indice)
   Invece di bloccare una condizione astratta (es. "tutte le sale tra le 14 e le 15"), il database mette il lucchetto direttamente sugli intervalli dell'indice.

Se hai un indice sulla colonna orario_inizio, l'indice sarà una lista ordinata di valori. L'index-range locking blocca:

Il valore specifico che hai cercato.

Lo spazio vuoto (gap) prima o dopo quel valore nell'indice.

2. Perché risolve i "Fantasmi"?
   Bloccando i "gap" tra i valori esistenti nell'indice, il database impedisce a chiunque di inserire una nuova riga in quel punto.

**esempio**
Se la Transazione A cerca prenotazioni tra le 14:00 e le 15:00, il database blocca l'intero segmento dell'indice che copre quell'orario.

Se la Transazione B prova a inserire una prenotazione alle 14:30, deve "scrivere" in quel punto dell'indice. Ma il punto è bloccato, quindi B deve aspettare.

Il Fantasma è sconfitto: Poiché B non può inserire nulla nell'intervallo, la ricerca di A rimarrà coerente per tutta la durata della transazione.

#### 💻 Perché è importante per un Senior Dev?

Questo concetto ti fa capire perché gli indici sono fondamentali non solo per la velocità delle SELECT, ma anche per la sicurezza delle transazioni:

Senza Indici: Il database non può fare index-range locking. Per essere sicuro di evitare Fantasmi, dovrebbe bloccare l'intera tabella. Questo uccide le performance di "Track-em-all" se hai molti utenti.

Con Indici: Il database può essere "chirurgico", bloccando solo la piccola fetta di dati che ti serve, lasciando il resto della tabella libero per gli altri.

In breve: l'Index-range lock è un'approssimazione sicura dei Predicate Locks. Blocca un po' più del necessario, ma lo fa in modo incredibilmente efficiente.

## Serializable Snapshot Isolation (SSI)

It provides full serializability, but has only a small performance penalty compoared to snapshot isolation.

### Pessimistic vs optimitic concurrency control
