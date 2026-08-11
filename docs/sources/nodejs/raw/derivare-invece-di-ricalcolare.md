# Derivare invece di ricalcolare

> Raw, italiano. Imparato il **10-11/08/2026** scrivendo il wiring del file derivato di metriche in
> `tracking-ds` (`writeMetricsArtifact` + `calculateMetricsPath`), poi consolidato con
> `learn-core-idea-first`. Il caso è banale — costruire il percorso di un file — ma il principio
> vale ogni volta che due valori "devono corrispondere".

---

## L'idea centrale

**Se due valori devono corrispondere, non calcolarli entrambi: ricava il secondo dal primo.**

Due valori calcolati in parallelo dallo stesso input *possono* divergere, e allora servono
meccanismi per tenerli allineati. Un valore derivato dall'altro **non può** divergere: non c'è un
secondo calcolo da allineare.

## Il caso concreto

Uno scan scrive `scan-results/2026-08-11/12-00-00-metrics-clone.json`. Il file di metriche derivato
deve stare in `metrics-results/` con lo **stesso** nome, per essere accoppiabile a occhio.

| Approccio | Come | Cosa può rompersi |
|---|---|---|
| **Ricalcolare** | ogni scrittore legge l'orologio e formatta data e ora | scan alle `12:00:00.980`, derivato alle `12:00:01.010` → nomi diversi, coppia separata, nessun errore |
| **Derivare** | leggi il percorso dello scan e sostituisci un segmento di cartella | niente: non c'è un secondo orologio |

Nel secondo, l'orologio viene letto **una volta sola**, da chi scrive lo scan. Non "i due orari
coincidono" — non esistono due orari.

## La conseguenza a monte: cosa cade con il campo

Il piano iniziale prevedeva un campo `generatedAt` dentro il file derivato. Da lì nascevano due
decisioni implementative: **passare l'istante condiviso** a entrambi gli scrittori, e un **helper di
formattazione** condiviso perché i due producessero la stessa stringa.

Poi il campo è stato eliminato — un derivato è funzione pura del suo input, la sua data è quella
dell'input, e tenerlo costringeva a scegliere fra onestà (l'ora del calcolo, che rende il file non
riproducibile) e riproducibilità (l'ora dello scan, che fa mentire il nome del campo).

Le due decisioni erano rimaste nel piano, orfane, e ci ho continuato a lavorare sopra per una serata.

> **Lezione trasferibile:** quando togli un campo da un contratto, controlla quali decisioni
> implementative esistevano *solo* per servirlo. Di solito una o due, e restano in piedi da sole
> perché nessuno le rilegge.

## Le quattro trappole "stessa macchina, macchina diversa"

Incontrate tutte nello stesso giro, e sono una famiglia sola: **un valore che finisce in un file
versionato non deve dipendere dall'ambiente.**

| Cosa | Perché tradisce | Rimedio |
|---|---|---|
| `a.localeCompare(b)` | senza locale esplicito segue quello di sistema → ordina diversamente su Windows IT e su un runner Linux | confronto sulle unità di codice (`<` / `>`) o locale fissato |
| `path.relative(a, b)` | restituisce `\` su Windows, `/` su Linux | normalizzare a `/` |
| ora locale nei nomi file | dipende dal fuso della macchina | UTC (`toISOString`) |
| `segments.join(path.sep)` | stesso problema in uscita | `join("/")` — Node accetta `/` anche su Windows per l'I/O |

## Due trappole di logica, dallo stesso giro

**Segmento contro sottostringa.** `path.includes("scan-results")` è vero anche per
`my-scan-results/`. E `String.replace` sostituisce la **prima** occorrenza, che in
`my-scan-results/x/scan-results/y.json` è quella sbagliata. Spezzare in segmenti
(`split(/[\\/]/)`) e confrontare per intero risolve entrambe insieme: `indexOf` su un array
confronta gli elementi, non le sottostringhe.

**Lo zero falsy.** `if (!segments.indexOf(X))` si comporta all'opposto di quello che sembra: se `X`
è il **primo** segmento `indexOf` restituisce `0` → `!0` è vero → lancia a torto; se non lo trova
restituisce `-1` → `!-1` è falso → non lancia. Stesso giorno, stessa trappola in un altro punto:
`if (!distance)` trattava `distance: 0` — un repo perfettamente aggiornato — come "dato assente".

> **Regola:** quando una funzione restituisce un numero come informazione, non trattarlo come
> booleano. Confronta esplicitamente col valore-sentinella.

## Fallire piano o fallire forte

`calculateMetricsPath` su un percorso senza `scan-results` poteva restituire il percorso
**invariato**. Sembra innocuo, ma il chiamante poi fa `writeFile(percorso, contenuto)` → avrebbe
**sovrascritto il file di input**.

Quando la conseguenza di un errore è irreversibile, il fallimento deve essere impossibile da
ignorare: **eccezione, non `null`**. Un `null` va controllato, e chi dimentica il controllo distrugge
il file.

## Quando il test è difficile, di solito è il codice a dirti qualcosa

Il primo tentativo leggeva l'orologio e la directory corrente. Il test risultava impossibile da
scrivere: passava solo in un istante preciso della giornata, e solo su una piattaforma.

Non era il test a essere sbagliato. **Una funzione difficile da testare per motivi che non c'entrano
con la sua logica sta prendendo input che non le servono.** Con la firma corretta — percorso in,
percorso fuori — il test è una riga deterministica su qualunque macchina.

E il corollario: se "aggiusti" il test costruendo l'atteso con la **stessa** logica
dell'implementazione (`join(path.sep)` da entrambe le parti), ottieni un test che passa sempre,
qualunque cosa faccia il codice.

---

## Dove

- `tracking-ds/src/metrics/calculateMetricsPath.ts` + i suoi 5 test
- `tracking-ds/src/writeMetricsArtifact.ts`
- `tracking-ds/docs/adr/0001-metrics-json-single-derived-file.md`
