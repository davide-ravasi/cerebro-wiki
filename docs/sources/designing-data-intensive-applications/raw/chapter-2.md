# Chapter 2: Data Models and Query Languages

Questo capitolo è fondamentale per te come **Frontend Developer**, perché spiega la logica dietro la scelta tra un database a documenti (come MongoDB nello stack MERN) e uno relazionale (SQL), e come questo influenzi il modo in cui scrivi il codice.

_Scegliere la "forma" dei dati._

- **Relational Model (SQL):** Rigido, ottimo per relazioni complesse e Join.
- **Document Model (NoSQL):** Flessibile (**Schema-on-read**). Migliore località dei dati per documenti auto-contenuti (JSON).
- **Graph Model:** Per dati dove le relazioni sono fitte (Social Network, Frodi).
- **Linguaggi:** Differenza tra **Dichiarativo** (SQL - dici _cosa_ vuoi) e **Imperativo** (JS - dici _come_ farlo).

---

_Il modello dei dati è probabilmente la parte più importante dello sviluppo di un software, perché condiziona non solo come scriviamo il codice, ma anche come pensiamo al problema che stiamo risolvendo._

---

## 🏗️ 1. Relational Model vs. Document Model

### **A. Il Modello Relazionale (SQL)**

- **Struttura:** Dati organizzati in tabelle (relazioni) con schemi rigidi.
- **Punto di forza:** Ottimo per le **Join** e per relazioni "molti-a-uno" o "molti-a-molti".
- **Normalizzazione:** I dati vengono divisi per evitare ridondanze. Se un utente cambia indirizzo, lo cambi in un solo posto.

### **B. Il Modello a Documenti (NoSQL)**

- **Struttura:** Dati salvati in documenti (JSON/BSON). Ideale per lo stack **MERN**.
- **Locality (Località dei dati):** Se hai bisogno di un profilo utente con tutte le sue preferenze, lo tiri su con una sola query. Non servono Join.
- **Schema-on-read:** Il database non impone una struttura. La struttura viene decisa dal codice applicativo (Frontend/Backend) quando legge i dati.

---

## 🔄 2. La "Mancanza di Corrispondenza" (Impedance Mismatch)

- **Definizione:** Il divario tra gli oggetti nel codice (es. le classi in Java o gli oggetti in JS/TypeScript) e le tabelle nel database SQL.
- **Soluzione:** Spesso usiamo gli **ORM** (come Prisma o Mongoose) per tradurre tra i due mondi, ma Kleppmann avverte che questa astrazione non è mai perfetta.

---

## 🔗 3. Relazioni: Molti-a-Uno e Molti-a-Molti

### **Perché NoSQL soffre le relazioni?**

- Se i tuoi dati sono molto collegati tra loro (es. un social network dove tutti seguono tutti), il modello a documenti diventa inefficiente perché devi simulare le Join nel codice applicativo.
- **Evoluzione:** Kleppmann nota che SQL sta aggiungendo il supporto JSON e NoSQL sta aggiungendo funzionalità simili alle Join. I due mondi stanno convergendo.
