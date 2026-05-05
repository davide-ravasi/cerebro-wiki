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

## Connecting with `mongosh`

Shell ufficiale per MongoDB: gestire e interrogare il database da terminale.

### Comandi utili

| Comando | Effetto |
|--------|---------|
| `db` | Database corrente |
| `use <nome-db>` | Cambia database |
| `show collections` | Elenca le collezioni |
| `show dbs` | Elenca i database |
| `help` | Aiuto comandi |

---

## Lab: installare `mongosh` su Ubuntu

In questo lab si installa **MongoDB Shell (`mongosh`)** in ambiente Ubuntu.

Flusso: verificare dipendenze → importare chiave PGP → aggiungere repo APT → `apt update` → installare `mongodb-mongosh`.

> **Nota:** Nel container del lab hai privilegi **root**: i comandi che su desktop richiederebbero `sudo` qui possono essere eseguiti così com’è.

### 1. Verificare `gnupg`

Il package manager richiede strumenti come `gpg`. Verifica:

```bash
gpg --version
```

### 2. Importare la chiave pubblica MongoDB (server 7.0)

```bash
wget -qO- https://www.mongodb.org/static/pgp/server-7.0.asc | sudo tee /etc/apt/trusted.gpg.d/server-7.0.asc
```

> Su container root del lab puoi omettere `sudo` se operi già come root.

### 3. Capire la versione di Ubuntu

La **list file** APT deve usare il codename giusto (`jammy`, `noble`, …):

```bash
cat /etc/os-release
```

Esempio di output (Ubuntu 22.04):

```text
PRETTY_NAME="Ubuntu 22.04.4 LTS"
NAME="Ubuntu"
VERSION_ID="22.04"
VERSION="22.04.4 LTS (Jammy Jellyfish)"
VERSION_CODENAME=jammy
ID=ubuntu
ID_LIKE=debian
...
```

Usa **`VERSION_CODENAME`** (qui `jammy`) nel passo successivo.

### 4. Creare la list file del repository MongoDB

Per **Ubuntu 22.04 (jammy)** e MongoDB **7.0**:

```bash
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
```

Se la tua distro ha un altro codename, sostituisci **`jammy`** con il valore di `VERSION_CODENAME` da `cat /etc/os-release`.

### 5. Aggiornare l’indice pacchetti e installare `mongosh`

```bash
sudo apt update
sudo apt install -y mongodb-mongosh
```

### 6. Verificare l’installazione

```bash
mongosh --version
```

Dovresti vedere la versione di `mongosh` installata.

---

## Riferimento rapido: connettersi con una stringa

```bash
mongosh "<connection-string>"
```

Oppure:

```bash
mongosh "mongodb://localhost:27017/trackemall"
```
