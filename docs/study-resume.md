# Study resume — riprendi domani

> **Un solo file.** Leggilo quando riapri Cursor — non serve navigare il resto di cerebro.  
> Aggiorna **Questa settimana** + **Da ripassare** a fine sessione (2 minuti).

**Obiettivo:** full-stack teorico solido + traiettoria lead.  
**Core:** track-em-all · DDIA · Mongo (poco) · Max Node solo on-demand.

---

## Settimana tipo (pomodoro 25 min)

| Giorno | Tempo | Pomodoros | Slot |
|--------|-------|-----------|------|
| **Lun** | ~1 h | **2×25** | **track-em-all** — 1 pezzo codice |
| **Mar** | ~1 h | **2×25** | **DDIA** — nuovo (leggi / core-idea) |
| **Mer** | ~30 min | **1×25** | **Ripasso** con Cursor (`@learn-error-simulator`) |
| **Gio** | ~30 min | **1×25** | **Ripasso** con Cursor (altro tema o stesso se debole) |
| **Ven** | ~1 h | **2×25** | **Mongo** corto **oppure** track-em-all |

**Pause:** 5 min tra pomodori; dopo 2 pomodori puoi fermarti (hai fatto l’ora).

**Max Node:** nessun slot fisso — 1 pomodoro solo se sblocca track-em-all.

### Come usare i 2 pomodori (Lun / Mar / Ven)

| Pomodoro | Cosa fare |
|----------|-----------|
| **1° (25)** | Obiettivo unico (codice / lettura / lezione) |
| **2° (25)** | Continua **lo stesso** obiettivo — oppure 15 min lavoro + 10 min aggiorna questo file |

### Mer / Gio (1 pomodoro)

1. Apri questa sezione **Da ripassare**
2. In chat: `@learn-error-simulator` + hook della voce
3. Fine: spunta o sposta in **Fatto di recente**

---

## Come usarlo con le skill

| Vuoi… | Scrivi in chat |
|-------|----------------|
| Concetto confuso | `@learn-core-idea-first` + **Hook** |
| Pezzo pratico | `@learn-operational-fast` + obiettivo |
| Verificare | `@learn-error-simulator` + tema |
| Solo rileggere | path in **Dove** — niente skill |

**Nuovo → ripasso:** Mar (nuovo) → Mer/Gio (simulator su quel pezzo). Lun (app) → eventualmente Gio se hai messo un hook tecnico.

---

## Questa settimana — focus

*(aggiorna ogni lunedì)*

| Giorno | Piano (1 riga) | Fatto? |
|--------|----------------|:------:|
| Lun | **20 min DDIA 2PC** ✓ · track-em-all: HomePage `useQuery` search + Context rimosso | ☑ |
| Mar | DDIA 2PC practice ✓ · **+25 min** track-em-all (`staleTime`/`refetch` same-term) ✓ · prossime: Fault-Tolerant Consensus + Membership | ☑ |
| Mer | ripasso: integrità favorites / never trust client | ☑ |
| Gio | ripasso: functional core / imperative shell (#2) | ☑ |
| Ven | **mattina** track-em-all (~fino 10) · **pomeriggio** Mongo quiz se c’è mezz’ora | ☑ mattina |

---

## Da ripassare (attivo — max 3)

*Mer/Gio: **1 voce per pomodoro**. In più → **Backlog ripasso**.*

### 1. DDIA — Atomic commit / 2PC (iniziato 2026-07-24)

| | |
|---|---|
| **Hook** | *2PC = prepare/commit + promesse. In practice: exactly-once ≈ atomicità messaggio+effetto; XA = 2PC standard; dopo prepare i lock restano “in doubt”; recovery dal log del coordinatore; limiti = costo/disponibilità. ≠ 2PL.* |
| **Skill** | `@learn-core-idea-first` (quando chiudi le 8 pp) → poi `@learn-error-simulator` |
| **Dove** | libro DDIA cap. 9 · `raw/chapter-9.md` |
| **Bookmark** | 2PC practice ✓. **Resta:** Fault-Tolerant Consensus → Membership & coordination (ZK/etcd). Poi ripasso 2PC Mer |

### 2. DDIA — Raft (dopo 2PC nel libro)

| | |
|---|---|
| **Hook** | *Come si implementa l’ordine totale? Leader + log + majority.* |
| **Skill** | `@learn-core-idea-first` → `@learn-error-simulator` |
| **Dove** | libro · `raw/chapter-9.md` |

### 3. (Opzionale) TOB / lin / causal — solo se confuso

| | |
|---|---|
| **Hook** | *TOB = stessa sequenza; lin = recency; causal = causa→effetto* |
| **Skill** | `@learn-error-simulator` |
| **Dove** | `raw/chapter-9.md` |

---

## Backlog ripasso

*Coda: non entra nel Mer/Gio finché non la promuovi in «Da ripassare».*

### track-em-all — useQuery search: draft vs committed + `enabled`

| | |
|---|---|
| **Hook** | *`textInput` = bozza (UI); `searchTerm` = chiave query. Submit aggiorna solo `searchTerm`. `useQuery` sta in top-level; `enabled: !!searchTerm` evita fetch a vuoto. Mai `useQuery` dentro l’handler.* |
| **Skill** | `@learn-error-simulator` |
| **Dove** | track-em-all `HomePage.tsx` |

### track-em-all — Context solo se serve davvero

| | |
|---|---|
| **Hook** | *Context utile se molti discendenti lontani condividono stato. Se i dati restano in Home (o bastano props a SearchBar/Search), Context è rumore — toglilo.* |
| **Skill** | `@learn-error-simulator` (scenario: “metto tutto in Context”) |
| **Dove** | HomePage (prima Provider, ora props locali) |

### track-em-all — React Query: staleTime vs refetch

| | |
|---|---|
| **Hook** | *staleTime = "ancora fresco, non rifetchare da solo". Stesso searchTerm + submit = niente in Network. refetch() = forza queryFn anche se la cache è fresh.* |
| **Skill** | `@learn-error-simulator` |
| **Dove** | cerebro `sources/react/raw/react-query-stale-time-and-refetch.md` · track-em-all `HomePage.tsx` |

### tracking-ds — paginazione API + "misura prima di costruire"

| | |
|---|---|
| **Hook** | *Paginazione API GitLab (`per_page`/`page`, loop finché una pagina torna < 100) — pattern riusabile per qualsiasi API paginata. + euristica di metodo: prima di costruire una feature, **misura il gap con una prova reale** (DEdge-Pay assente dai risultati della discovery → conferma il monorepo-gap) invece di stimare a naso.* |
| **Skill** | `@learn-operational-fast` (pratico/API) |
| **Dove** | tracking-ds `src/discover.ts` (da rileggere lunedì con calma) |

---

## In corso

| Tema | Stato | Prossimo |
|------|--------|----------|
| DDIA cap. 9 | Lin. + TOB + **2PC practice ✓**; **resta:** Fault-Tolerant Consensus + Membership/coordination | Leggere consensus (priorità) → ZK/etcd (più leggero) → promote EN |
| Mongo find | confronti, elemMatch, `$and`/`$or` ✓ | prossima lezione |
| track-em-all | Security ✓ · favorites mirror ✓ · **Home search → `useQuery` + Context rimosso** ✓ · **same-term refetch (`staleTime` / `refetch`)** ✓ | Feature/UI lun/ven; optional Load more `useInfiniteQuery`, auth `useMutation` |
| tracking-ds | P0 | lavoro (non serale) |
| Libri coda | Fowler, Makarevich, Head First SA… | dopo blocco DDIA |

---

## Fatto di recente

- **2026-07-28** — DDIA: *Distributed transactions in practice* ✓. **Resta:** Fault-Tolerant Consensus → Membership/coordination (non “8 pp di 2PC”).
- **2026-07-28** — Track'em All: same-term search `refetch()` (`staleTime` 5 min → forza `queryFn` se submit identico); nota cerebro `sources/react/raw/react-query-stale-time-and-refetch.md`
- **2026-07-27** — Track'em All Home: search `useQuery` + Context rimosso; Bugbot fix empty Search on home; **follow-up annotato:** same-term no-refetch (`staleTime` 5 min → `refetch` o `staleTime: 0`)
- **2026-07-27 (mattina, pre-session)** — DDIA 2PC ~20 min: single node→distributed atomic commit, intro 2PC, system of promises, coordinator failure
- **2026-07-24 (mattina)** — Track'em All: mirror `src/utils/favoriteValidation.js` + `useFavorite` (early return, toast max/API, RTK `.unwrap()`, type `ShowData`); keep-in-sync comments front/back
- **2026-07-24** — DDIA: bookmark cap. 9 dopo TOB → **atomic commit / 2PC** (poche pagine); ven = track-em-all mattina, Mongo quiz pomeriggio se possibile
- **2026-07-23** — Ripasso functional core / imperative shell ✓: I/O al bordo · `aggregateRawReport` puro · token iniettati (no `process.env` nel core) → **tolto dall’attivo**
- **2026-07-22 (sera)** — Ripasso favorites ✓: never trust client · duplicato su `showId` · check-then-push non atomico → **tolto dall’attivo**
- **2026-07-22** — Track'em All: chiuso logging + dependency hygiene; security path base completo; aggiornato questo resume
- **2026-07-20** — Track'em All: favorites hardening (formato, duplicato `409`, max 100, `FavoriteSchema` condiviso) + health check
- **2026-07-22** — TOB simulator + confronto lin/TOB/causal; Mongo `$and`/`$or` + `$elemMatch`; piano settimana + pomodoro
- **Lug 2026** — Track'em All security: HTTP hardening, env server-only, JWT expiry, response minimization favorites, Redux `state.auth.favorites`; cerebro raw CORS/CSP/Helmet/rate-limit
- **Lug 2026** — TOB raw; Mongo wiki; CAP, write skew, causal vs lin

---

## Template voce ripasso

```markdown
### N. [Tema]

| | |
|---|---|
| **Hook** | *una frase tua* |
| **Skill** | `@learn-...` |
| **Dove** | path o libro |
```

---

## Regole

1. **Attivo ≈ max 3**; in più → **Backlog ripasso** (poi discriminare).
2. **1 obiettivo per sessione** (anche con 2 pomodori).
3. Mer/Gio = solo ripasso da «Da ripassare» (non dal backlog intero).
4. Fine sessione: spunta tabella settimana + aggiorna ripasso (2 min).

*Ultimo aggiornamento: 2026-07-28 (DDIA 2PC practice · track-em-all staleTime/refetch · next: Fault-Tolerant Consensus)*
