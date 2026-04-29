how to model the relationship with:

## embedding

one to one relationship - use embedding
keeps data togheter
studios -- movies --> relazione uno a uno
exempio: unico documento, lo studio è embeddatto nei dati del movie - una sola query necessaria

one to many relationship- embedding is preferred - per tenere insieme dati che possono essere archiviati insieme e a cui posso accedere insieme

- ma posso usare references se ho un large dataset o un accesso frequente ai dati
  PER ESEMPIO: IL CAST del film, posso usare un array o posso creare referenze verso una collezione separata

many to many - emebed or refenrece dipendenete dai requirements

exemple: movies- theaters ma possono essere molti dati nell'aray quindi posso fare rifermineti incrociati tra i due - ma può costare molto - aumentare costo query - diminuire peso dei dati

Embedding

- access to single unit
- small fixed size data
- minimize db oprations

reference

- large / complex data sets
- large growing documents

another thing:

- max size of document in mongodb (16MB)
- limit of 100 levels of nesting
