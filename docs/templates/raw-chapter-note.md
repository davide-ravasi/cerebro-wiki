---
title: "Raw chapter note — template (DDIA-style)"
type: template
language: it
promote_to_language: en
---

# Riassunto capitolo N: <Titolo capitolo> (in corso | completo)

> **Wiki (inglese, promosso):** [[source-<slug>-ch-NN]] — `../ch-NN-<slug>.md`  
> **Concept estratte:** `<slug-1>`, `<slug-2>`, … → [[map-<domain>]]  
> **Provenienza:** rilettura / appunti a mano (scan) / data

---

## <Sezione del libro (## come nel capitolo)>

### <Sottosezione (### opzionale)>

**Key idea:** (1–2 frasi: cosa devo ricordare davvero)

| Opzione A | Opzione B |
|-----------|-----------|
| … | … |

- bullet per cause, meccanismi, elenchi

**Takeaway:** sintesi operativa (opzionale se key idea basta)

**Example:** caso concreto, anche breve (opzionale)

**Open question:** cosa non è ancora chiarissimo (opzionale)

→ [[concept-<slug>]] (quando esiste già un concept promosso)

---

## Filo narrativo del capitolo (memoria)

```text
Idea 1 → conseguenza → idea 2 → …
```

---

## Domande aperte

- …

---

## Convenzione lingua (cerebro-wiki)

| Layer | Lingua |
|-------|--------|
| `sources/**/raw/` | **Italiano** — appunti personali, scan, memorizzazione |
| `sources/**/ch-*.md` (source promossi) | **English** |
| `concepts/`, `patterns/`, `maps/`, `glossary/` | **English** |

Promozione: raw IT → source EN + concept EN; link `[[...]]`, non duplicare parola per parola.

Riferimento: `sources/designing-data-intensive-applications/raw/chapter-8.md`
