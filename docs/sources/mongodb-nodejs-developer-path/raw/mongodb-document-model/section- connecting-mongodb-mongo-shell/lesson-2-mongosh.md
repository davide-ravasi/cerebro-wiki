# MongoDB Shell (`mongosh`)

Shell ufficiale per MongoDB: gestire e interrogare il database da terminale.

> Per la sintassi della **connection string**, vedi [`lesson-1-connection-string.md`](./lesson-1-connection-string.md).

## Comandi utili

| Comando            | Effetto              |
| ------------------ | -------------------- |
| `db`               | Database corrente    |
| `use <nome-db>`    | Cambia database      |
| `show collections` | Elenca le collezioni |
| `show dbs`         | Elenca i database    |
| `help`             | Aiuto comandi        |

## Riferimento rapido: connettersi con una stringa

```bash
mongosh "<connection-string>"
```

Esempio locale:

```bash
mongosh "mongodb://localhost:27017/trackemall"
```

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

## Lab: connettersi al cluster Atlas con `mongosh`

In questo lab si recupera la **connection string** del cluster Atlas e si usa per connettersi con la **MongoDB Shell**.

> Le credenziali qui sotto sono **placeholder**. Nel lab reale puoi usare i valori forniti dal corso; in produzione **non committare** mai password o host reali.

### 1. Recuperare la connection string da Atlas

1. Vai alla pagina del database Atlas e fai clic su **Connect** accanto al cluster (es. `myAtlasClusterEDU`).
2. Nel dialog, seleziona **Shell** per generare la stringa di connessione.
3. Clicca sull’icona di copia per copiarla.

### 2. Connettersi al cluster

Incolla la stringa nel terminale e sostituisci `<db_username>` con l’utente del lab. Quando viene richiesta la password, inseriscila (nel lab del corso, ad esempio `myatlas-001`).

Comando tipico:

```bash
mongosh "mongodb+srv://<cluster-host>" --apiVersion 1 --username <db_username>
```

Esempio (con placeholder):

```bash
mongosh "mongodb+srv://myatlasclusteredu.xxxxxx.mongodb.net" --apiVersion 1 --username myAtlasDBUser
```

### 3. Elencare i database

Una volta dentro `mongosh`:

```shell
show dbs
```

Mostra i database presenti nel cluster Atlas.

### 4. Uscire dalla shell

```shell
exit
```

### Recap

- **Connect** su Atlas → opzione **Shell** → copia la connection string.
- `mongosh "<connection-string>" --apiVersion 1 --username <db_username>` → connessione.
- `show dbs` → esplori i database; `exit` → chiudi `mongosh`.
