# GitLab Pages — il modello in quattro pezzi

> Raw, italiano. Imparato il **31/07-03/08/2026** pubblicando la dashboard di `tracking-ds`
> (`ds-tracker`), poi consolidato con un pomodoro `learn-error-simulator` (4 scenari).
> Termine di paragone utile: la smoke-app del DS deploya su **S3 + CloudFront** — due attori
> espliciti, bucket e CDN. Su Pages non ne dichiari nessuno: da qui la confusione iniziale.

---

## I quattro pezzi del modello

| Domanda | Risposta |
|---|---|
| **Cosa** viene pubblicato | tutto e solo ciò che sta in **`public/`** a fine job |
| **Come** sopravvive | `artifacts: paths: [public]` → caricato nello storage GitLab; Pages serve da lì |
| **Chi** attiva il deploy | il **nome del job**: `pages`. Convenzione magica, nessun flag |
| **Quando** | a **ogni** run riuscito del job, su **qualunque** branch → serve un guardrail |

Il workspace del job viene distrutto a fine job. Senza la riga `artifacts`, `public/` esisterebbe
per qualche secondo e svanirebbe. Non configuri nessun bucket: la destinazione è implicita.

## La riga `artifacts` fa tre cose, non una

```yaml
artifacts:
  paths: [public]     # ≡  paths:\n  - public   (lista YAML inline)
```

1. **Sopravvivenza** — l'artefatto resta dopo la distruzione del workspace.
2. **Trasporto tra job** — per default un job **scarica gli artefatti di tutti gli stage
   precedenti**. È così che due job si passano file, non avendo un disco condiviso.
3. **Pubblicazione** — per il job `pages`, l'artefatto **è** ciò che viene pubblicato. Pages non
   guarda il workspace, legge l'artefatto caricato. Per questo la cartella **deve** chiamarsi
   `public`: è il contratto, non estetica.

## Il guardrail obbligatorio

```yaml
rules:
  - if: $CI_COMMIT_BRANCH == $CI_DEFAULT_BRANCH
```

Pages deploya a ogni run del job, **ovunque giri**. Senza questa regola, un push su un branch
qualunque sovrascrive il sito di produzione, in silenzio.

⚠️ **Ma la regola ha un lato buio simmetrico** (scenario 3 del simulatore): se la punti a un branch
di test e poi te ne dimentichi, le pipeline di `main` restano **tutte verdi** — il job `pages`
semplicemente non viene creato — e il sito resta **congelato sul contenuto del branch**. Nessun job
rosso, nessun warning: potenzialmente non te ne accorgi mai. E dopo il merge quel branch si cancella,
quindi il sito serve contenuto di provenienza non più ricostruibile.

Corollario: un **typo** nel nome del branch dentro la `rules` non dà errore. La pipeline gira e non
ha niente da eseguire. (Successo davvero: `test-based` invece di `text-based`.)

## Ogni deploy è uno snapshot completo e immutabile

Non si fonde niente, non si accumula niente: il nuovo artefatto **rimpiazza** il sito.

Verifica logica (è il modo per ricordarselo): *se i deploy si accumulassero, come rimuoveresti un
file dal sito?* Non esisterebbe modo. Un `rm` nel job cambia solo il **nuovo** artefatto — quindi
funziona **solo** perché il nuovo artefatto sostituisce il vecchio.

Conseguenza pratica: per aggiornare **un solo** file, il job deve ri-produrre **tutti** i file del
sito. Un job che copia solo `latest.json` in `public/` fa sparire `index.html` → radice in 404.

## L'URL vero, e perché non si indovina

Tre posti dove si trova:
- interfaccia: **Deploy → Pages**
- API: `GET /projects/:id/pages` → campo `url`
- **il modo giusto**: la variabile predefinita **`$CI_PAGES_URL`** dentro il job
  (`- echo "Published at $CI_PAGES_URL"`) → il log diventa la fonte, sempre aggiornata.
  Esiste anche `$CI_PAGES_DOMAIN`.

Storicamente l'URL era prevedibile (`https://<namespace>.gitlab.io/<progetto>`). Con i **domini
unici** attivi diventa `ds-tracker-9c2c7d.gitlab.io`, con suffisso casuale. **Non è un capriccio:**
su un dominio condiviso tutti i siti Pages sarebbero *same-origin* tra loro, e un sito potrebbe
leggere cookie e localStorage di un altro. Il suffisso dà a ogni progetto un'origine propria. Il
prezzo è che l'URL non è derivabile dal nome del progetto → va letto.

## Artefatto ≠ deployment (l'abbaglio del 31/07)

```
/-/jobs/<id>/artifacts/public/index.html   ← browser degli ARTEFATTI, non il sito
https://ds-tracker-9c2c7d.gitlab.io/       ← il sito
```

Sono **due oggetti distinti**: il primo mostra il contenuto dello zip di *quel* job, legato al suo ID
e alla sua scadenza; il secondo è un *pages deployment*, creato a partire da quell'artefatto e con
vita propria (l'API li elenca separatamente, con la loro data). L'artefatto può scadere e il sito
continua a funzionare — ma quel link no. **Non è un indirizzo condivisibile.**

## Accesso: `private` e il 404 deliberato

Con `pages_access_level: private` il sito richiede login GitLab **e** l'appartenenza al progetto.

Chi non è membro, **dopo** un login riuscito, vede **404** — non 403. È voluto: un 403 confermerebbe
*"questa risorsa esiste ma non puoi vederla"*, e già solo quello è una fuga di informazione (nomi di
progetti, struttura). Le risorse private sono rese **indistinguibili da quelle inesistenti**.

Vocabolario da tenere preciso: in quel caso **l'autenticazione è riuscita, l'autorizzazione no**.
Dire "non hai le credenziali" manda la persona a cercare una password sbagliata.

Per verificare, guardare i membri **inclusi gli ereditati** — l'accesso può venire dal gruppo padre:
```
/projects/:id/members       → solo diretti
/projects/:id/members/all   → diretti + ereditati   ← questo
```

## Fuori dal job `pages`: cosa serve davvero

Il job `pages` **non ha bisogno di token**: copia file. Serve invece configurazione per uno **scan
schedulato** — variabili CI per i token + un token con permesso di **push** per ricommittare i
risultati. Verificato il 30/07 su `ds-tracker`: nessuna variabile CI e nessuno schedule esistenti.

---

## Da ricordare (5 righe)

1. `public/` = cosa · `artifacts` = come sopravvive/viaggia/si pubblica · nome `pages` = chi attiva · ogni run = quando.
2. Gli artefatti degli stage precedenti **arrivano da soli** nel job successivo. `mkdir -p` **non azzera**.
3. Ogni deploy **sostituisce** tutto: per cambiare un file, ri-produci tutti i file.
4. La `rules` sul branch di default è obbligatoria — e un typo lì non dà errore, dà silenzio.
5. URL non indovinabile (domini unici = origine separata) → `$CI_PAGES_URL`. Il link `/-/jobs/.../artifacts/` non è il sito.

---

## Tutorial — i 3 trap (errori del 16/09)

Riletto dopo il simulatore. Ogni pezzo: cosa credevi → cosa fa GitLab → analogia → come verificarlo.

### 1. Il sito nuovo non si somma a quello vecchio

**Cosa è andato storto.** Job `pages` mette in `public/` solo `latest.json`. Pensavi: la home di settimana scorsa resta, magari con dati vecchi.

**Cosa fa Pages.** Ogni run del job `pages` pubblica **un artifact intero** e quello **diventa** il sito. Il precedente viene **buttato**, non fuso. Se nell’artifact manca `index.html`, la home **non** la pesca dal deploy vecchio → 404 / sito spezzato.

**Analogia.** Non è un cassetto in cui aggiungi un foglio. È una **foto che sostituisce l’album**: se oggi scatti solo il json, le altre pagine non sono “ancora nell’album”.

**Check.** “Per togliere un file dal sito, basta non metterlo nel **nuovo** `public/`.” Se i deploy si accumulassero, non avresti modo di cancellare. Quindi **devono** rimpiazzare.

**Cosa fare.** Ogni `pages` ricostruisce **tutto** `public/` (come fa tracking-ds: copia l’intera `dashboard/`, non un file solo).

### 2. Gli artifact dello stage prima arrivano da soli

**Cosa è andato storto.** Due job in fila (`scan` poi `pages`). `pages` non fa `cp` di `metrics.json`. Pensavi: la cartella di `pages` è vuota da quel file, perché “è `pages` che copia da dashboard a public”.

**Cosa fa GitLab.** I job **non** condividono il disco (a fine job la macchina muore). Per questo esistono gli artifact: file zippati e **scaricati di default** nel job dello **stage successivo**. Senza `needs` speciale, senza `cp` tuo. (`needs: [tests]` in tracking-ds è un altro discorso: *ordine/gate*, non “non scaricare niente”.)

**Analogia.** Turno 1 lascia un **pacco in magazzino**. Turno 2 trova il pacco già in sala, anche se non è andato in magazzino a prenderlo. Non è lo stesso tavolo: è il magazzino GitLab.

**Check.** All’inizio di `pages`, un `ls` può mostrare file che **non** hai copiato nello `script:`. Quelli sono artifact dello stage prima.

### 3. `mkdir -p` non svuota

**Cosa è andato storto.** `public/` c’è già (file vecchi arrivati al punto 2). Fai `mkdir -p public` e `cp latest.json public/`. Pensavi: cartella pulita, solo latest.

**Cosa fa `mkdir -p`.** “Crea la catena di cartelle **se manca**.” Se `public/` **esiste**, non tocca i file dentro. Risultato: `index.html` vecchio **+** `latest.json`.

**Analogia.** “Apri il cassetto se non c’è.” Se il cassetto c’è già pieno, non lo svuoti: ci metti un altro foglio.

**Cosa fare se vuoi pulito.** `rm -rf public && mkdir -p public` (o ricostruire `public/` da zero copiando l’albero giusto, come la dashboard). Poi ricorda il punto 1: quello che resta in `public/` **è** il sito intero.

