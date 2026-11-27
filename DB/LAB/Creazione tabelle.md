`CREATE TABLE` definisce lo schema di una tabella e ne crea un'istanza **vuota**. Chi la crea ha tutti i privilegi su di essa. Schemi e domini personalizzati sono in [[Domini e schemi]], le chiavi esterne in dettaglio in [[Vincoli di integrità referenziale]].

### 1. Sintassi generale

``` sql
CREATE TABLE [NomeSchema.]NomeTabella (
    NomeColonna Dominio [DEFAULT valore] [VincoliColonna],
    ...,
    NomeColonna Dominio [DEFAULT valore] [VincoliColonna],
    [VincoliTabella]
);
```

- Elementi separati da **virgola**, **nessuna virgola** dopo l'ultimo (altrimenti errore di sintassi).
- Senza prefisso la tabella finisce nello schema corrente (`public`): scrivere `segreteria.studenti`.
- I nomi non tra virgolette vengono convertiti in **minuscolo**: `CREATE TABLE Studenti` crea `studenti`. Con `"Studenti"` il nome resta maiuscolo e andrà sempre scritto tra virgolette: meglio evitarlo.
- Le **colonne** sono ordinate (come scritte); le **righe** non hanno un ordine.
- Le tabelle create con `CREATE TABLE` sono *tabelle di base* (memorizzate fisicamente), da distinguere dalle [[Viste]].

### 2. Tipi di dato

| **Categoria** | **Tipo** | **Descrizione** |
| --- | --- | --- |
| Interi | `SMALLINT` | 2 byte (da −32.768 a +32.767) |
| | `INTEGER` / `INT` | 4 byte (circa ±2 miliardi): la scelta abituale |
| | `BIGINT` | 8 byte |
| Decimali esatti | `DECIMAL(p, s)` = `NUMERIC(p, s)` | `p` cifre totali, di cui `s` decimali; ideale per importi (`DECIMAL(10,2)`) |
| Virgola mobile | `REAL` / `DOUBLE PRECISION` | 4 / 8 byte, approssimati |
| Stringhe | `CHAR(n)` | lunghezza fissa, completata con spazi |
| | `VARCHAR(n)` | lunghezza variabile, massimo `n` |
| | `TEXT` | lunghezza variabile illimitata (non standard) |
| Temporali | `DATE` | solo data, formato `'AAAA-MM-GG'` |
| | `TIME` | solo ora `'HH:MM:SS'` |
| | `TIMESTAMP` | data e ora |
| | `INTERVAL` | intervallo di tempo (es. `'2 days'`) |
| Booleani | `BOOLEAN` | `TRUE`, `FALSE` (+ `NULL` = `UNKNOWN`) |
| Auto-incremento | `SERIAL` / `BIGSERIAL` | intero generato automaticamente (chiavi artificiali) |
| Personalizzati | dominio (`CREATE DOMAIN`) | tipo base + default + vincoli |

Casi limite dei tipi (troncamenti, arrotondamenti, divisione intera…) → [[Domini e schemi#3. Tipi di dato - casi limite utili|Domini e schemi]].

### 3. Valori di default

``` sql
DEFAULT <GenericoValore | USER | NULL>
```

| Opzione | Significato | Esempio |
| --- | --- | --- |
| valore / espressione | costante o espressione compatibile con il tipo | `num_figli SMALLINT DEFAULT 0`, `data_ins DATE DEFAULT CURRENT_DATE` |
| `USER` | nome dell'utente che esegue l'inserimento | `inserito_da VARCHAR(20) DEFAULT USER` |
| `NULL` | default implicito se la clausola manca | — |

Il default si usa **solo se la colonna viene omessa** nell'`INSERT` (o se si scrive `DEFAULT` come valore). Un `NULL` scritto esplicitamente **non** viene sostituito dal default.

### 4. Vincoli intra-relazionali

| Vincolo        | Significato                                                               | `NULL`                                       | Quanti per tabella |
| -------------- | ------------------------------------------------------------------------- | -------------------------------------------- | ------------------ |
| `NOT NULL`     | valore obbligatorio                                                       | vietato                                      | uno per colonna    |
| `UNIQUE`       | nessuna coppia di righe con gli stessi valori (definisce una superchiave) | **ammessi, anche ripetuti**                  | quanti si vuole    |
| `PRIMARY KEY`  | chiave primaria = `UNIQUE` + `NOT NULL`                                   | vietato                                      | **una sola**       |
| `CHECK (cond)` | condizione vera per ogni riga                                             | **ammesso** (condizione `UNKNOWN` ≠ `FALSE`) | quanti si vuole    |

**A livello di colonna o di tabella?**

| Vincolo su… | Dove |
| --- | --- |
| una sola colonna | subito dopo la colonna (`matricola CHAR(6) PRIMARY KEY`) oppure in fondo |
| **più colonne** | **obbligatoriamente in fondo**, a livello di tabella (`PRIMARY KEY (studente, corso)`) |

``` sql
cognome   VARCHAR(20) NOT NULL,
cf        CHAR(16) NOT NULL UNIQUE,          -- chiave candidata alternativa alla PK
matricola CHAR(6) PRIMARY KEY,
voto      SMALLINT CHECK (voto BETWEEN 18 AND 30),
mansione  VARCHAR(20) CHECK (mansione IN ('dirigente', 'ingegnere', 'tecnico', 'segretario')),
...
UNIQUE (cognome, nome),
PRIMARY KEY (studente, corso),
CHECK (stipendio > premio)
```

⚠️ **`UNIQUE (cognome, nome)` ≠ `cognome UNIQUE, nome UNIQUE`**:
- `UNIQUE (cognome, nome)` vieta due righe con **stessa coppia** (ammessi "Sara Orru" e "Sara Pinna");
- `UNIQUE` sulle singole colonne vieta **lo stesso nome** o **lo stesso cognome** ripetuti (molto più restrittivo).

Lo stesso vale per `PRIMARY KEY (a, b)` vs dichiarare `PRIMARY KEY` su due colonne → la seconda forma è un **errore** (*"non è possibile avere più di una chiave primaria"*).

### 5. Vincoli inter-relazionali (chiavi esterne)

``` sql
docente INTEGER REFERENCES docenti(cod_docente)                -- colonna singola
FOREIGN KEY (provincia, numero) REFERENCES auto(provincia, numero) -- chiave composta
    ON DELETE <CASCADE | SET NULL | SET DEFAULT | RESTRICT | NO ACTION>
    ON UPDATE <CASCADE | SET NULL | SET DEFAULT | RESTRICT | NO ACTION>
```

- La tabella riferita deve **già esistere** e gli attributi riferiti devono essere `PRIMARY KEY` o `UNIQUE`.
- Default della politica: `NO ACTION`.
- Dettagli, politiche e casi limite → [[Vincoli di integrità referenziale]].

### 6. Nomi dei vincoli (`CONSTRAINT`)

``` sql
CONSTRAINT stip_ok CHECK (stipendio > premio),
CONSTRAINT unq_corso UNIQUE (nome, docente),
CONSTRAINT fk_esami_studenti FOREIGN KEY (studente) REFERENCES studenti(matricola)
```

Il nome compare nei messaggi d'errore e serve per rimuovere il vincolo con `ALTER TABLE ... DROP CONSTRAINT nome` (vedi [[Alter tabelle]]). Senza nome, PostgreSQL ne genera uno automatico (`tabella_colonna_check`, `tabella_pkey`, …).

### 7. Casi limite

- **`CHECK` e `NULL`**: `voto SMALLINT CHECK (voto >= 18)` accetta `NULL`. Se il voto è obbligatorio aggiungere `NOT NULL`.
- **`UNIQUE` e `NULL`**: con `UNIQUE (cf)` si possono inserire più righe con `cf = NULL`. Per una chiave candidata vera: `NOT NULL UNIQUE`.
- **Default che viola un `CHECK`**: `a INT DEFAULT 0 CHECK (a > 0)` → la tabella viene creata, ma ogni `INSERT` che omette `a` fallisce.
- **`CHECK` con sub-query**: non ammesso (*"non si può usare una sottoquery nel vincolo di controllo"*). I vincoli tra tabelle si esprimono con `FOREIGN KEY`.
- **`CHECK` su più colonne scritto inline**: le slide lo vogliono a livello di tabella; PostgreSQL lo accetta anche dopo una colonna, ma per chiarezza (e all'esame) va messo in fondo.
- **`SERIAL` e valori espliciti**: se si inseriscono a mano valori nella colonna `SERIAL`, il contatore non lo sa e i successivi inserimenti automatici possono dare **chiave duplicata**.
- **Ordine di creazione**: prima le tabelle **riferite**, poi quelle che contengono le FK (altrimenti *"la relazione … non esiste"*). Con riferimenti circolari: creare le tabelle e aggiungere una FK dopo con `ALTER TABLE`.
- **Tipo della FK**: deve essere compatibile con quello riferito (`INTEGER` verso `SERIAL`, `CHAR(6)` verso `CHAR(6)`).

## Esempi

### Esempio 1: tipi base, default e vincoli di colonna

``` sql
CREATE TABLE segreteria.aule (
    codice_aula CHAR(5) PRIMARY KEY,
    capienza    SMALLINT DEFAULT 30 CHECK (capienza > 0),
    edificio    VARCHAR(30) NOT NULL,
    piano       SMALLINT DEFAULT 0
);
```

- `PRIMARY KEY` su una colonna: implica `NOT NULL` e unicità.
- `capienza`: se omessa vale 30; deve essere positiva (ma `NULL` esplicito sarebbe accettato).

### Esempio 2: `UNIQUE` composto con nome

``` sql
CREATE TABLE segreteria.appelli (
    id_appello     INTEGER PRIMARY KEY,
    codice_materia INTEGER NOT NULL,
    data_appello   DATE NOT NULL,
    tipo_prova     VARCHAR(10) CHECK (tipo_prova IN ('scritto', 'orale')),
    CONSTRAINT unq_materia_data UNIQUE (codice_materia, data_appello)
);
```

Non possono esistere due appelli della stessa materia nello stesso giorno; la stessa materia in giorni diversi o materie diverse nello stesso giorno sono ammesse.

### Esempio 3: chiave primaria composta (tabella di associazione)

``` sql
CREATE TABLE segreteria.iscrizioni (
    id_studente     INTEGER REFERENCES segreteria.studenti(matricola),
    id_corso        INTEGER REFERENCES segreteria.corsi(codice),
    data_iscrizione DATE NOT NULL DEFAULT CURRENT_DATE,
    PRIMARY KEY (id_studente, id_corso)
);
```

Uno studente non può iscriversi due volte allo stesso corso; nessuno dei due campi può essere `NULL`.

### Esempio 4: `CHECK` su più colonne

``` sql
CREATE TABLE segreteria.borse_studio (
    id_borsa        INTEGER PRIMARY KEY,
    studente        INTEGER NOT NULL,
    data_inizio     DATE NOT NULL,
    data_fine       DATE NOT NULL,
    importo_mensile DECIMAL(8,2) DEFAULT 500.00,
    CONSTRAINT chk_date_valide CHECK (data_inizio < data_fine)
);
```

### Esempio 5: dominio personalizzato e FK con politiche

``` sql
CREATE TABLE segreteria.giudizi (
    id_giudizio     INTEGER PRIMARY KEY,
    codice_studente INTEGER NOT NULL,
    voto_finale     segreteria.votazione NOT NULL,   -- eredita tipo, default e CHECK del dominio
    FOREIGN KEY (codice_studente) REFERENCES segreteria.studenti(matricola)
        ON DELETE RESTRICT
        ON UPDATE CASCADE
);
```

### Esempio 6: schema completo (acquario, esercizio delle slide)

``` sql
CREATE TABLE pesce (
    codicep  SMALLINT PRIMARY KEY,
    nome     VARCHAR(20) NOT NULL,
    famiglia VARCHAR(20)
);

CREATE TABLE vasca (
    codicev   SMALLINT PRIMARY KEY,
    nomevasca VARCHAR(20),
    volume    SMALLINT CHECK (volume > 0),
    sala      VARCHAR(15)
);

CREATE TABLE presenza (
    codicevasca SMALLINT REFERENCES vasca(codicev) ON DELETE CASCADE,
    codicepesce SMALLINT REFERENCES pesce(codicep) ON DELETE RESTRICT,
    quantita    SMALLINT NOT NULL CHECK (quantita > 0),
    PRIMARY KEY (codicevasca, codicepesce)
);
```

Se una vasca viene eliminata spariscono anche le sue presenze; un tipo di pesce presente in qualche vasca non può essere eliminato.
