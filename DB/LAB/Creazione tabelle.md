Creazione di uno schema:
``` sql
CREATE SCHEMA nome_schema;
```

### Tipi di dati

| **Categoria**                        | **Tipo di Dato**                  | **Descrizione Breve**                                                                      |
| ------------------------------------ | --------------------------------- | ------------------------------------------------------------------------------------------ |
| **Numerici interi**                  | `SMALLINT`                        | Intero a 2 byte (da -32.768 a +32.767)                                                     |
|                                      | `INTEGER` / `INT`                 | Intero standard a 4 byte (da circa -2 miliardi a +2 miliardi)                              |
|                                      | `BIGINT`                          | Intero grande a 8 byte per valori molto elevati                                            |
| **Numerici esatti/decimali**         | `DECIMAL(p, s)` / `NUMERIC(p, s)` | Numero con virgola a precisione esatta ($p$ cifre totali, $s$ decimali); ideale per valute |
| **Numerici approssimati**            | `REAL` / `FLOAT`                  | Virgola mobile a precisione singola (4 byte)                                               |
|                                      | `DOUBLE PRECISION`                | Virgola mobile a precisione doppia (8 byte)                                                |
| **Carattere / Stringhe**             | `CHAR(n)`                         | Testo a lunghezza fissa di $n$ caratteri (riempito con spazi se più corto)                 |
|                                      | `VARCHAR(n)`                      | Testo a lunghezza variabile fino a un massimo di $n$ caratteri                             |
|                                      | `TEXT`                            | Testo a lunghezza variabile senza limite esplicito prefissato                              |
| **Temporali**                        | `DATE`                            | Memorizza solo la data (`AAAA-MM-GG`)                                                      |
|                                      | `TIME`                            | Memorizza solo l'orario (`HH:MM:SS`)                                                       |
|                                      | `TIMESTAMP`                       | Memorizza data e ora insieme                                                               |
|                                      | `INTERVAL`                        | Rappresenta un intervallo di tempo (es. 2 giorni, 3 ore)                                   |
| **Logici**                           | `BOOLEAN`                         | Valore booleano: ammette `TRUE`, `FALSE` e `UNKNOWN` (gestito tramite `NULL`)              |
| **Identificatori auto-incrementali** | `SERIAL` (PostgreSQL)             | Intero auto-incrementante gestito tramite sequenza (comune per ID primari)                 |
| **Definiti dall'utente**             | `DOMINI` (`CREATE DOMAIN`)        | Tipi personalizzati basati su un tipo predefinito con l'aggiunta di vincoli e/o default    |

### 1. Sintassi Generale

La struttura di base presentata nelle slide è la seguente:

``` sql
CREATE TABLE [NomeSchema.]NomeTabella (
    NomeColonna Dominio [ValoreDiDefault] [VincoliColonna],
    ...
    [VincoliTabella]
);
```

### 2. Specifica del Nome e dello Schema

- **Nome della tabella**: identifica la relazione.
- **Prefisso dello schema (`NomeSchema.NomeTabella`)**: permette di specificare esplicitamente a quale schema appartiene la tabella (es. `segreteria.studenti`). Se omesso, viene utilizzato lo schema predefinito della sessione (`search_path`).

### 3. Tipi di Dato / Domini delle Colonne

Ogni colonna deve avere un tipo di dato associato:

  

- **Tipi standard predefiniti**:
    - _Interi_: `SMALLINT`, `INTEGER`, `BIGINT`.
    - _Numeri esatti/decimali_: `DECIMAL(precisione, scala)` o `NUMERIC` (es. `DECIMAL(10,2)` per valute).
    - _Stringhe_:
        - `CHAR(n)`: stringa a lunghezza fissa di $n$ caratteri (eventualmente completata con spazi).
        - `VARCHAR(n)`: stringa a lunghezza variabile fino a $n$ caratteri.
    - _Temporali_: `DATE`, `TIME`, `TIMESTAMP`.
	
    - _Booleani_: `BOOLEAN` (`TRUE`, `FALSE`, `UNKNOWN`).

- **Domini personalizzati (_User-Defined Domains_)**: tipi creati precedentemente con `CREATE DOMAIN` (es. il dominio `votazione` o `Voto`).

### 4. Clausola del Valore di Default (`DEFAULT`)

Specifica il valore che la colonna assume automaticamente quando una nuova riga viene inserita senza fornire un valore per quell'attributo.

Sintassi:
``` sql
DEFAULT < GenericoValore | USER | NULL >
```

- **`GenericoValore`**: una costante numerica, stringa o un'espressione compatibile con il tipo (es. `DEFAULT 0`, `DEFAULT 1`, `DEFAULT 'Nessuno'`).
- **`USER`**: assegna come default il nome/identificativo dell'utente che esegue l'inserimento.  
- **`NULL`**: valore di default implicito se la clausola viene omessa (a meno che non sia specificato `NOT NULL`).


### 5. Vincoli di Integrità Intra-relazionali

I vincoli possono essere definiti **a livello di singola colonna** (immediatamente dopo il tipo/default) oppure **a livello di tabella** (in coda, separati da virgola). La definizione a livello di tabella è **obbligatoria** quando il vincolo coinvolge due o più colonne.

I vincoli intra-relazionali supportati sono:
#### a. `NOT NULL`

- Impedisce l'inserimento del valore `NULL` nella colonna.
- Si specifica solo a livello di colonna:
``` sql
Cognome VARCHAR(20) NOT NULL
```
    

#### b. `UNIQUE`

- Impone che tutti i valori nella colonna (o combinazione di colonne) siano distinti, ammettendo però i valori `NULL`.
- **Su singola colonna**:
``` sql
Matricola CHAR(6) UNIQUE
```
    
- **Su più colonne (livello di tabella)**: garantisce che la coppia/combinazione di valori non sia duplicata:
``` sql
UNIQUE (Cognome, Nome)
```

(Nota: una tabella può avere molteplici vincoli `UNIQUE`).


#### c. `PRIMARY KEY` (Chiave Primaria)

- Identifica la chiave primaria della tabella.
- **Proprietà**: implica automaticamente `NOT NULL` e `UNIQUE` per gli attributi coinvolti, e **può esserne definita soltanto una per tabella**.
- **Su colonna singola**:
``` sql
Matricola CHAR(6) PRIMARY KEY
```
    
- **Composta da più attributi (livello di tabella)**:
    

``` sql
PRIMARY KEY (studente, corso)
```

#### d. `CHECK (condizione)`

- Specifica una condizione booleana che ogni tupla deve soddisfare.
- **Su singolo attributo**:
``` sql
anno_corso SMALLINT CHECK (anno_corso >= 1 AND anno_corso <= 3)
```
    
- **Su più attributi contemporaneamente (livello di tabella)**:
``` sql
CHECK (Stipendio > Premio)
```

### 6. Vincoli di Integrità Inter-relazionali (`FOREIGN KEY`)

Definiscono il vincolo di chiave esterna (integrità referenziale) collegando una o più colonne a una chiave primaria o univoca di un'altra tabella.

  

- **A livello di colonna** (sintassi abbreviata per chiave singola):
``` sql
docente INTEGER REFERENCES docenti(cod_docente)
```

- **A livello di tabella** (sintassi esplicita, obbligatoria per chiavi composte):    
``` sql
FOREIGN KEY (docente) REFERENCES docenti(cod_docente)
```

- **Opzioni di reazione referenziale** (possono essere specificate direttamente o con `ALTER TABLE`):
    - `ON DELETE <RESTRICT ACTION CASCADE NO NULL SET |>`
    - `ON UPDATE <RESTRICT ACTION CASCADE NO NULL SET |>`      

### 7. Assegnazione di un Nome ai Vincoli (`CONSTRAINT`)

In qualsiasi vincolo di tabella (e di colonna), è possibile anteporre la clausola `CONSTRAINT NomeVincolo` per assegnare un identificativo univoco al vincolo.

- Risulta utile nei messaggi d'errore e permette di rimuovere o modificare il vincolo in seguito tramite `ALTER TABLE`:

``` sql
CONSTRAINT unq_corso UNIQUE (nome, docente),
CONSTRAINT chk_anni CHECK (anno_corso BETWEEN 1 AND 3)  
```

### 8. Note Strutturali

- **Ordinamento delle colonne**: in SQL l'ordine con cui le colonne vengono scritte in `CREATE TABLE` è preservato e significativo dal punto di vista dello schema.
- **Ordinamento delle righe**: le tuple memorizzate non hanno alcun ordinamento intrinseco.


## Esempi 

### Esempio 1: Tabella semplice con tipi base, default e vincoli a livello di colonna

In questo esempio vediamo come usare `PRIMARY KEY` singola, `NOT NULL`, un valore di `DEFAULT` e un `CHECK` direttamente sulla colonna:



``` sql
CREATE TABLE segreteria.aule (
    codice_aula CHAR(5) PRIMARY KEY,
    capienza SMALLINT DEFAULT 30 CHECK (capienza > 0),
    edificio VARCHAR(30) NOT NULL,
    piano SMALLINT DEFAULT 0
);
```

- **`codice_aula CHAR(5) PRIMARY KEY`**: chiave primaria su singolo attributo a lunghezza fissa (implica automaticamente `NOT NULL` e unicità).
- **`DEFAULT 30 CHECK (capienza > 0)`**: se non specificata, la capienza sarà 30, e il valore deve essere strettamente positivo.
- **`edificio VARCHAR(30) NOT NULL`**: stringa a lunghezza variabile che non può essere lasciata vuota/nulla.

### Esempio 2: Vincolo `UNIQUE` composto e nome del vincolo (`CONSTRAINT`)

Spesso l'unicità non riguarda una sola colonna, ma la **combinazione** di due o più colonne (ad esempio: non possono esistere due dipendenti con lo stesso cognome e nome, oppure due appelli dello stesso corso nello stesso giorno):

``` sql
CREATE TABLE segreteria.appelli (
    id_appello INTEGER PRIMARY KEY,
    codice_materia INTEGER NOT NULL,
    data_appello DATE NOT NULL,
    tipo_prova VARCHAR(10) CHECK (tipo_prova IN ('scritto', 'orale')),
    CONSTRAINT unq_materia_data UNIQUE (codice_materia, data_appello)
);
```

- **`CONSTRAINT unq_materia_data UNIQUE (...)`**: definisce a livello di tabella che la coppia `(codice_materia, data_appello)` non può ripetersi. Assegnare un nome con `CONSTRAINT` rende chiaro l'errore qualora il vincolo venisse violato.
- **`tipo_prova ... CHECK (... IN (...))`**: limita i valori ammissibili a una lista specifica.
### Esempio 3: Chiave primaria composta (`PRIMARY KEY` su più colonne)

Quando una tabella non ha un identificativo univoco singolo (come accade spesso nelle tabelle di associazione o presenza), la chiave primaria è formata da più attributi uniti:

``` sql
CREATE TABLE segreteria.iscrizioni (
    id_studente INTEGER,
    id_corso INTEGER,
    data_iscrizione DATE NOT NULL,
    PRIMARY KEY (id_studente, id_corso)
);
```

- **`PRIMARY KEY (id_studente, id_corso)`**: assicura che uno studente non possa essere iscritto due volte allo stesso corso e garantisce che nessuno dei due campi possa essere `NULL`.
### Esempio 4: Vincolo `CHECK` a livello di tabella (confronto tra più colonne)

Se una condizione di validità deve confrontare due o più colonne della stessa riga, deve essere dichiarata a livello di tabella:

``` sql
CREATE TABLE segreteria.borse_studio (
    id_borsa INTEGER PRIMARY KEY,
    studente INTEGER NOT NULL,
    data_inizio DATE NOT NULL,
    data_fine DATE NOT NULL,
    importo_mensile DECIMAL(8,2) DEFAULT 500.00,
    CONSTRAINT chk_date_valide CHECK (data_inizio < data_fine)
);
```

- **`CHECK (data_inizio < data_fine)`**: impedisce l'inserimento di righe dove la data di fine sia precedente a quella di inizio.
### Esempio 5: Uso di un dominio personalizzato e chiave esterna inline

Se è già stato creato un dominio (ad esempio `votazione`):

``` sql
CREATE TABLE segreteria.giudizi (
    id_giudizio INTEGER PRIMARY KEY,
    codice_studente INTEGER NOT NULL,
    voto_finale segreteria.votazione NOT NULL,
    FOREIGN KEY (codice_studente) REFERENCES segreteria.studenti(matricola)
        ON DELETE RESTRICT
        ON UPDATE CASCADE
);
```

- **`voto_finale segreteria.votazione`**: la colonna eredita automaticamente tutti i vincoli e i tipi impostati nella definizione del dominio `votazione`.
- **`FOREIGN KEY ... ON DELETE RESTRICT ON UPDATE CASCADE`**: collega la colonna alla tabella padre con le relative regole di reazione.