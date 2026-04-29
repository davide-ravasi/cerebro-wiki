# Chapter 6: Partitioning

# 🧩 Capitolo 6: Partitioning (Sharding)

> **Core Idea**: Quando il dataset è troppo grande per un solo nodo, lo dividiamo in "partizioni" (Shard). Ogni nodo gestisce una parte dei dati per permettere la **Scalabilità Orizzontale**.

---

## 🏗️ 1. Architettura Shared-Nothing

_Il fondamento del partizionamento moderno._

- **Indipendenza**: Ogni nodo ha CPU, RAM e Disco propri.
- **Comunicazione**: I nodi si parlano solo via rete.
- **Vantaggio**: Scalabilità infinita aggiungendo server economici (Commodity Hardware).

---

## 🎯 2. Strategie di Partizionamento

_Come assegniamo i dati ai nodi?_

| Strategia    | Funzionamento                                 | Pro                                   | Contro                                                        |
| ------------ | --------------------------------------------- | ------------------------------------- | ------------------------------------------------------------- |
| **By Range** | Ordine alfabetico o temporale (es. A-M, N-Z). | Query su intervalli veloci.           | Rischio **Hotspots** (es. tutti scrivono sulla data di oggi). |
| **By Hash**  | Applica una funzione Hash alla chiave.        | Distribuzione uniforme (niente Skew). | Query su intervalli lente (**Scatter/Gather**).               |

---

## 🔍 3. Indici Secondari in Sistemi Distribuiti

_Cercare dati per campi diversi dalla chiave primaria._

### 📍 Local Index (Document-based)

- Ogni nodo indicizza solo i documenti che possiede.
- **Scrittura**: Veloce (scrivi solo in locale).
- **Lettura**: Lenta (**Scatter/Gather**). Devi interrogare tutti i nodi.
- _Nota: È il modello usato da **MongoDB**._

### 🌍 Global Index (Term-based)

- L'indice è spalmato tra i nodi in base al valore (es. tutti i "Rock" sul Nodo 1).
- **Lettura**: Veloce (vai al nodo giusto).
- **Scrittura**: Lenta e complessa (un inserimento può dover aggiornare più nodi).

---

## ⚖️ 4. Rebalancing (Ribilanciamento)

_Cosa succede quando aggiungiamo un nuovo server al cluster?_

- **Cosa EVITARE**: `hash mod N`. Se aggiungi un nodo, quasi tutti i dati devono traslocare.
- **Strategie Migliori**:
  - **Fixed Number of Partitions**: Molte partizioni logiche spostate tra pochi nodi fisici.
  - **Dynamic Partitioning**: Le partizioni si dividono a metà (mitosi) quando superano una certa dimensione (es. MongoDB, HBase).

---

## 🚦 5. Request Routing (Service Discovery)

_Come fa il client a sapere a quale nodo inviare la richiesta?_

Esistono tre architetture principali per instradare le query:

1. **Direct Routing**: Il client invia la richiesta a un nodo a caso; se non è quello giusto, il nodo la inoltra internamente.
2. **Routing Tier**: Un intermediario (load balancer o router) conosce la mappa delle partizioni e smista il traffico.
   - _In MongoDB è il processo **`mongos`**._
3. **Smart Client**: Il client scarica la mappa delle partizioni e invia la richiesta direttamente al nodo corretto.

> **Coordinamento**: Per tenere aggiornata la "mappa" delle partizioni tra i vari nodi, si usano spesso servizi esterni di consenso come **Apache ZooKeeper** o i **Config Servers** (in MongoDB).

---

## 🛠️ Connessione con il Corso MongoDB

- **Sharding**: È il termine "pratico" per il Partitioning.
- **Shard Key**: La scelta più critica. Se scelta male, avrai nodi sovraccarichi.
- **Mongos**: Funge da "Routing Tier" per nascondere la complessità del cluster al tuo backend.

---

## 🎸 Recap per il Batterista

- **Replication (Cap 5)**: Avere 3 batterie uguali in 3 studi (Sicurezza).
- **Partitioning (Cap 6)**: Smontare la batteria in più casse perché una sola è troppo pesante (Gestione del carico).
- **Routing**: Il tecnico di palco che ti dice esattamente in quale cassa hai messo il rullante così non devi aprirle tutte.
