# Chapter 1: Reliability, Scalability, and Maintainability

---

_Il fondamento di un'applicazione solida._

- **Reliability (Affidabilità):** Il sistema deve tollerare i guasti (**Faults**) senza subire un fallimento totale (**Failure**).
- **Scalability (Scalabilità):** Capacità di gestire il carico. Si analizzano i **Load Parameters** (es. fan-out di Twitter).
  - _Vertical vs Horizontal:_ Potenziare una macchina vs aggiungere nodi.
- **Maintainability (Manutenibilità):** Rendere la vita facile ai dev del futuro (Operabilità, Semplicità, Evolvibilità).

---

_L'obiettivo di questo capitolo è definire cosa rende un'applicazione "Data-Intensive" di alta qualità. Non si tratta solo di codice, ma di come il sistema sopravvive al mondo reale._

---

## 🛡️ 1. Reliability (Affidabilità)

> "Il sistema deve continuare a funzionare correttamente, anche quando si verificano problemi."

### **A. Fault vs. Failure**

- **Fault (Guasto):** Un componente smette di seguire le specifiche (es. un disco fisso crasha).
- **Failure (Fallimento):** L'intero sistema smette di fornire il servizio all'utente finale.
- **Obiettivo:** Costruire sistemi **Fault-Tolerant** (che tollerano i guasti senza fallire).

### **B. Tipi di Guasti**

1. **Hardware Faults:** RAM corrotta, dischi rotti, interruzioni di corrente. Soluzione: _Ridondanza_.
2. **Software Errors:** Bug "dormienti" (es. memory leak, processi che mangiano CPU). Sono più pericolosi perché tendono a far cadere più nodi contemporaneamente.
3. **Human Errors:** La causa principale dei problemi. Soluzione: Sandbox, test automatizzati, monitoraggio e facilità di _Rollback_.

---

## 📈 2. Scalability (Scalabilità)

> "La capacità di gestire l'aumento del carico senza degradare le performance."

### **A. Descrivere il Carico (Load Parameters)**

- Bisogna identificare i parametri che contano: richieste al secondo (throughput), ratio letture/scritture, o il numero di utenti contemporanei.
- **Esempio Fan-out (Twitter):** Il problema non è scrivere un tweet, ma distribuirlo a milioni di follower (lettura).

### **B. Descrivere le Performance**

- **Non usare la Media:** La media nasconde i problemi dei singoli utenti.
- **Percentili ($p50, p95, p99$):** \* Il **$p95$** indica che il 95% delle richieste è più veloce di quel valore.
  - Il **$p99$ (Tail Latency)** è il più importante: rappresenta gli utenti con l'esperienza peggiore, che spesso sono quelli con più dati (i tuoi clienti "Power User").

### **C. Strategie di Scaling**

- **Scaling Up (Verticale):** Macchina più grossa (CPU/RAM). Costoso e ha un limite fisico.
- **Scaling Out (Orizzontale):** Più macchine piccole. Richiede un'architettura distribuita complessa.

---

## 🛠️ 3. Maintainability (Manutenibilità)

> "Ridurre il costo e la fatica per i dev che lavoreranno sul codice tra mesi o anni."

1. **Operability (Operatività):** Facilitare il monitoraggio e la gestione del sistema (buoni log, dashboard, visibilità).
2. **Simplicity (Semplicità):** Rimuovere la **Complessità Accidentale**. Usare buone _astrazioni_ per nascondere i dettagli tecnici sporchi.
3. **Evolvability (Evolvibilità):** Facilitare l'aggiunta di nuove feature (estremamente legato allo _Schema Evolution_ del Cap. 4).

---

### 🎸 Il parallelo con il P-Bass (Edizione "Deep Dive")

- **Reliability:** Se una corda si ossida (**Fault**), il basso deve comunque permetterti di finire il concerto senza che il manico si imbarchi o l'elettronica muoia (**Failure**).
- **Scalability:** Suonare in camera vs. suonare allo stadio. Non ti serve solo un ampli più grosso (**Scaling Up**), ma un sistema di PA che distribuisca il suono a tutti (**Scaling Out**).
- **Maintainability:** Se l'elettronica del tuo Precision economico è pulita e i cavi sono ordinati, cambiare un pickup è facile. Se è un "nido di ratti" di fili, rischi di fare danni ogni volta che lo apri.

`# 📘 Capitolo 1: Reliable, Scalable, and Maintainable Applications

_L'obiettivo di questo capitolo è definire cosa rende un'applicazione "Data-Intensive" di alta qualità. Non si tratta solo di codice, ma di come il sistema sopravvive al mondo reale._

---

## 🛡️ 1. Reliability (Affidabilità)

> "Il sistema deve continuare a funzionare correttamente, anche quando si verificano problemi."

### **A. Fault vs. Failure**

- **Fault (Guasto):** Un componente smette di seguire le specifiche (es. un disco fisso crasha).
- **Failure (Fallimento):** L'intero sistema smette di fornire il servizio all'utente finale.
- **Obiettivo:** Costruire sistemi **Fault-Tolerant** (che tollerano i guasti senza fallire).

### **B. Tipi di Guasti**

1.  **Hardware Faults:** RAM corrotta, dischi rotti, interruzioni di corrente. Soluzione: _Ridondanza_.
2.  **Software Errors:** Bug "dormienti" (es. memory leak, processi che mangiano CPU). Sono più pericolosi perché tendono a far cadere più nodi contemporaneamente.
3.  **Human Errors:** La causa principale dei problemi. Soluzione: Sandbox, test automatizzati, monitoraggio e facilità di _Rollback_.

---

## 📈 2. Scalability (Scalabilità)

> "La capacità di gestire l'aumento del carico senza degradare le performance."

### **A. Descrivere il Carico (Load Parameters)**

- Bisogna identificare i parametri che contano: richieste al secondo (throughput), ratio letture/scritture, o il numero di utenti contemporanei.
- **Esempio Fan-out (Twitter):** Il problema non è scrivere un tweet, ma distribuirlo a milioni di follower (lettura).

### **B. Descrivere le Performance**

- **Non usare la Media:** La media nasconde i problemi dei singoli utenti.
- **Percentili ($p50, p95, p99$):** \* Il **$p95$** indica che il 95% delle richieste è più veloce di quel valore.
  - Il **$p99$ (Tail Latency)** è il più importante: rappresenta gli utenti con l'esperienza peggiore, che spesso sono quelli con più dati (i tuoi clienti "Power User").

### **C. Strategie di Scaling**

- **Scaling Up (Verticale):** Macchina più grossa (CPU/RAM). Costoso e ha un limite fisico.
- **Scaling Out (Orizzontale):** Più macchine piccole. Richiede un'architettura distribuita complessa.

---

## 🛠️ 3. Maintainability (Manutenibilità)

> "Ridurre il costo e la fatica per i dev che lavoreranno sul codice tra mesi o anni."

1.  **Operability (Operatività):** Facilitare il monitoraggio e la gestione del sistema (buoni log, dashboard, visibilità).
2.  **Simplicity (Semplicità):** Rimuovere la **Complessità Accidentale**. Usare buone _astrazioni_ per nascondere i dettagli tecnici sporchi.
3.  **Evolvability (Evolvibilità):** Facilitare l'aggiunta di nuove feature (estremamente legato allo _Schema Evolution_ del Cap. 4).

---

### 🎸 Il parallelo con il P-Bass (Edizione "Deep Dive")

- **Reliability:** Se una corda si ossida (**Fault**), il basso deve comunque permetterti di finire il concerto senza che il manico si imbarchi o l'elettronica muoia (**Failure**).
- **Scalability:** Suonare in camera vs. suonare allo stadio. Non ti serve solo un ampli più grosso (**Scaling Up**), ma un sistema di PA che distribuisca il suono a tutti (**Scaling Out**).
- **Maintainability:** Se l'elettronica del tuo Precision economico è pulita e i c`
