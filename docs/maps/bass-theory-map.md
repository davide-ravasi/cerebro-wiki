---
id: map-bass-theory
title: "Bass Theory Map"
type: map
domain: music
tags: [map, navigation, music, bass, non-core]
status: planned
updated: 2026-08-12
---

# Purpose

Piano per un progetto **cerebro-style dedicato alla teoria musicale applicata al basso** — percorso completo da zero a suonare a orecchio / leggere sigle, con esercizi, ascolto guidato ed esempi da brani reali.

**Non-core** (vedi `study-resume.md` regola 5: Mongo e basso = solo surplus). Questa mappa è la memoria del piano discusso il 2026-08-12 — **da avviare dopo la vacanza (~fine agosto/settembre)**.

# Decisione: tre pezzi, non uno

| Progetto | Ruolo | Stato |
|----------|-------|-------|
| **cerebro-bass** *(da creare)* o mappa in cerebro | Teoria, roadmap, concetti, esercizi testuali | Piano in questa mappa |
| **learn-bass-fretboard** | Simulazione / pratica sul manico | Esiste (visualizzatore) |
| **bass-sheets-library** (`react-exp/bass-sheets-library`) | Catalogo spartiti scritti a mano (foto → metadati → ricerca → status studio) | Scaffold Next.js + brainstorming completo (5/08); MVP non ancora costruito |

Stesso pattern già in uso: `cerebro` (teoria) + app pratiche. Qui **sheets-library = repertorio personale reale** (le tue trascrizioni/spartiti), non solo brani di YouTube.

# Limiti onesti dell'agente (da tenere a mente)

- **Ascolto:** l'agente non sente audio. Può indicare brani/bassisti di riferimento, scrivere tab/notazione, progettare esercizi di riconoscimento (teoria), ma **non dà feedback su una registrazione reale**.
- **Simulazioni:** una vera simulazione su tastiera/manico con suono richiede un tool interattivo (Web Audio API / Tone.js / VexFlow per notazione) — è codice, va costruito in `learn-bass-fretboard`, non è una nota di studio.

# Percorso proposto (completo, zero → orecchio/lettura)

| Fase | Contenuto | Output atteso |
|------|-----------|----------------|
| **0. Fondamenta** | Note sul manico (posizioni/ottave), valori ritmici, metronomo/pulse | Base già in parte coperta da `learn-bass-fretboard` (visualizzatore) |
| **1. Teoria applicata** | Intervalli sul manico (orizzontale/verticale), scale maggiore/minore (naturale/armonica/melodica), modi, triadi e arpeggi | `concepts/music/` — 1 nota per concetto |
| **2. Armonia** | Lettura sigle accordi, estensioni (7/9/11/13), progressioni comuni (ii-V-I, blues, turnaround), chord tones vs passing tones | `concepts/music/` + `exercises/` |
| **3. Vocabolario da bassista** | Walking bass (jazz, approach notes), groove per genere (funk/ghost notes, rock root-fifth, Motown/R&B stile Jamerson, reggae/latin) | `repertoire/` per genere/bassista |
| **4. Orecchio + applicazione** | Riconoscimento intervalli/scale/accordi, trascrizione bassline reali, play-along | Richiede tool esterno per audio (Spotify/YouTube + tab) |
| **5. Improvvisazione/composizione** | Costruire linee proprie su una progressione, solo su forma semplice, comporre bassline originali | Sintesi finale del percorso |

# Fonti esterne consigliate (verificate 2026-08-12)

| Libro/Metodo | Autore | Punto forte |
|---------------|--------|-------------|
| Music Theory for the Bass Player | Ariane Cap | Teoria applicata direttamente al manico, hands-on — buona spina dorsale |
| Building Walking Bass Lines | Ed Friedland (Hal Leonard) | Standard per jazz/walking bass, con backing tracks |
| Bass Fretboard Basics | Paul Farnen (Musicians Institute) | Scale, intervalli, triadi, modal patterns, position playing |
| Music Theory for Bassists | Sean Malone (Hal Leonard) | Teoria pura ben spiegata, con audio d'esempio |

Non serve che l'agente "legga" questi libri: servono come riferimento per sequenziare bene il curriculum — i contenuti/esercizi specifici li porti tu (foto, PDF, indice capitoli) come già fai con DDIA.

# Struttura cartelle proposta (cerebro-bass, mirror di cerebro)

```text
cerebro-bass/
  maps/roadmap-teoria-basso.md      # questa tabella, evoluta
  sources/                          # 1 cartella per metodo/libro, raw notes
  concepts/                         # intervalli, scale, modi, arpeggi, walking-bass...
  exercises/                        # drill con criterio di successo esplicito
  repertoire/                       # brani reali + bassline da studiare, per genere/bassista
```

Skill riusabili (dal repertorio cerebro): `@learn-error-simulator` (scenari tipo "data questa progressione, quale nota target suoni?"), `@learn-core-idea-first` per concetti nuovi.

# Da decidere quando riprendi (settembre)

- [ ] Confermare se creare `cerebro-bass` come repo separato o cartella dentro `cerebro/docs`
- [ ] Verificare cosa c'è davvero in `learn-bass-fretboard` (oggi: solo visualizzatore, da confermare con codice alla mano)
- [ ] Scegliere il primo libro/metodo di riferimento per Fase 1 (consigliato: Ariane Cap, come spina dorsale)
- [ ] Decidere se e quando estendere `learn-bass-fretboard` con audio reale (Web Audio/Tone.js) per la Fase 4/5

# Come collegano (settembre+)

```text
cerebro / bass theory map     →  "cosa studiare" (intervalli, walking, ii-V-I…)
        ↓
bass-sheets-library           →  "su cosa praticarlo" (i TUOI spartiti, status, tag, key, techniques)
        ↓
learn-bass-fretboard          →  "come esercitarlo sul manico" (visualizzazione / drill)
```

**Bridge naturali già previsti nello schema sheets** (`docs/PROJECT_BRAINSTORMING.md`):
- `key`, `techniques`, `difficulty`, `status` (`to-learn` / `practicing` / `mastered`)
- `genre`, `tags`, note personali
- futura tabella `practice_sessions`

Esempio concreto: in teoria studi “walking bass in Em” → in library filtri `techniques: walking bass` + `key: Em` → apri lo spartito e pratichi → aggiorni status / sessione.

**Ordine di costruzione consigliato:** prima MVP catalogo (upload + metadati + ricerca), poi collegamento didattico (tag allineati ai concetti della mappa), poi fretboard. Non serve OCR/AI al giorno 1.

# Open threads

- Percorso "all_together": nessuna fase esclusa, ma restano **surplus** rispetto al core (track-em-all + DDIA) — non compete con la settimana tipo finché non lo promuovi tu
- Genere/repertorio preferito ancora da definire (jazz? funk? rock? mix?) — utile per calibrare Fase 3–4; **sheets-library lo rivelerà dai tuoi spartiti reali**
- Allineare vocabolario tag/techniques tra mappa teoria e schema `Sheet` (una sola lingua condivisa)
