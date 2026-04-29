# Chapter 3: Storage and Retrieval

_Cosa succede fisicamente sul disco._

- **LSM-Trees (Log-Structured):** Scritture veloci in coda. Usa la **Compaction** per pulire i duplicati. (Es: Cassandra, RocksDB).
- **B-Trees (Page-Oriented):** Standard SQL. Divide il disco in pagine fisse. Più lento a scrivere, molto affidabile in lettura.
- **OLTP vs OLAP:**
  - **OLTP:** Transazioni utente veloci (es. "Aggiungi al carrello").
  - **OLAP (Analytics):** Analisi su milioni di righe. Usa lo **Storage a colonne** per comprimere i dati e velocizzare i report.

---

_Cosa succede quando invii un dato al database? Come fa il database a ritrovarlo tra miliardi di altri record? Il Capitolo 3 esplora le strutture dati sottostanti ai motori di storage._

---

## 🏗️ 1. I due grandi approcci: Log-Structured vs. Page-Oriented

### **A. Log-Structured (LSM-Trees)**

- **Concetto:** Il database scrive i dati semplicemente aggiungendoli alla fine di un file (**Append-only**). È il modo più veloce possibile per scrivere su disco.
- **SSTables (Sorted String Tables):** I segmenti di log vengono ordinati per chiave.
- **Compaction (Il "Cleanup"):** Poiché scriviamo sempre in coda, avremo duplicati della stessa chiave (es. l'utente cambia nome 3 volte). Il processo di _Compaction_ fonde i file, tenendo solo l'ultima versione di ogni chiave e liberando spazio.
- **Esempi:** Cassandra, RocksDB, LevelDB.

### **B. Page-Oriented (B-Trees)**

- **Concetto:** È lo standard dei database relazionali da 40 anni. Divide il database in "pagine" di dimensione fissa (solitamente 4KB).
- **Struttura:** Una struttura ad albero dove ogni pagina punta ad altre pagine tramite riferimenti.
- **Vantaggio:** Molto stabile per le letture e le transazioni. Più lento nelle scritture perché deve aggiornare diverse pagine e gestire i "split" delle pagine quando si riempiono.
- **Esempi:** PostgreSQL, MySQL, SQLite.

---

## 🔍 2. Indici: Trovare l'ago nel pagliaio

- **Cos'è un indice?** È una struttura dati aggiuntiva derivata dai dati primari.
- **Il Trade-off:** Gli indici velocizzano le **letture** (SELECT), ma rallentano ogni singola **scrittura** (INSERT/UPDATE) perché anche l'indice deve essere aggiornato.

---

## 📊 3. OLTP vs. OLAP (Transazioni vs. Analisi)

### **OLTP (Online Transaction Processing)**

- **Uso:** Applicazioni rivolte all'utente (es. il tuo "Track-em-all").
- **Pattern:** Tante piccole query che leggono/scrivono pochi record per volta tramite una chiave (ID).
- **Focus:** Bassa latenza, alta disponibilità.

### **OLAP (Online Analytical Processing)**

- **Uso:** Business Intelligence e Data Science.
- **Pattern:** Query enormi che leggono milioni di righe ma solo poche colonne (es. "Somma totale vendite di Aprile").
- **Column-Oriented Storage:** Invece di salvare i dati riga per riga, li salva colonna per colonna. Questo permette una **compressione incredibile** e una velocità di lettura pazzesca per i report.

---

## 🧠 4. In-Memory Databases

- Alcuni database (come **Redis**) tengono tutto nella RAM invece che sul disco.
- **Perché?** Non è solo per la velocità del supporto, ma perché eliminano la complessità di dover tradurre le strutture dati in memoria in formati compatibili con il disco.

---

### 🎸 Il parallelo con il tuo P-Bass (Edizione Storage)

- **LSM-Trees (Log-Structured):** È come registrare un concerto su un **nastro magnetico**. Continui a incidere senza mai fermarti (scrittura veloce). Se vuoi sentire l'ultima versione di un brano, devi scorrere il nastro fino alla fine o fare un "taglia e cuci" (**Compaction**) dei nastri vecchi.
- **B-Trees (Page-Oriented):** È come un **vecchio raccoglitore di spartiti** con le linguette. Se devi aggiungere una nota a pagina 42 e non c'è spazio, devi creare una nuova pagina 42b e aggiornare l'indice all'inizio. È più ordinato per trovare le cose subito, ma richiede più tempo per "archiviare".
- **Column-Oriented (OLAP):** Immagina di avere 10 bassisti che suonano. Invece di registrare l'intera band, registri solo la traccia di basso di tutti e 10 in un unico file. Se vuoi studiare come suonano il basso, non devi ascoltare batteria e chitarra; vai dritto alla colonna del basso.
