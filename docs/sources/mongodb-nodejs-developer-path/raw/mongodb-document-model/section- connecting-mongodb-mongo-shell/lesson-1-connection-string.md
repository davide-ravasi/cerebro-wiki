# Using Mongodb connection strings

Connect to our cluster and work with our data

two formats:

- standard
- srv
  mongodb://[username:password@]host1[:port1],...hostN[:portN]][/[defaultauthdb][?options]]

  ## Analisi dei componenti

  **mongodb://**: Il prefisso obbligatorio che identifica il protocollo. Se usi un cluster Atlas (DB-as-a-service), vedrai spesso mongodb+srv://, che indica l'uso di record DNS SRV per una configurazione più flessibile.

  **username:password@**: Le credenziali di autenticazione. Se incluse nella stringa, devono essere codificate per URL se contengono caratteri speciali (come @, :, /).

  **host1[:port1]**: L'indirizzo del server. Se non specificata, la porta di default è 27017. In una configurazione MERN professionale, qui troverai spesso l'indirizzo del cluster (es. cluster0.abcde.mongodb.net).

  /defaultauthdb: Il nome del database che il driver userà per autenticare l'utente (spesso è admin). Se non specificato, il driver cercherà di autenticarsi sul database indicato nella stringa o su admin.

  ?options: Una serie di parametri query string per configurare il comportamento della connessione.

## Esempi Pratici

1. Connessione Locale (Sviluppo)
   Per il tuo ambiente locale durante i test su React e Node:
   mongodb://localhost:27017/trackemall

2. Connessione Atlas (Produzione/Cloud)
   Molto comune quando pubblichi su Netlify o simili:
   mongodb+srv://davide:TuaPasswordSicura@cluster0.mongodb.net/trackemall?retryWrites=true&w=majority
