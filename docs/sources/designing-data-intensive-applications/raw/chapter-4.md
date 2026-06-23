# Chapter 4: Encoding and Evolution

L'idea centrale è che i sistemi cambiano costantemente.

Il codice cambia (rolling upgrades), ma i dati nel database restano.

Come facciamo a far convivere versioni diverse?

Attraverso la **Compatibilità**:

- **Backward (Retrocompatibilità):** Il nuovo codice legge dati scritti dal vecchio
- **Forward (Compatibilità in avanti):** Il vecchio codice legge dati scritti dal nuovo (ignorando i campi ignoti).

---

## **Formati di Codifica (Encoding Formats)**

Come trasformiamo gli oggetti in memoria (es. oggetti JS) in byte da inviare sulla rete?

### **A. Formati Testuali (JSON, XML, CSV)**

- **Pro:** Leggibili dagli umani, universali.
- **Contro:** Occupano molto spazio, ambiguità sui numeri (es. precisione dei float in JS), non hanno schemi rigidi di default.

### **B. Binari (Thrift & Protocol Buffers)**

Sviluppati da Facebook (Thrift) e Google (Protobuf).

Usano un **ID numerico (tag)** per ogni campo.

- **Evoluzione:** Puoi aggiungere campi (nuovo tag), ma non puoi cambiare il tag di un campo esistente.
- **Forward Compatibility:** Se il vecchio codice vede un tag che non conosce, lo salta semplicemente.

### **C. Apache Avro (La "Maga" degli Schemi)**

È il formato più raffinato. Non usa tag numerici.

- **Writer’s vs Reader’s Schema:** Chi scrive usa uno schema, chi legge ne usa un altro. Avro confronta i due "al volo" risolvendo le differenze (Schema Resolution).
- **Dynamic:** È perfetto per i database SQL. Se aggiungi una colonna al DB, Avro genera il nuovo schema automaticamente senza che tu debba aggiornare i "tag" a mano.

---

### **Modalità di Dataflow (Come si muovono i dati)**

Una volta codificati i dati, come viaggiano tra i servizi?

### **A. Attraverso il Database**

- Il database è un messaggio inviato al **"te stesso del futuro"**.
- **Problema:** Se il nuovo codice scrive un dato e il vecchio codice lo riscrive subito dopo, i nuovi campi potrebbero andare persi (Data outlives code).

### **B. Attraverso i Servizi (REST & SOAP)**

- **REST:** Stile basato su HTTP e JSON. Più semplice, flessibile e amato nel Frontend.
- **SOAP:** Protocollo XML rigido basato su WSDL. Molto comune in ambito bancario/enterprise per la sua estrema formalità.
- **RPC (Remote Procedure Call):** Cerca di far sembrare una chiamata di rete come una funzione locale. _Pericolo:_ La rete può fallire, le funzioni locali no.

---

### **C. Attraverso il Message Passing (Asincrono)**

Uso di Message Brokers (Kafka, RabbitMQ).

- Il mittente non aspetta una risposta.
- Migliora l'isolamento: se il destinatario è giù, il messaggio resta in coda.

---

---

### **3. Punti Salienti per il tuo Ripasso (The "Golden Rules")**

| **Concetto**         | **Definizione Rapida**                                                                          |
| -------------------- | ----------------------------------------------------------------------------------------------- |
| **Schema Evolution** | La capacità di cambiare la struttura dei dati senza rompere il sistema.                         |
| **Binary Encoding**  | Più veloce e leggero del JSON (fondamentale per le performance).                                |
| **Default Values**   | La chiave per la compatibilità: permettono al codice di "riempire i vuoti" quando mancano dati. |
| **Loose Coupling**   | Usare schemi (come in Avro) permette a Backend e Frontend di evolvere in tempi diversi.         |

## 📡 Capitolo 4: Encoding and Evolution

_Come i dati viaggiano e cambiano nel tempo._

- **Compatibilità:** \* **Backward:** Codice nuovo legge dati vecchi.
  - **Forward:** Codice vecchio legge dati nuovi (ignorandoli).
- **Formati Binari:** \* **Protobuf/Thrift:** Usano tag numerici per evolvere lo schema.
  - **Apache Avro:** Usa un **Writer's Schema** e un **Reader's Schema**. È il più elegante per sistemi dinamici.
- **Dataflow:**
  - **Database:** Messaggi per il "te stesso del futuro".
  - **REST vs SOAP:** REST (flessibile/JSON) vs SOAP (rigido/XML/WSDL).
  - **RPC:** Far sembrare la rete una funzione locale (pericoloso perché la rete fallisce spesso).

---

Riassunti utili

# 📡 Capitolo 4: Encoding and Evolution (Deep Dive)

_Come i dati si trasformano (Encoding) e come fluiscono tra i sistemi (Dataflow)._

---

## 🏗️ 1. Formati di Codifica (Encoding)

- **Testuali (JSON, XML, CSV):** Facili, universali ma pesanti. Problemi con i grandi numeri (float in JS) e assenza di schemi rigidi.
- **Binari (Thrift, Protobuf, Avro):** Convertono i dati in byte. Più veloci, compatti e sicuri.
  - **Thrift/Protobuf:** Usano **Field Tags** (numeri) per identificare i campi.
  - **Avro:** Non usa tag. Si basa sull'ordine dei campi e sulla risoluzione tra **Writer's Schema** e **Reader's Schema**.

## 🔄 2. La Sfida della Compatibilità

- **Forward (Avanti):** Il vecchio codice ignora i nuovi campi.
- **Backward (Indietro):** Il nuovo codice gestisce i vecchi dati usando i **valori di default**.
- **Database:** Il "te stesso del passato" parla al "te stesso del futuro". Attenzione ai cicli di scrittura che possono cancellare nuovi campi ignoti al vecchio codice.

## 🚚 3. Modalità di Dataflow (Come si muovono i dati)

1. **Via Database:** I dati sopravvivono al codice.
2. **Via Servizi (REST/RPC):** Comunicazione sincrona. REST è flessibile (JSON), RPC (gRPC) è veloce ma rischioso se la rete fallisce.
3. **Via Message Brokers (Kafka):** Comunicazione asincrona. Garantisce isolamento e scalabilità.

# 📨 Capitolo 4.3: Message Passing Dataflow

_I Message Broker sono sistemi intermediari che permettono a diverse applicazioni (Producer e Consumer) di comunicare in modo asincrono, agendo come una via di mezzo tra un database e una chiamata di rete._

---

## 🏗️ 1. Architettura dei Sistemi a Messaggi

Invece di una connessione diretta (come REST), il flusso segue questo schema:

1. **Producer (Produttore):** Invia un messaggio a un "Topic" o una "Coda" sul Broker.
2. **Message Broker:** Riceve il messaggio, lo memorizza temporaneamente e lo rende disponibile.
3. **Consumer (Consumatore):** Legge il messaggio dal Topic e lo elabora.

---

## 🌟 2. Vantaggi del Modello Asincrono

- **Durabilità (Buffering):** Se il Consumer è offline o lento, i messaggi non vanno persi; restano nel Broker finché non vengono processati.
- **Isolamento (Decoupling):** Il Producer non ha bisogno di conoscere l'indirizzo IP del Consumer, né se sia attivo in quel momento.
- **Fan-out:** Un singolo messaggio può essere consegnato a più Consumer diversi contemporaneamente (es: un servizio per le email e uno per le statistiche).
- **Affidabilità:** Se un Consumer crasha mentre elabora, il Broker può riconsegnare il messaggio una volta che il servizio è ripartito.

---

## 🧬 3. Evoluzione dello Schema nei Messaggi

In un sistema a messaggi, la compatibilità è fondamentale perché Producer e Consumer cambiano in tempi diversi:

- **Forward Compatibility:** Il vecchio Consumer deve poter ignorare i nuovi campi aggiunti dal nuovo Producer.
- **Backward Compatibility:** Il nuovo Consumer deve poter leggere i vecchi messaggi ancora presenti nel Broker usando i **valori di default**.
- **Avro & Registry:** Spesso si usa un **Schema Registry** per assicurarsi che i messaggi binari inviati nel Topic siano sempre comprensibili da chi li legge.

---

## 🆚 4. Tipologie di Broker

| Caratteristica    | Message Queues (es. RabbitMQ)                 | Log-based Brokers (es. Kafka)                                     |
| ----------------- | --------------------------------------------- | ----------------------------------------------------------------- |
| **Persistenza**   | I messaggi vengono eliminati una volta letti. | I messaggi vengono salvati su disco per giorni o settimane.       |
| **Rigiocabilità** | Non puoi rileggere messaggi già consumati.    | Puoi "riavvolgere" e rileggere i dati (fondamentale per analisi). |
| **Performance**   | Ottimo per task brevi e conferme veloci.      | Scalabilità massiva (milioni di messaggi al secondo).             |

---

### 🎸 Il parallelo con il tuo P-Bass (L'Interfaccia Audio)

- **REST (Sincrono):** È suonare dal vivo. Se il mixer si rompe o il pubblico non ascolta in quel momento, la nota è persa per sempre.
- **Message Broker (Asincrono):** È registrare una traccia sul tuo software (DAW). La traccia resta lì (nel Broker). Il tuo produttore (Consumer) potrà ascoltarla domani, riascoltarla dieci volte o aggiungerci degli effetti, anche se tu in quel momento stai facendo altro.

---

## Book club

Paste-ready copy: [`book-club/chapter-4.md`](../book-club/chapter-4.md)

Here's my take on the key concepts in chapter 4:

### The Compatibility

The biggest challenge is that code changes every day, but data outlives code. We have to manage two directions:

- **Backward Compatibility:** My new code needs to be smart enough to read the "old" data sitting in the DB without crashing. Usually, this means having solid default values for new fields.
- **Forward Compatibility:** Old versions of our code need to be able to read data written by the new version. The old code should just ignore the fields it doesn't recognize.

### Binary Formats

JSON it’s heavy and "loose." When performance and schema safety matter, binary is better:

- **Protobuf & Thrift:** They use Field Tags (numbers). Instead of sending the string `"username"`, they just send `1`. It’s fast and compact.
- **Apache Avro:** No tags, no field names. It relies on a **Writer’s Schema** and a **Reader’s Schema**. As long as you have the "map" (the schema), you can decode the raw bits.

### Dataflow

Data is always in transit through three main channels:

1. **Databases:** Think of the DB as a **"message to your future self."** You write something today, and you (or a newer version of your app) will read it in two years. You better make sure that future-you knows how to parse it!
2. **REST vs. SOAP:** REST is not a strict protocol but a design style. It usually uses **JSON**, making it super flexible. **SOAP** is an XML-based protocol that uses a **WSDL** (a strict "rulebook" file). The code must follow the contract exactly, or it won't even compile.
3. **RPC:** Tries to make a request to a remote server look exactly like calling a local function in your code. It's messy: it can timeout, the remote server might be down, or the "plug" might be pulled.

### One line to remember

> Data outlives code — design **encoding**, **schemas**, and **compatibility** so past and future versions of your app can still talk to what's in storage.
