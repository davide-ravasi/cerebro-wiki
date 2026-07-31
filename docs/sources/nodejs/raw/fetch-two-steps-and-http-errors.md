# fetch — due passaggi, errori HTTP, e perché serve un server

> Raw, italiano. Imparato il **2026-07-31** collegando la pagina statica di `tracking-ds`
> (`dashboard/index.html`) al `latest.json` pubblicato su GitLab Pages.
> Metodo: `learn-operational-fast`. Promosso in `concepts/web/` (vedi §Promote in fondo).

---

## 1. `FileReader` non è `fetch` — sono due sorgenti diverse

Punto di partenza sbagliato che ho scritto io:

```js
const input = "latest.json";
const f = new File([input], "latest.json", { type: "application/json" });
reader.readAsText(input, "utf8");
```

Due errori, uno banale e uno concettuale.

- **Banale:** il primo argomento di `new File([...])` è il **contenuto**, non il nome. Quel file conteneva
  gli 11 caratteri `latest.json`. (E `readAsText` vuole un `Blob`, non una stringa → `TypeError`.)
- **Concettuale, quello che conta:** **niente in `File`/`FileReader` fa una richiesta di rete.**

| API | Da dove prende i dati |
|---|---|
| `FileReader` | un `Blob`/`File` che **hai già in memoria**: input file, drag&drop, costruttore |
| `fetch` | va a **prendere** una risorsa via HTTP |

Il server non è nel vocabolario di `FileReader`. Se i dati stanno su un server, `FileReader` è il
tool sbagliato, non un tool da usare diversamente.

## 2. Perché `fetch` ha DUE passaggi asincroni

Questo era il "flusso che non capivo".

```js
fetch("./latest.json")
  .then(response => response.json())   // 1° passaggio
  .then(data => { ... })               // 2° passaggio
```

- `fetch(...)` restituisce **subito** una Promise (non i dati).
- Quella Promise si risolve con un **`Response`**: la *busta* della risposta HTTP — status,
  headers, e il corpo come **stream non ancora letto**.
- Si risolve **appena arrivano gli header**. A quel punto il corpo può essere ancora in viaggio.
- `.json()` **finisce di leggere** lo stream e ci fa un `JSON.parse`. Leggere uno stream è a sua
  volta asincrono → seconda Promise.

Le due Promise significano quindi: **"la risposta è iniziata"** e **"la risposta è finita ed è
stata interpretata"**.

⚠️ Vocabolario: `.json()` **non converte**. Scarica il resto e interpreta. Non è un cambio di formato.

## 3. `fetch` NON rifiuta sugli errori HTTP

Il trap più importante della giornata.

**`fetch` rifiuta solo se la rete si rompe** (DNS, connessione, CORS). Un `404` o un `500` sono
risposte HTTP **riuscite** dal suo punto di vista: la promise si risolve normalmente.

Conseguenza pratica: senza controllo, su un 404 arrivi a `.json()` su una pagina HTML di errore, e
vedi un **parse error incomprensibile** invece di "not found".

Il controllo va nel **primo** `.then()`, l'unico punto dove hai il `Response` in mano *prima* che
diventi dati:

```js
fetch("./latest.json")
  .then(response => {
    if (response.ok) {
      return response.json();          // continua la catena con i dati
    } else {
      throw new Error(`HTTP ${response.status} ${response.statusText}`.trim());
    }
  })
  .then(data => render(el, data))
  .catch(error => { /* rete rotta, !ok, o parse error: tutto qui */ });
```

Due errori miei prima di arrivarci:

- Ho messo `if (response.ok)` nel **secondo** `.then()` → `response` **non è in scope** lì (è il
  parametro del primo callback) → `ReferenceError`, e il render non avveniva mai.
- Anche fosse in scope, sarebbe **troppo tardi**: `.json()` è già stato chiamato sul corpo d'errore.

## 4. `throw` dentro un `.then()` finisce nel `.catch()`

Meccanismo da ricordare: dentro un callback della catena, `throw` **rifiuta** la promise di quel
passaggio, e la rifiuzione **salta i `.then()` successivi** fino al primo `.catch()`.

Per questo la coppia "`throw` nel primo then + un solo `.catch()` in fondo" gestisce **tre**
famiglie di problemi in un punto: rete rotta, HTTP non-ok, JSON malformato. Se invece di `throw`
avessi *restituito* un valore, la catena sarebbe continuata come se nulla fosse.

## 5. `statusText` è vuoto in HTTP/2 → usa `status`

Bug che passa tutti i test locali e si manifesta **solo online**.

**HTTP/2 ha eliminato la reason phrase** dal protocollo. Quindi `response.statusText` è una
**stringa vuota** su un server HTTP/2.

- `npx serve` in locale parla **HTTP/1.1** → `statusText` = `"Not Found"`, tutto sembra a posto.
- GitLab Pages serve in **HTTP/2** (verificato: `curl -sI` → `HTTP/2 302`) → messaggio d'errore
  vuoto: *"Error fetching latest.json: "*.

Regola: **`status` (numero) sopravvive sempre**, `statusText` è un extra.

```js
throw new Error(`HTTP ${response.status} ${response.statusText}`.trim());
// locale:  "HTTP 404 Not Found"
// Pages:   "HTTP 404"
```

## 6. `file://` non ha un'origine HTTP → il fetch è bloccato

Aprendo la pagina con doppio clic l'URL è `file://C:/...`. Il fetch **fallisce comunque**, con un
errore che sembra un bug del tuo codice: `TypeError: Failed to fetch`, o un errore CORS con
`origin: null`.

Il modello di sicurezza del browser ragiona per **origine** (schema + host + porta). `file://` non
ne ha una utilizzabile, quindi le richieste sono bloccate a monte. Serve servire la cartella via
HTTP:

```bash
npx serve dashboard          # → http://localhost:3000
python -m http.server 8000   # da dentro la cartella
```

Test che l'ambiente è a posto **prima** di debuggare il codice: apri direttamente
`http://localhost:3000/latest.json`. Se vedi il JSON, il fetch ha tutto ciò che gli serve.

## 7. `npx` — eseguire un pacchetto senza installarlo

Due cose distinte che si scrivono insieme:

- **`npx`** = il lanciatore che arriva con npm. Scarica un pacchetto in una cache temporanea se non
  l'hai, lo esegue, e **non lo aggiunge alle dipendenze del progetto**. Per i tool usa-e-getta.
- **`serve`** = il pacchetto: server HTTP statico minimale. Espone una cartella su `localhost` con i
  MIME type corretti. Nessuna build, nessun watch, nessun reload.

Perché `npx` e non un'installazione: `serve` **non serve alla pagina**, serve a **rimuovere un
ostacolo del protocollo `file://`** durante lo sviluppo. In produzione quel ruolo lo fa GitLab
Pages. È strumento, non dipendenza.

---

## Da ricordare (5 righe)

1. `FileReader` = byte che hai già · `fetch` = rete. Non interscambiabili.
2. Due passaggi perché gli **header arrivano prima del corpo**; `.json()` finisce di leggere lo stream.
3. `fetch` **non** rifiuta su 404/500 → controlla `response.ok` nel **primo** `.then()`.
4. `throw` in un `.then()` → arriva al `.catch()`, che copre rete + HTTP + parse.
5. `statusText` è vuoto in HTTP/2 → il numero è `status`. `file://` non ha origine → serve un server.

## Promote

Promosso in wiki EN il 2026-07-31:

- `concepts/web/fetch-response-model.md` — il modello a due passaggi
- `concepts/web/http-errors-in-fetch.md` — `response.ok`, `throw` → `catch`
- `concepts/web/http2-reason-phrase.md` — `statusText` vuoto in HTTP/2

Collegati da [[map-web-security]] (sezione HTTP client).
