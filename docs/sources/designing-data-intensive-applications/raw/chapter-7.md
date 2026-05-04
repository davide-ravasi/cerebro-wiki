# 📚 Riassunto Capitolo 7: Transazioni

_Tratto da: "Designing Data-Intensive Applications" (Martin Kleppmann)_

Le transazioni non sono una soluzione "magica", ma un insieme di astrazioni che permettono all'applicazione di gestire errori e concorrenza senza impazzire.

---

## 1. Il Concetto di Transazione (ACID)

L'obiettivo è raggruppare più letture e scritture in un'unica unità logica.

- **Atomicity (Atomicità):** Se qualcosa fallisce, la transazione viene interrotta e tutte le modifiche fatte vengono scartate (**Abortability**).
- **Consistency (Coerenza):** Il database passa da uno stato valido a un altro (responsabilità dell'applicazione).
- **Isolation (Isolamento):** Transazioni eseguite contemporaneamente non si calpestano i piedi.
- **Durability (Durabilità):** Una volta fatto il _commit_, i dati non vengono persi nemmeno in caso di guasto hardware.

---

## 2. Livelli di Isolamento Deboli (Weak Isolation)

A causa delle performance, si usano livelli meno rigidi:

### Read Committed

- **No Dirty Reads:** Vedi solo dati che hanno già fatto il commit.
- **No Dirty Writes:** Non puoi sovrascrivere dati di una transazione ancora aperta.

### Snapshot Isolation (MVCC)

Risolve il **Read Skew** (vedere dati diversi durante una lettura lunga).

- **MVCC (Multi-Version Concurrency Control):** Il database tiene diverse versioni dello stesso oggetto.
- **Vantaggio:** I lettori non bloccano mai gli scrittori e viceversa.

---

## 3. Prevenire i "Lost Updates"

Accade nel ciclo _leggi-modifica-scrivi_. Se due utenti salvano insieme, uno perde le modifiche.

- **Scritture Atomiche:** Operatori come `$inc` in MongoDB.
- **Locking Esplicito:** Bloccare i record letti (es. `SELECT FOR UPDATE`).
- **Compare-and-set:** Aggiornare solo se il valore è ancora quello che avevamo letto.

---

## 4. Anomalie Avanzate: Write Skew e Phantoms

- **Write Skew:** Due transazioni leggono gli stessi dati, ma aggiornano oggetti diversi, violando una regola logica (es. due medici che si cancellano insieme dal turno).
- **Phantoms (Fantasmi):** Quando una scrittura cambia il risultato di una query di ricerca in un'altra transazione aperta.

---

## 5. Serializzabilità (Il Massimo Livello)

Garantisce che l'esecuzione parallela sia identica a un'esecuzione sequenziale.

| Metodo                                    | Approccio    | Descrizione                                                                                |
| :---------------------------------------- | :----------- | :----------------------------------------------------------------------------------------- |
| **Actual Serial Execution**               | Fisico       | Esegue le transazioni una alla volta su un unico thread.                                   |
| **Two-Phase Locking (2PL)**               | Pessimistico | Chi legge blocca chi scrive. Usa **Index-range locks** per evitare i fantasmi.             |
| **SSI (Serializable Snapshot Isolation)** | Ottimistico  | Le transazioni corrono veloci; il DB le interrompe al commit se rileva un conflitto reale. |

---

## 💻 Applicazione per Frontend Developer (MERN)

- **MongoDB:** Supporta le transazioni multi-documento dalla v4.0 usando lo **Snapshot Isolation**.
- **Design:** Preferire aggiornamenti atomici quando possibile per mantenere alta la velocità.
- **Retry:** Quando si usano transazioni complesse, il backend Node.js deve gestire gli errori di conflitto

---- note più dettagliate --------

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

It provides full serializability, but has only a small performance penalty compared to snapshot isolation.

Il Serializable Snapshot Isolation (SSI) è un algoritmo relativamente nuovo (reso famoso da PostgreSQL intorno al 2012) che Kleppmann adora perché risolve il grande dilemma dei database: come avere la massima sicurezza (Serializzabilità) senza distruggere le prestazioni.

### Pessimistic vs optimistic concurrency control

Two-Phase Locking (2PL) è PESSIMISTICO: Il database assume che qualcosa andrà male, quindi blocca tutto preventivamente. Se vuoi leggere, aspetti; se vuoi scrivere, aspetti. È come un semaforo che sta sempre sul rosso "per sicurezza".

Serial execution è estremamente pessimistico. é equivalente a bloccare tutto il database per ogni transaction. In compnenso le preformances sono alte per le singole transazioni.

SSI è OTTIMISTICO: Il database ti lascia correre. Non mette lucchetti (lock). Ti permette di leggere e scrivere liberamente, sperando che non ci siano conflitti. Combina snapshot isolation con un algoritmo che detect se ci sono conflitti di serializzazione tra i vari writes.

### DEcisions based on an outdated premise

Il database sta facendo azioni basate su possibili outdated premises.

Come può quindi controllare che quando fa commit non ci siano errori?

Invece di bloccare gli altri, il database osserva e annota. Mentre la tua transazione gira, il database tiene traccia di due cose:

Se hai letto dati che nel frattempo sono stati modificati da qualcun altro (letture obsolete).

Se le tue scritture potrebbero aver invalidato le letture di qualcun altro.

Quando arrivi al momento del commit, il database fa un controllo rapido. Se vede che è avvenuta una Write Skew (come quella dei medici), allora e solo allora interrompe la transazione e ti costringe a riprovare.

Il SSI permette di avere i vantaggi dello Snapshot Isolation (letture veloci che non bloccano nessuno) ma aggiunge un algoritmo che scova i "fantasmi" e le "distorsioni" automaticamente.

Vantaggio: Le letture sono istantanee. Non devi preoccuparti di Predicate Locks o indici bloccati.

Svantaggio: Se il database è molto congestionato e ci sono tantissimi conflitti, molte transazioni verranno "abortite" e dovrai gestirne il riprovo (retry) nel tuo codice Node.js.

# Chapter 7 Summary: Transactions

## Core Definition (ACID)

Transactions provide a safety mechanism to group multiple reads and writes into a single logical unit.

- **Atomicity**: The ability to abort a transaction on error and discard all writes from it.
- **Consistency**: The database remains in a valid state (primarily an application-level responsibility).
- **Isolation**: Concurrently executing transactions are isolated from each other.
- **Durability**: Once a transaction is committed, data will not be lost.

## Weak Isolation Levels

Most databases use weak isolation levels to maintain performance.

- **Read Committed**: Prevents dirty reads (reading uncommitted data) and dirty writes (overwriting uncommitted data).
- **Snapshot Isolation (MVCC)**: Solves read skew by allowing each transaction to read from a consistent snapshot of the database. This ensures readers do not block writers and vice versa.

## Write Anomalies

- **Lost Update**: Occurs in the read-modify-write cycle when two transactions attempt to update the same value simultaneously.
  - _Solutions_: Atomic write operations (e.g., MongoDB's `$inc`), explicit locking, or compare-and-set.
- **Write Skew**: Occurs when two transactions read the same objects but update different objects, violating a business requirement (e.g., the doctor on-call example).
- **Phantoms**: Occurs when a write in one transaction changes the result of a search query in another transaction.

## Serializability

The strongest level of isolation, ensuring the result is the same as if transactions executed one at a time.

1. **Actual Serial Execution**: Physically executing one transaction at a time on a single thread.
2. **Two-Phase Locking (2PL)**: A pessimistic approach where writers block readers and vice versa. It uses index-range locks to prevent phantoms.
3. **Serializable Snapshot Isolation (SSI)**: An optimistic approach that allows transactions to proceed without locks, only aborting at commit time if a conflict is detected.

## Practical Implications (MERN/MongoDB)

- **MongoDB**: Supports multi-document transactions using Snapshot Isolation (since v4.0).
- **Implementation**: For many web applications, atomic operations are preferred over full transactions for better performance.
- **Error Handling**: When using high isolation levels like SSI, the application backend must be designed to retry transactions that fail due to serialization conflicts.
