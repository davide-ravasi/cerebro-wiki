# Chapter 5: Distributed data

# 📖 Capitolo 5: Replication (DDIA)

> **Core Idea**: Mantenere copie degli stessi dati su più macchine (nodi) per garantire alta disponibilità, bassa latenza e scalabilità delle letture.

---

## 🏛️ 1. Architetture di Controllo

_Chi decide cosa viene scritto?_

### 🟢 Single-Leader (Master/Slave)

- **Meccanismo**: Tutte le scritture vanno al **Leader**. I **Followers** scaricano il log delle modifiche.
- **Failover**: Se il leader cade, si elegge un follower (Timeout + Elezione).
- **Rischio**: _Split Brain_ (due leader contemporaneamente).

### 🟡 Multi-Leader

- **Meccanismo**: Più leader (spesso in data center diversi).
- **Vantaggi**: Scritture locali veloci, tolleranza ai guasti del data center.
- **Sfida**: Risoluzione dei conflitti (chi vince se due scrivono sullo stesso dato?).

### 🔵 Leaderless (Dynamo-style)

- **Meccanismo**: Il client scrive a tutti i nodi contemporaneamente.
- **Quorum**: $w + r > n$ (Scritture + Letture > Nodi Totali) per garantire dati freschi.
- **Esempi**: Cassandra, Riak, Dynamo.

---

## ⚡ 2. Metodi di Propagazione (Replication Logs)

| Metodo                  | Descrizione            | Rischio/Vantaggio                                      |
| ----------------------- | ---------------------- | ------------------------------------------------------ |
| **Statement-based**     | Invia l'SQL originale  | ❌ Funzioni come `NOW()` creano dati diversi sui nodi. |
| **WAL (Write-Ahead)**   | Invia i byte del disco | ✅ Velocissimo; ❌ Blocca le versioni del software.    |
| **Logical (Row-based)** | Invia i cambi per riga | ✅ Il più flessibile (ottimo per migrazioni).          |

---

## 🕰️ 3. I 3 Problemi della Consistenza Eventuale

_Fondamentale per il lavoro di Frontend (React)._

1. **Read-After-Write Consistency**
   - _Problema_: Scrivo, ricarico e non vedo il dato.
   - _Soluzione_: Leggere dal **Leader** per i dati propri dell'utente.
2. **Monotonic Reads**
