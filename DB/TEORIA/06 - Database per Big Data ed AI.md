# 06 - DBMS nell'era dei Big Data e dell'AI

> **NoSQL, distribuzione e convergenza dei modelli**
> Fonte: [[DB/TEORIA/PDF/06 - Database per Big Data ed AI.pdf]]

> [!summary] Obiettivi
> 1. Capire **perché sono nati i database NoSQL** e quali limiti del modello relazionale affrontano.
> 2. Conoscere le **quattro categorie** classiche e i criteri per scegliere tra loro.
> 3. Comprendere i compromessi tra **consistenza, disponibilità e latenza** (CAP, PACELC).
> 4. Avere uno sguardo sull'evoluzione attuale: **NewSQL**, **multi-modello**, **database vettoriali**.

---

## 1. Big Data e limiti del modello relazionale

### Le 3V dei Big Data
Dataset **massivi e complessi**, caratterizzati da:

| **Volume** | **Velocità** | **Varietà** |
|---|---|---|
| Quantità di dati | Rapidità di generazione | Eterogeneità dei formati |

> I DBMS relazionali **non sono pensati** per i Big Data.

### Esempi

| | Volume | Record | Compiti |
|---|---|---|---|
| **1. Sensori in una smart city** | ~10⁶ dati al minuto (miliardi al giorno) | UserID, timestamp, dato dipendente dal sensore (temperatura, presenza, azione, evento…) | Rilevare sequenze di eventi; analizzare storici ed estrarre statistiche |
| **2. Messaggi su social network** | Migliaia al secondo, centinaia di milioni al giorno, centinaia di miliardi all'anno | UserID, timestamp, testo, immagini, info aggiuntive… | Aggiungere un messaggio, modificarne lo stato, recuperarlo per criterio di ricerca, hashtag… |
| **3. Connessioni in un social network** | — | Amicizia/follower `<UserID1, UserID2>`; utente: UserID, nome, foto, profilo… | Trovare i follower dei follower dei follower… di un utente (es. tutti i follower "a cascata" di Don Rodrigo) |

### Perché il modello relazionale non è ottimale

| Modello relazionale | Big Data |
|---|---|
| Basato su **valori e tipi di dato** fissati | Dati **eterogenei e flessibili** |
| Interrogazioni basate su **JOIN** | JOIN **troppo costosi** su questi volumi |
| **Sintassi rigida** delle query | Servono interrogazioni "stile Google" |
| Grande impegno per **affidabilità e concorrenza** | Non sempre cruciali per queste applicazioni |

### Requisiti per i Big Data
- Evitare complessità non necessaria per ottenere **scalabilità**.
- **Schemi semplici** (o nessuno schema).
- **Sacrificare l'affidabilità** in cambio di prestazioni.
- Strutture dati più vicine alla **programmazione** che alla progettazione di basi di dati.

> *"Più codice, meno progettazione del DB."*

---

## 2. I sistemi NoSQL

### Che cos'è NoSQL
Una famiglia di DBMS che **non usa il modello relazionale** (non si basa su tabelle) e adotta modelli logici diversi: coppie `<chiave, valore>`, **documenti** (JSON, XML…), **grafi**, **colonne**.

> [!note] NoSQL ≠ "no SQL"
> È l'abbreviazione storica di **"Not only SQL"**: non rifiuta SQL, ma alcuni vincoli del modello relazionale classico.
> - Il nome descrive una famiglia di **modelli di dati**, non un linguaggio di interrogazione.
> - Molti sistemi NoSQL offrono comunque linguaggi **SQL-like**.
> - La vera distinzione è tra il modello relazionale **a tabelle fisse** e modelli **più flessibili**.

### ACID vs BASE
L'approccio **BASE** rilassa le proprietà ACID:
1. **B**asically **A**vailable — il DBMS funziona "praticamente" sempre;
2. **S**oft state — non deve essere consistente in ogni istante;
3. **E**ventually consistent — il sistema diventa *infine* consistente quando gli aggiornamenti si propagano.

### Le quattro categorie

| # | Categoria | Modello |
|---|---|---|
| 1 | **Key-value store** | Coppie `<chiave, valore>` |
| 2 | **Wide-column store** | Famiglie di colonne per riga |
| 3 | **Document store** | Documenti semistrutturati |
| 4 | **Graph database** | Nodi, archi e proprietà |

---

### 2.1 Key-value store

- **Modello dati**: coppie `<chiave, valore>` (più un timestamp).
- I valori possono contenere **qualsiasi tipo di dato** (nessuno schema).
- **Nessuna interrogazione sui valori**: solo sulle chiavi.

| In-memory | Su disco |
|---|---|
| Tutti i dati in memoria (a volte con durabilità opzionale) | Dati scritti stabilmente su disco |

**Casi d'uso**
- Informazioni di **sessione**.
- **Profili utente e preferenze** (userID → lingua, colore, fuso orario…).
- Dati del **carrello**, associati alla chiave userID.
- **Risultati intermedi** di task computazionalmente intensivi (specie negli store in-memory).

*Es.: Facebook usa **Memcached** per algoritmi ad alta intensità di dati; LinkedIn usa **Venice** per dati derivati (suggerimenti, offerte…).*

---

### 2.2 Wide-column store

La maggior parte deriva da **Google BigTable**:
- ogni riga ha diverse **column family**;
- le column family sono insiemi di coppie `<chiave, valore>` con **timestamp**;
- righe diverse possono avere column family diverse.

> Nei DBMS relazionali i dati sono di norma salvati **riga per riga**, nei wide-column store **colonna per colonna**: una differenza fisica con un impatto enorme sulle prestazioni (cfr. [[04 - Architettura fisica dei DBMS#1.5 Row store vs column store|row vs column store]]).

**Mattoni del modello**: *column* = `(name, value)`; *column family* = row key + insieme di column.
- **Schema-less** all'interno della famiglia: righe diverse possono avere colonne diverse.
- **Dati sparsi**: se una colonna non ha valore per una riga, **non viene salvata** (niente `NULL`).
- **Timestamped**: ogni valore ha una versione, utile per risolvere i conflitti nei sistemi distribuiti.

**Esempio — catalogo prodotti**

```
prodotto:101
 ├─ anagrafica: { titolo: "1984", autore: "Orwell", anno: "1949", prezzo: "9.90" }
 └─ libro:      { pagine: "328", genere: "Distopico" }

prodotto:450
 └─ anagrafica: { titolo: "Penna BIC", prezzo: "1.20" }      (fine della riga)
```
Due righe, due strutture: in un column store non è un'eccezione, è la norma.

**Casi d'uso**: serie temporali (time-series), dati da sensori, analisi dei social media.

> [!warning] Aggregazioni
> Nella maggior parte dei wide-column store NoSQL (es. **Cassandra**, **HBase**) le aggregazioni (`SUM`, `AVG`) sono **assenti o limitate**: vanno fatte a livello applicativo.
> I **column store relazionali** per l'analisi (es. **ClickHouse**, **Amazon Redshift**, **Google BigQuery**) sono invece progettati proprio per eseguire aggregazioni in modo efficiente, con SQL standard.

---

### 2.3 Document store

- **Modello dati**: coppie `<chiave, documento>`.
- **Documento**: JSON, XML o altri formati semistrutturati.
- I documenti **non hanno uno schema fisso**.
- Interrogazioni **anche sul contenuto** dei documenti.

**Esempi di documenti**
```json
{"nome": "Mario", "cognome": "Rossi"}
```
```json
{"dipendenti": [
  {"nome": "Mario", "cognome": "Rossi"},
  {"nome": "Anna",  "cognome": "Bianchi"},
  {"nome": "Piero", "cognome": "Verdi"}
]}
```

**Query aggregata in MongoDB** — quartieri con più di 5 ristoranti brasiliani:
```javascript
db.restaurants.aggregate([
  { $match: { "cuisine": "Brazilian" } },
  { $group: { "_id": "$borough", "count": { $sum: 1 } } },
  { $match: { "count": { $gt: 5 } } }
]);
```

> [!example] Esercizio — da tabelle a documento
> Schema relazionale: `Clienti(ID, Nome, Cognome, Email)`, `Ordini(ID, ClienteID → Clienti.ID, Data, Totale)`.
> Come progettare un documento JSON che rappresenti un cliente con i suoi ordini, senza JOIN? Vantaggi e svantaggi?
>
> **Soluzione**
> ```json
> {
>   "clienteId": "C001", "nome": "Mario", "cognome": "Rossi",
>   "email": "mario.rossi@email.it",
>   "ordini": [
>     {"ordineId": "O1001", "data": "2026-03-02", "totale": 59.90},
>     {"ordineId": "O1002", "data": "2026-04-18", "totale": 124.50}
>   ]
> }
> ```
> - ✅ Il **JOIN diventa annidamento**: una sola lettura recupera cliente e ordini.
> - ❌ I documenti possono diventare **molto grandi**; gli stessi dati possono essere **duplicati** in più documenti, e una modifica richiede di aggiornarli tutti.

^es-documento

**Casi d'uso**: dati semistrutturati (es. post di un blog, con pochi campi fissi e molte voci variabili), event logging, web analytics, analisi in tempo reale, e-commerce.

---

### 2.4 Graph database

**Modello dati**: grafi i cui **nodi** e **archi** hanno **proprietà** rappresentate come coppie chiave-valore.

**Esempio** — *"Quali amici di Marco hanno superato un corso tenuto dal docente Rossi, e con quale voto?"*

In **Cypher** (linguaggio di Neo4j):
```cypher
MATCH (m:Studente {nome: "Marco"})-[:AMICO_DI]->(a:Studente)
      -[s:HA_SUPERATO]->(c:Corso)<-[:INSEGNA]-(d:Docente {nome: "Rossi"})
RETURN a.nome AS amico, c.titolo AS corso, s.voto AS voto
```

| amico | corso | voto |
|---|---|---|
| "Anna" | "Basi di Dati" | 30 |

La stessa query in **SQL** richiede una catena di sei JOIN:
```sql
SELECT S2.nome AS amico, C.titolo AS corso, E.voto
FROM Studente S1
JOIN Amicizia     A  ON A.studente   = S1.matricola
JOIN Studente     S2 ON S2.matricola = A.studente2
JOIN Esame        E  ON E.studente   = S2.matricola
JOIN Corso        C  ON C.codice     = E.corso
JOIN Insegnamento I  ON I.corso      = C.codice
JOIN Docente      D  ON D.codice     = I.docente
WHERE S1.nome = 'Marco' AND D.nome = 'Rossi';
```

**Casi d'uso**: tutte le applicazioni basate su grafi — social network, reti biologiche, reti narrative, link analysis.

---

### 2.5 Confronto e scelta

**Dagli esempi alle categorie**

| Esempio | Categoria più adatta | Perché |
|---|---|---|
| Sensori smart city | Wide-column | Scritture ad altissimo volume, serie temporali |
| Messaggi social | Document / key-value | Record semistrutturati, accesso quasi sempre per chiave |
| Grafo social network | Graph | La query stessa è una ricerca nel grafo |

^es-esempi-categorie

**Panorama dei prodotti e confronto**

| Categoria | Modello | Caso d'uso ideale | Prodotti |
|---|---|---|---|
| **Key-value** | Coppie chiave-valore, nessuno schema | Sessioni, cache, carrelli | Redis, Memcached, Riak, Amazon DynamoDB |
| **Wide-column** | Famiglie di colonne (da BigTable) | Serie temporali, sensori | Apache Cassandra, Apache HBase, Google BigTable |
| **Document** | Documenti JSON/XML, schema flessibile | Contenuti semistrutturati, cataloghi | MongoDB, Apache CouchDB, Amazon DocumentDB |
| **Graph** | Nodi e archi con proprietà | Relazioni complesse, raccomandazioni | Neo4j, JanusGraph, Amazon Neptune |

> [!example]- Esercizio — scegli la categoria giusta
> | Scenario | Categoria | Perché |
> |---|---|---|
> | Catalogo e-commerce con attributi molto variabili tra categorie | **Document** | Schema flessibile per attributi variabili |
> | Cache di sessione per un sito ad alto traffico | **Key-value** | Accesso immediato per chiave di sessione |
> | Dati da sensori IoT campionati ogni secondo, analizzati come serie temporale | **Wide-column** | Scritture ad alto volume, serie temporali |
> | Rete di amicizie per suggerire "persone che potresti conoscere" | **Graph** | Query di traversata sulle relazioni |

^es-categorie

---

## 3. DBMS distribuiti

### Scalabilità verticale vs orizzontale

| **Verticale** (*scale up*) | **Orizzontale** (*scale out*) |
|---|---|
| Più potenza (CPU, RAM…) alla macchina esistente | Più macchine, tramite **distribuzione e parallelismo** |
| Scalabilità **limitata** | Carico distribuito su **migliaia di server economici** organizzati in rack |

Con la scalabilità orizzontale è però **difficile garantire insieme consistenza e affidabilità** → teorema CAP.

### Proprietà CAP

| | Proprietà | Significato |
|---|---|---|
| **C** | **Consistency** | In ogni istante, tutti i nodi vedono gli stessi dati |
| **A** | **Availability** | Il sistema risponde sempre alle richieste |
| **P** | **Partition tolerance** | Il sistema continua a funzionare anche se la comunicazione tra server è inaffidabile o un nodo si guasta |

> [!theorem] Teorema CAP (Eric Brewer, 1999)
> **Non è possibile soddisfare contemporaneamente tutte e tre** le proprietà CAP in un sistema distribuito.
> - Se il sistema deve essere tollerante alle partizioni (P), bisogna **scegliere tra C e A**.
> - C può essere sostituita da **eventual consistency**: le repliche convergono allo stesso stato quando la connettività viene ripristinata.

**Interpretazione** — una base di dati $S$ partizionata in due gruppi di nodi $S_1$, $S_2$ che non comunicano:
- se **rifiuta tutte le scritture** sia in $S_1$ sia in $S_2$ → **consistente ma non disponibile**;
- se $S_1$ e $S_2$ **accettano scritture** → **disponibile ma potenzialmente inconsistente**.

**Il compromesso — due proprietà su tre**

| Scelta | Comportamento |
|---|---|
| **AP** | Una replica eroga servizio anche durante una partizione, ma può essere inconsistente |
| **CP** | Sempre consistente, anche in partizione, ma una replica può negare il servizio |
| **CA** | Disponibile e consistente, **a meno che** non ci sia una partizione |

### Oltre CAP: il teorema PACELC (Daniel Abadi, 2010)
Estende CAP: **anche senza partizioni** un sistema distribuito deve scegliere tra **latenza** e **consistenza**.

> **if P**artition → scegli tra **A**vailability e **C**onsistency; **E**lse → scegli tra **L**atency e **C**onsistency.

| Classificazione | Significato | Esempi |
|---|---|---|
| **PA/EL** | In partizione sceglie A, altrimenti bassa latenza | Cassandra, DynamoDB |
| **PC/EC** | In partizione sceglie C, altrimenti consistenza forte | HBase, MongoDB (configurazione tipica) |

> [!example]- Esercizio — CAP e PACELC per una banca
> Una banca gestisce i conti su due data center (Milano e Francoforte) che replicano i dati in tempo reale. Un guasto di rete li separa per alcuni minuti.
> 1. Durante il guasto, C o A? → **C**: meglio rifiutare temporaneamente un'operazione che rischiare un **doppio prelievo** da due data center disallineati. Il sistema è **CP**.
> 2. In condizioni normali, bassa latenza o consistenza forte? → ancora **C** (**PC/EC**): il costo di un errore sul saldo supera quello di qualche millisecondo in più.
>
> *Contrappunto*: un sistema di raccomandazioni e-commerce farebbe la scelta opposta (**PA/EL**): meglio dati leggermente obsoleti che un'esperienza utente rallentata.

^es-cap

---

## 4. Limiti, sfide ed evoluzioni del NoSQL

### Limiti

**Mancanza di standard nelle query** — grande varietà di linguaggi, interrogazioni **non portabili**:

| Categoria | Linguaggio di interrogazione |
|---|---|
| Key-value | Nessuno (`put`, `get`, `update`, `delete`) |
| Wide-column | Programmatico, o SQL-like (Apache Hive, Pig Latin…) |
| Document | Proprietario, orientato ai documenti |
| Graph | Cypher, altri linguaggi proprietari |

**Consistenza e affidabilità** — se non le garantisce il DBMS, deve farlo l'**applicazione**:
- lo sforzo si sposta da "SQL" a "programmazione";
- gestire conflitti e consistenza eventuale ricade sul codice, con più complessità e rischio di bug;
- i requisiti di business cambiano: una scelta adeguata oggi potrebbe non bastare domani.

**Complessità operativa** — spesso si gestiscono **più sistemi specializzati** invece di un unico DBMS:
- **polyglot persistence**: key-value per le sessioni, document per il catalogo, graph per le raccomandazioni…;
- più sistemi da installare, monitorare, aggiornare e proteggere;
- dati duplicati/sincronizzati tra sistemi → nuovi rischi di inconsistenza.

**Scalabilità vs funzionalità** — i NoSQL scalano orizzontalmente ma spesso sacrificano **query complesse** e **transazioni multi-record**. Anche i relazionali possono scalare (es. DBMS **main-memory**). Non esiste una soluzione universalmente migliore.

### Evoluzioni recenti

**NewSQL** — scalabilità orizzontale del NoSQL **più** transazioni ACID e SQL standard:
- **Google Spanner**: database globalmente distribuito, consistenza forte tramite orologi atomici e GPS (*TrueTime*);
- **CockroachDB**: ispirato a Spanner, open source, resiliente al guasto di singoli nodi;
- **TiDB**: compatibile con il protocollo MySQL, separa storage e calcolo.

**HTAP** (*Hybrid Transactional/Analytical Processing*) — transazioni operative **e** analisi su grandi volumi nello stesso sistema:
- tradizionalmente: un DBMS **OLTP** (*Online Transaction Processing*) per le transazioni e un **data warehouse** separato per l'analisi (**OLAP**, *Online Analytical Processing*), con scambio di dati tra i due;
- HTAP elimina lo scambio: dati sempre aggiornati per entrambi gli usi;
- esempi: SAP HANA, TiDB, SingleStore.

> [!info] Data warehouse
> Sistema che analizza i dati per individuare tendenze, elaborare statistiche e supportare le decisioni (es. *quali prodotti hanno venduto di più negli ultimi 12 mesi? come variano le vendite tra estate e inverno?*).

**SQL su NoSQL** — livelli SQL-like per abbassare la curva di apprendimento:
- **CQL** (Cassandra Query Language);
- **Apache Phoenix**: motore SQL sopra HBase, compila le query in scansioni native;
- risultato: usabilità di SQL ma con i limiti del modello sottostante (niente JOIN arbitrari, transazioni multi-riga spesso limitate).

**Database multi-modello** — i confini tra categorie si sfumano:
- **PostgreSQL con JSONB**: colonne relazionali e documenti JSON nello stesso DB;
- **MongoDB**: anche ricerca testuale, vettoriale e geografica;
- **ArangoDB**, **Azure Cosmos DB**: documento, grafo e key-value nello stesso motore.

Vantaggio: **meno sistemi da gestire**, si attenua la complessità operativa.

> [!example]- Caso d'uso — NewSQL o NoSQL classico?
> Una fintech gestisce pagamenti tra milioni di utenti: ogni transazione deve essere **atomica e consistente** (niente doppio addebito), ma il sistema deve **scalare orizzontalmente** su più data center ed essere disponibile 24/7.
> - Un **key-value store puro non basta**: garantisce disponibilità e scalabilità, ma non **transazioni ACID multi-record** (addebitare A e accreditare B atomicamente).
> - Serve un **NewSQL** (Spanner, CockroachDB): transazioni distribuite ACID con scalabilità orizzontale — proprio il compromesso che i NoSQL classici avevano sacrificato.

^es-fintech

---

## 5. Uno sguardo al futuro (e al presente)

### AI generativa e database vettoriali

I modelli di AI generativa rappresentano testo, immagini e altri dati come **embedding**: vettori numerici ad alta dimensionalità che catturano il **significato**.

| Documento | Embedding (semplificato) |
|---|---|
| «Come cucinare la pasta» | [0.21, 0.85, 0.32] |
| «Ricetta per gli spaghetti» | [0.19, 0.82, 0.35] |
| «Motore di un'automobile» | [0.91, 0.12, 0.67] |

- Si cerca **per significato, non per uguaglianza**: i contenuti "più simili" a una query, non quelli che la contengono alla lettera.
- I DBMS tradizionali (relazionali e NoSQL classici) **non sono ottimizzati** per calcolare similarità tra milioni di vettori → nascono i **database vettoriali**.

**Database vettoriale**: indicizza embedding e risponde a query di **similarità** (anche approssimata) in millisecondi, anche su miliardi di vettori. Prodotti: **Pinecone**, **Weaviate**, **Qdrant**, **Milvus**.

> [!info] RAG — Retrieval-Augmented Generation
> Prima di rispondere, un **LLM recupera** i documenti più rilevanti da un database vettoriale (o altre fonti) e li usa come **contesto**. È oggi il modo più comune per dare a un LLM accesso a conoscenza aggiornata o privata **senza riaddestrarlo**.

**Integrazione vettoriale nei DB esistenti** (altro esempio di convergenza multi-modello):
- **MongoDB Atlas Vector Search**: indici vettoriali nello stesso DB documentale;
- **DynamoDB**, **Azure Cosmos DB**: ricerca vettoriale nativa;
- **PostgreSQL con pgvector**: colonne vettoriali accanto a tabelle relazionali.

### Tendenze in evoluzione
- **Interfacce in linguaggio naturale**: traducono domande libere in query SQL o NoSQL.
- **Database autonomi e self-tuning**: regolano automaticamente indici, partizionamento e risorse in base al carico.

Promettenti ma ancora da maturare: **strumenti di supporto**, non sostituti della progettazione.

### Infrastruttura

**Database serverless** — scala automaticamente (anche **a zero**) e si paga in base all'**uso effettivo**, senza gestire server o cluster.
- Ideale per carichi **imprevedibili**: applicazioni event-driven, agenti software, prototipi.
- Esempi: DynamoDB on-demand, Azure Cosmos DB serverless, Google Firestore.
- ❌ Meno controllo fine su prestazioni e costi rispetto a un cluster dedicato.

**Cloud-native e multi-cloud**
- La maggior parte dei NoSQL moderni gira nativamente su infrastrutture cloud (Kubernetes, storage a oggetti, reti distribuite).
- **Multi-cloud**: stessi dati su più provider per resilienza o requisiti normativi.
- Rischio di **vendor lock-in**: le API proprietarie rendono costoso cambiare provider; le alternative open source (a partire da Kubernetes) lo riducono ma non lo eliminano.

**Edge computing** — database che girano **sul dispositivo** (smartphone, sensore, auto) e si sincronizzano col cloud quando possibile (es. **Couchbase Lite**, **SQLite** con estensioni di sync). Sfida principale: la **conflict resolution** (stesso dato modificato offline su due dispositivi).

### Il paradigma publish/subscribe

1. **Publisher**: produttori di informazioni (quotazioni di borsa, feed di notizie…).
2. **Subscriber**: consumatori di informazioni.
3. **Broker**: identificano gli eventi che corrispondono agli interessi dei consumatori.

Primitive: `publish()`, `subscribe()`, `unsubscribe()`, `notify()`.

Varianti di middleware: centralizzato vs distribuito, persistente (DB) vs transitorio, basato su **topic** vs su **contenuto**, affidabile vs best-effort…

**Vantaggi (disaccoppiamento)**
- **Spazio**: produttori e consumatori non devono conoscersi.
- **Tempo**: non devono essere attivi nello stesso momento.
- **Sincronizzazione**: i produttori non si bloccano, i consumatori ricevono in modo asincrono.
- **Indirizzamento indiretto**, basato su dati e sottoscrizioni.

**Su larga scala** (es. **Apache Kafka**):
```
          Produttori (App, App, App)
                    │
 Connettori ──► Kafka Cluster ◄──► Elaborazione (App)
 (DB, DB)           │
          Consumatori (App, App, App)
```

### Data lake

> [!definition] Data lake
> Repository **centralizzato** che conserva **dati grezzi**, in qualunque formato e a qualunque scala, così come arrivano, insieme a metadati, catalogazione e controllo degli accessi.

1. **Schema-on-read** (non *schema-on-write*): la struttura si applica quando i dati vengono **letti** e analizzati.
2. **Dati eterogenei** nel formato nativo: log, JSON, CSV, immagini, video… senza trasformazioni preventive.
3. **Storage a oggetti**, economico e scalabile: Amazon S3, Azure Data Lake Storage…

| | **Data lake** | **Data warehouse** |
|---|---|---|
| Dati | Grezzi, eterogenei | Strutturati, puliti |
| Schema | On-read | On-write |
| Scopo tipico | Data science, ML, esplorazione | Business intelligence, reportistica |
| Utenti | Data engineer, data scientist | Analisti di business |

**Evoluzione**: il **data lakehouse** combina i vantaggi di entrambi (Databricks, Snowflake, Dremio…).

---

## 6. Conclusioni

### Dal relazionale, al NoSQL puro, ai sistemi ibridi
Dalle quattro categorie NoSQL "pure" di un decennio fa si è passati a un ecosistema di sistemi **ibridi e specializzati**:
- **NewSQL** per chi ha bisogno di ACID **e** scala;
- **multi-modello** per ridurre la complessità della polyglot persistence;
- **database vettoriali** per le applicazioni basate su AI.

### Il futuro è la convergenza
- I confini tra "relazionale" e "NoSQL" si stanno sfumando.
- La domanda non è più *"quale categoria?"*, ma *"quali garanzie servono a questa applicazione, e quale sistema le offre al minor costo operativo?"*

> I prodotti cambiano, i principi — **CAP, PACELC, ACID/BASE** — restano.

### Relazionale o NoSQL? Il criterio pratico

| Se l'applicazione richiede… | Scegli… |
|---|---|
| Dati strutturati, relazioni complesse, transazioni multi-tabella | **DBMS relazionale** |
| Scala massiccia, schema flessibile, accesso per chiave singola | **NoSQL classico** |
| ACID e scala orizzontale insieme | **NewSQL** |
| Ricerca per similarità su embedding | **Database vettoriale** |

### Sfide aperte
- **Standardizzazione**: ancora nessun linguaggio di interrogazione universale per NoSQL.
- **Portabilità**: migrare tra sistemi NoSQL richiede spesso di riscrivere la logica applicativa.
- **Sicurezza e governance**: dati distribuiti su più sistemi e cloud, più superfici da proteggere.
- **Complessità crescente**: architetture sempre più eterogenee richiedono competenze più ampie.

---

## Tabella riassuntiva per gli esercizi

### Dalla richiesta al sistema

| Nella richiesta compare… | Scelta | Perché |
|---|---|---|
| Accesso per chiave, sessioni, cache, carrello, preferenze | **Key-value** | Lettura/scrittura immediata per chiave, nessuno schema |
| Attributi molto variabili, oggetti annidati, cataloghi, post, log di eventi | **Document** | Schema flessibile, query anche sul contenuto |
| Serie temporali, sensori, scritture ad altissimo volume | **Wide-column** | Righe sparse con column family diverse, scritture massive |
| Relazioni, "amici di amici", traversate, raccomandazioni | **Graph** | La query è una visita del grafo (evita catene di JOIN) |
| Dati strutturati, relazioni complesse, transazioni multi-tabella | **Relazionale** | ACID, JOIN, SQL standard |
| ACID **e** scala orizzontale insieme (pagamenti globali) | **NewSQL** | Transazioni distribuite ACID (Spanner, CockroachDB) |
| Transazioni operative e analisi sugli stessi dati aggiornati | **HTAP** | Niente scambio fra sistemi OLTP e OLAP |
| Ricerca "per significato", similarità, RAG | **Database vettoriale** | Indicizza embedding, query di similarità |
| Dati grezzi eterogenei per data science / ML | **Data lake** | Schema-on-read, storage a oggetti |
| Reportistica, business intelligence | **Data warehouse** | Dati puliti, schema-on-write |
| Carico imprevedibile, prototipi | **Serverless** | Scala anche a zero, pagamento a consumo |

### CAP e PACELC

| Se nello scenario… | Durante una partizione | In assenza di partizioni | Classe | Esempi |
|---|---|---|---|---|
| Un errore sui dati è **costoso** (saldi, pagamenti, prenotazioni) | **C**: meglio rifiutare l'operazione | **C**: consistenza forte | **CP · PC/EC** | HBase, MongoDB (config. tipica) |
| Conta la **reattività**, dati un po' obsoleti sono accettabili (raccomandazioni, feed) | **A**: rispondere comunque | **L**: bassa latenza | **AP · PA/EL** | Cassandra, DynamoDB |

### ACID vs BASE

| ACID | BASE |
|---|---|
| **A**tomicità, **C**onsistenza, **I**solamento, **D**urability | **B**asically **A**vailable, **S**oft state, **E**ventually consistent |
| Consistenza in ogni istante | Consistenza *alla fine*, quando gli aggiornamenti si propagano |
| DBMS relazionali, NewSQL | NoSQL classici |

## Esercizi della lezione

| # | Esercizio | Argomento | Dove |
|---|---|---|---|
| 1 | Sensori, messaggi e social graph: quale categoria per ciascuno? | Categorie NoSQL | [[#^es-esempi-categorie]] |
| 2 | Da tabelle `Clienti`/`Ordini` a documento JSON | Document store | [[#^es-documento]] |
| 3 | Scegli la categoria giusta (4 scenari) | Categorie NoSQL | [[#^es-categorie]] |
| 4 | Banca su due data center: C o A? | CAP e PACELC | [[#^es-cap]] |
| 5 | Fintech: NewSQL o NoSQL classico? | NewSQL | [[#^es-fintech]] |
