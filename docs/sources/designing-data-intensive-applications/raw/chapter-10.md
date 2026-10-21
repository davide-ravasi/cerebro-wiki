# Riassunto capitolo 10: Batch and Stream Processing (in corso)

> **Wiki (inglese, promosso):** TBD → `../ch-10-batch-and-stream-processing.md`  
> **Concept estratte:** TBD → [[map-distributed-systems]]  
> **Provenienza:** lettura a mano · **2026-09-22** ~pp. 1–6 · **2026-09-29** ~pp. 6–11 · **2026-10-06** ~pp. 11–15 / ~40  
> **Note dettagliate:** a fine capitolo (scelta 29/09)

---

## Intro capitolo

> Compila dopo le prime pagine / a fine capitolo.

**Key idea (bozza libro):** i dati non finiscono quando sono scritti nel DB — servono **job** che li leggono, trasformano e producono altri dataset (batch) oppure li elaborano **in continuo** (stream).

**Collegamento al cap. 9:** consensus/consistency = *come* i dati restano coerenti; cap. 10 = *come* li **muovi e rielabori** dopo.

---

## Batch processing with Unix tools

> **Stato:** letto a mano (~fino a p.11 inizio MR). Digitazione → fine capitolo.

### Idea chiave (lab 29/09 — ponte a MR)

Unix: file in → pipe/filtri → file out. MapReduce = stesso spirito su cluster.

### Simple log analysis / Unix philosophy

*(note a fine capitolo)*

---

## MapReduce and Distributed Filesystems

> **Stato:** in corso — fino a ~p.15 (06/10).

### Idea chiave (ferma in chat 29/09)

- **Map:** record → serie di `(key, value)`
- **Shuffle:** raggruppa per chiave
- **Reduce:** per ogni chiave, aggrega i valori → output (file)

Esempio URL count: map `(url, 1)` → reduce somma.

### Distributed filesystems (HDFS-style)

*(da leggere / note fine cap.)*

### MapReduce job execution

*(da leggere / note fine cap.)*

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

> MapReduce = Unix pipes in grande (map → shuffle → reduce).

---

## Checklist lettura

- [x] Unix tools / philosophy *(letto 29/09; note a fine cap.)*
- [x] MapReduce core-idea ✓ 13/10 (map/shuffle/reduce) · DFS / resto lettura in corso
- [x] Join (batch/MR) core-idea ✓ 13/10
- [ ] Resto cap. 10 — lista utente 13/10; sessione **14/10** (reduce-side ✓ in corso):
  - [x] Reduce-side joins and grouping — **core 14/10** (etichetta dataset · 1+N · join nel reduce · skew accennato). Rilettura 20/10
  - [x] Map-side joins — **core 20/10** (broadcast se piccolo · oppure fette co-partizionate; altrimenti reduce-side). Sessione: riletto anche reduce-side; gate ok
  - [x] Output of batch workflows — **letto 20/10** (senza appunti) · da fissare in Q&A con joins
  - [ ] Hadoop vs distributed databases — **skip deep** (surplus; ok cenni in Q&A se esce)
  - [ ] Beyond MapReduce — **skip deep** (surplus dopo Fowler / se serve)
- [x] **Path chiuso 21/10** — core MR/joins/output enough; Q&A joins opz. se ripasso. **Fowler 2ª** = libro attivo. Cap. 11–12 surplus.
- [ ] Cap. 11–12 — **non** in path ora (surplus dopo Fowler)
