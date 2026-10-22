Per modificare la struttura di tabelle già esistenti in SQL si utilizza l'istruzione DDL **`ALTER TABLE`**.

### 1. Aggiungere Vincoli di Chiave Esterna (`FOREIGN KEY`)

Quando si separano la creazione delle tabelle e la definizione delle relazioni (come richiesto per `alter1.sql`), si aggiungono le chiavi esterne a posteriori specificando le regole di reazione referenziale:


- **Sintassi**:

``` sql
ALTER TABLE [schema.]tabella_figlia
    ADD CONSTRAINT nome_vincolo
    FOREIGN KEY (colonna_figlia) 
    REFERENCES [schema.]tabella_padre (colonna_padre)
    ON DELETE azione
    ON UPDATE azione;
```

- **Politiche di reazione principali**:
    - `ON DELETE RESTRICT` (oppure `NO ACTION`): blocca la cancellazione dalla tabella padre se esistono righe collegate nella tabella figlia.
    - `ON UPDATE CASCADE`: propaga automaticamente la modifica della chiave primaria a tutte le righe correlate della tabella figlia.   
- **Esempio pratico**:

``` sql
ALTER TABLE segreteria.iscrizioni
    ADD CONSTRAINT fk_iscrizioni_studenti
    FOREIGN KEY (id_studente) 
    REFERENCES segreteria.studenti (matricola)
    ON DELETE RESTRICT
    ON UPDATE CASCADE;
```

### 2. Modificare il Valore di Default di una Colonna

Permette di impostare, cambiare o rimuovere il valore predefinito assunto da una colonna durante gli inserimenti successivi.

- **Impostare o modificare un default**

``` sql
ALTER TABLE [schema.]nome_tabella
    ALTER COLUMN nome_colonna SET DEFAULT nuovo_valore;
```

- **Esempio pratico**:

``` sql
ALTER TABLE segreteria.studenti
    ALTER COLUMN anno_corso SET DEFAULT 3;
```

- **Rimuovere un default esistente**:

``` sql
ALTER TABLE [schema.]nome_tabella
    ALTER COLUMN nome_colonna DROP DEFAULT;
```

### 3. Aggiungere una Nuova Colonna (`ADD COLUMN`)

Permette di estendere lo schema di una tabella con un nuovo attributo, specificandone tipo ed eventuale valore di default.

  

- **Sintassi**:

``` sql
ALTER TABLE [schema.]nome_tabella
    ADD COLUMN nome_colonna tipo_di_dato [DEFAULT valore];
```

- **Esempi pratici**:

``` sql
-- Colonna con default NULL (comportamento standard)
ALTER TABLE segreteria.docenti
    ADD COLUMN num_telefono VARCHAR(15) DEFAULT NULL;

-- Colonna con default numerico
ALTER TABLE segreteria.corsi
    ADD COLUMN crediti SMALLINT DEFAULT 6;
```

### 4. Aggiungere un Vincolo di Controllo (`ADD CONSTRAINT ... CHECK`)

Consente di aggiungere una condizione booleana che tutte le tuple devono rispettare da quel momento in poi.

  

> **Attenzione**: se la tabella contiene già dei dati, l'operazione avrà successo **solo se tutte le righe già presenti rispettano la condizione**. Se anche una sola riga viola il vincolo, il comando restituirà un errore.
- **Sintassi**:

``` sql
ALTER TABLE [schema.]nome_tabella
    ADD CONSTRAINT nome_vincolo CHECK (condizione);
```

- **Esempio pratico**:

``` sql
ALTER TABLE segreteria.studenti
    ADD CONSTRAINT chk_data_nascita 
    CHECK (data_nascita > '1900-01-01');
```

### 5. Modificare il Vincolo `NOT NULL` su una Colonna Esistente

Se si vuole rendere un campo obbligatorio o facoltativo dopo la creazione della tabella:

  

- **Rendere un campo obbligatorio (`SET NOT NULL`)**:

``` sql
ALTER TABLE segreteria.studenti
    ALTER COLUMN data_nascita SET NOT NULL;
```

- **Rendere un campo facoltativo (`DROP NOT NULL`)**:

```sql
ALTER TABLE segreteria.docenti
    ALTER COLUMN indirizzo DROP NOT NULL;
```

### 6. Rimuovere Vincoli o Colonne (`DROP`)

Nel caso sia necessario eliminare un vincolo creato in precedenza o una colonna non più necessaria:

  

- **Rimuovere un vincolo tramite il suo nome**:

``` sql
ALTER TABLE segreteria.studenti
    DROP CONSTRAINT chk_data_nascita;
```

- **Rimuovere una colonna**:

```
ALTER TABLE segreteria.docenti
    DROP COLUMN indirizzo;
```