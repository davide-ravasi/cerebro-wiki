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
| Lun | **20 min DDIA 2PC** (pre-session) ✓ · track-em-all: 1 pezzo **feature/UI** | ☐ track-em-all |
| Mar | DDIA: Raft / cap. 9 … | ☐ |
| Mer | ripasso: integrità favorites / never trust client | ☑ |
| Gio | ripasso: functional core / imperative shell (#2) | ☑ |
| Ven | **mattina** track-em-all (~fino 10) · **pomeriggio** Mongo quiz se c’è mezz’ora | ☑ mattina |

---

## Da ripassare (attivo — max 3)

*Mer/Gio: **1 voce per pomodoro**. In più → **Backlog ripasso**.*

### 1. DDIA — Atomic commit / 2PC (iniziato 2026-07-24)

| | |
|---|---|
| **Hook** | *Cap. 9 § Distributed transactions and consensus — dopo TOB. Atomic commit = tutti commit o tutti abort; 2PC = coordinatore + prepare/commit. Non confondere con 2PL (locking, cap. 7).* |
| **Skill** | `@learn-core-idea-first` (quando riprendi) → poi `@learn-error-simulator` |
| **Dove** | libro DDIA cap. 9 (dopo total order broadcast) · `raw/chapter-9.md` |
| **Bookmark** | Letto: single node→distributed, intro 2PC, system of promises, **coordinator failure** (2026-07-27); finire resto 2PC → Raft |

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
| DDIA cap. 9 | Lin. + TOB ok; **2PC in corso** (prepare/commit, promises, coordinator failure — 2026-07-27) | Finire 2PC → Raft → promote concept EN |
| Mongo find | confronti, elemMatch, `$and`/`$or` ✓ | prossima lezione |
| track-em-all | **Security path base ✓** · mirror front `favoriteValidation` + `useFavorite` (validate, toast max/API, RTK `.unwrap()`) ✓ | **Feature / UI** 1 pezzo lun/ven; optional later JWT refresh / cookies |
| tracking-ds | P0 | lavoro (non serale) |
| Libri coda | Fowler, Makarevich, Head First SA… | dopo blocco DDIA |

---

## Fatto di recente

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

*Ultimo aggiornamento: 2026-07-27 (DDIA 2PC: prepare/commit, promises, coordinator failure)*
