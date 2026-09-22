# Riassunto capitolo 10: Batch and Stream Processing (in corso — inizio 22/09)

> **Wiki (inglese, promosso):** TBD → `../ch-10-batch-and-stream-processing.md`  
> **Concept estratte:** TBD → [[map-distributed-systems]]  
> **Provenienza:** lettura a mano avviata **2026-09-22** (~pp. 1–6 / ~40)

---

## Intro capitolo

> Compila dopo le prime pagine.

**Key idea (bozza libro):** i dati non finiscono quando sono scritti nel DB — servono **job** che li leggono, trasformano e producono altri dataset (batch) oppure li elaborano **in continuo** (stream).

**Collegamento al cap. 9:** consensus/consistency = *come* i dati restano coerenti; cap. 10 = *come* li **muovi e rielabori** dopo.

---

## Batch processing with Unix tools

> **Stato:** da leggere / digitare.

### Idea chiave

*(1–2 frasi dopo lettura)*

### Simple log analysis

| | |
|--|--|

### The Unix philosophy

- …

### Alternative: awk, sed, …

---

## MapReduce and Distributed Filesystems

> **Stato:** da leggere / digitare.

### Idea chiave

*(distribuire il modello “file in → file out” su tanti nodi)*

### Distributed filesystems (HDFS-style)

### MapReduce job execution

### MapReduce workflows / higher-level tools

### Beyond MapReduce (joins, grouping — se nel tuo libro è sotto questa sezione)

---

## Stream Processing

> **Stato:** da leggere / digitare.

### Idea chiave

*(batch = finito; stream = continuo / unbounded)*

### Messaging systems / event logs

### Partitioning streams

### Stream joins / windows (quando arrivi)

---

## Filo narrativo (memoria)

```text
Unix pipes / file intermedi
  → MapReduce + DFS (stesso spirito, in grande)
  → limiti batch / job compositi
  → stream (log, messaging) — stesso dato, tempo continuo
  → …
```

---

## Domande aperte

- …

---

## Book club

*(English — paste-ready. Fill after a solid chunk of reading.)*

Here's my take on chapter 10 so far:

### …

### One line to remember

> …

---

## Checklist lettura

- [ ] Unix tools / philosophy
- [ ] MapReduce + DFS
- [ ] Beyond MapReduce (se presente)
- [ ] Messaging / event streams
- [ ] Stream processing core (joins, windows, …)
