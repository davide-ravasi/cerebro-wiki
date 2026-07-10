# Guida — studiare un articolo con le learning skills

Usa questa pagina quando leggi un **articolo**, un **capitolo**, o un pezzo denso e vuoi **carpire i punti principali** e **ragionarci sopra** — senza chiedere un riassunto passivo.

**Router:** `@learning-modes` se non sai quale skill scegliere.

---

## Le tre fasi (in ordine)

| Fase | Skill | Scopo |
|------|-------|--------|
| **1. Essenza** | `@learn-core-idea-first` | Una idea centrale + analogia + 3 domande gate |
| **2. Applicazione** | `@learn-operational-fast` | Cosa fare con quello che hai capito (operativo, passo-passo) |
| **3. Verifica** | `@learn-error-simulator` | Scenari reali — dimostri se hai capito davvero |

Non devi fare sempre tutte e tre. Salta la fase che non ti serve.

| Tu sei qui… | Parti da |
|-------------|----------|
| Confuso, troppo gergo | Fase 1 |
| Capito ma devo usarlo domani | Fase 2 |
| Letto tutto, “penso di aver capito” | Fase 3 |
| Già solido ieri, oggi solo test | Fase 3 |

---

## Fase 1 — Core idea first

**Quando:** articolo denso, termini nuovi, “non ho capito niente”.

**Cosa fa l'agent:** (1) idea centrale in 1–3 frasi, (2) analogia quotidiana senza gergo, (3) **tre domande una alla volta** — niente spiegazione completa finché non passi il gate.

### Prompt da copiare

```
@learn-core-idea-first

Sto leggendo: [titolo / URL / incolla testo]
Tema: [in una riga]
Non capisco: [paragrafo o bullet che non tornano]
Obiettivo: [es. DDIA, lavoro, esame, curiosità]
```

### Alias utili

- “traduttore” / “core idea” → stessa skill

### Dopo il gate

Chiedi esplicitamente di espandere o collegare:

```
Ora espandi il resto dell'articolo in punti strutturati.
Collega a [progetto / concetti che già conosco].
```

---

## Fase 2 — Operational fast

**Quando:** l'articolo è **how-to** o tooling; vuoi essere operativo in poco tempo.

**Cosa fa l'agent:** impara per primo / ignora per ora / un esercizio — **un passo per messaggio**, niente muro di teoria.

### Prompt da copiare

```
@learn-operational-fast

Da questo articolo voglio essere operativo su: [SKILL]
Contesto: [link o incolla sezione rilevante]
Tempo: [es. oggi pomeriggio / questa settimana]
```

### Alias utili

- “learning curve destroyer” / “rendimi operativo”

---

## Fase 3 — Error simulator

**Quando:** hai letto e vuoi **verificare** (come con gli operatori MongoDB).

**Cosa fa l'agent:** scenari concreti, tu rispondi, niente lezione lunga; correzione solo dopo tentativi falliti (regola della skill).

### Prompt da copiare

```
@learn-error-simulator

Mettimi alla prova su: [concetto dell'articolo]
Contesto opzionale: [Track'em All / il mio stack / DDIA]
```

### Alias utili

- “simulatore errori” / “mettimi alla prova”

### Fine sessione

Di’ **“stop per oggi”** quando basta. L'agent può riprendere domani con scenari più difficili.

---

## Cosa mandare (checklist)

Più mandi contesto mirato, meno il chat diventa riassunto generico.

- [ ] **Link** o **incolla** (anche solo la sezione difficile)
- [ ] **Tema** in una riga
- [ ] **Cosa non torna** — anche vago va bene
- [ ] **Obiettivo** — esame, ship feature, wiki, curiosità
- [ ] (Opzionale) **Progetto** — Track'em All, cerebro, lavoro

---

## Flusso completo (esempio)

1. Leggi l'articolo una prima volta (anche veloce).
2. `@learn-core-idea-first` + incolla il pezzo che confonde.
3. Rispondi alle 3 domande gate.
4. “Espandi e collega a …”
5. Se è pratico: `@learn-operational-fast` sullo stesso tema.
6. A fine giornata: `@learn-error-simulator` sul concetto.
7. (Opzionale) Promuovi in wiki cerebro — sotto.

---

## Wiki cerebro (opzionale)

Solo se **tu** lo chiedi:

- `docs/sources/.../raw/` — appunti IT mentre studi
- `docs/concepts/` — concept EN evergreen
- `docs/maps/` — link dalla mappa del dominio

Vedi [`docs/README.md`](../../../docs/README.md).

Prompt:

```
Promuovi in wiki: [tema] — raw IT + concept EN se ha senso.
```

---

## Richiamare questa guida in chat

```
@learning-modes guida articoli
```

oppure incolla:

```
Segui .cursor/skills/learning-modes/guida-articoli-e-lettura.md
```

---

## Riferimenti skill

| Skill | Cartella |
|-------|----------|
| Router | [learning-modes/SKILL.md](./SKILL.md) |
| Core idea | [learn-core-idea-first](../learn-core-idea-first/SKILL.md) |
| Operational | [learn-operational-fast](../learn-operational-fast/SKILL.md) |
| Simulator | [learn-error-simulator](../learn-error-simulator/SKILL.md) |
