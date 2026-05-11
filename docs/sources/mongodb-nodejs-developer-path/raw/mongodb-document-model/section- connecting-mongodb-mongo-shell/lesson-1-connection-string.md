# Using MongoDB connection strings

Connect to our cluster and work with our data.

## Formats

- **Standard** — `mongodb://...`
- **SRV** — `mongodb+srv://...` (Atlas / DNS SRV)

Template (standard):

```text
mongodb://[username:password@]host1[:port1],...hostN[:portN]][/[defaultauthdb][?options]]
```

### Analisi dei componenti

- **`mongodb://`** — Prefisso che identifica il protocollo. Su Atlas compare spesso **`mongodb+srv://`**: usa record DNS SRV per una configurazione più flessibile.
- **`username:password@`** — Credenziali. Se nella stringa, vanno **URL-encoded** se contengono caratteri speciali (`@`, `:`, `/`, ecc.).
- **`host1[:port1]`** — Host del server. Porta default **27017** se omessa. In setup MERN/cloud spesso l’host del cluster (es. `cluster0.xxxxx.mongodb.net`).
- **`/defaultauthdb`** — Database usato per l’autenticazione (spesso `admin`). Se omesso, il driver può usare il DB nel path o `admin`.
- **`?options`** — Query string (es. `retryWrites`, `w`, TLS).

## Esempi pratici

### 1. Connessione locale (sviluppo)

Per test con React e Node in locale:

```text
mongodb://localhost:27017/trackemall
```

### 2. Connessione Atlas (cloud)

Esempio tipico (sostituisci user, password e cluster):

```text
mongodb+srv://<user>:<password>@cluster0.xxxxx.mongodb.net/trackemall?retryWrites=true&w=majority
```

> Non committare stringhe con password reali. Usa variabili d’ambiente o secret manager.

---

## Lezione collegata

- Comandi `mongosh`, installazione su Ubuntu, lab Atlas → vedi [`lesson-2-mongosh.md`](./lesson-2-mongosh.md).
