`ALTER TABLE` (DDL) modifica lo schema di una tabella già esistente, anche se contiene dati. Per eliminare tabelle, schemi e domini vedi [[Eliminazione di oggetti (DROP)]].

``` sql
ALTER TABLE [schema.]NomeTabella Azione [, Azione ...];
```

Si possono eseguire **più azioni in un solo comando**, separate da virgola:

``` sql
ALTER TABLE segreteria.corsi
    ADD COLUMN crediti SMALLINT DEFAULT 6,
    ADD CONSTRAINT chk_crediti CHECK (crediti > 0);
```

### Riepilogo delle azioni

| Obiettivo | Sintassi |
| --- | --- |
| Aggiungere una colonna | `ADD [COLUMN] nome tipo [DEFAULT v] [vincoli]` |
| Eliminare una colonna | `DROP [COLUMN] nome [RESTRICT \| CASCADE]` |
| Impostare / togliere il default | `ALTER [COLUMN] nome SET DEFAULT v` / `DROP DEFAULT` |
| Rendere obbligatoria / facoltativa | `ALTER [COLUMN] nome SET NOT NULL` / `DROP NOT NULL` |
| Aggiungere un vincolo | `ADD CONSTRAINT nome <CHECK (...) \| UNIQUE (...) \| PRIMARY KEY (...) \| FOREIGN KEY ...>` |
| Eliminare un vincolo | `DROP CONSTRAINT nome [RESTRICT \| CASCADE]` |
| *Extra PostgreSQL*: cambiare tipo | `ALTER [COLUMN] nome TYPE nuovo_tipo [USING espressione]` |
| *Extra PostgreSQL*: rinominare | `RENAME COLUMN vecchio TO nuovo` / `RENAME TO nuovo_nome_tabella` |

### 1. Aggiungere chiavi esterne

Quando creazione delle tabelle e definizione delle relazioni sono separate (es. `alter1.sql`), o in caso di riferimenti circolari, le FK si aggiungono dopo:

``` sql
ALTER TABLE [schema.]tabella_figlia
    ADD CONSTRAINT nome_vincolo
    FOREIGN KEY (colonna_figlia)
    REFERENCES [schema.]tabella_padre (colonna_padre)
    ON DELETE azione
    ON UPDATE azione;
```

``` sql
ALTER TABLE segreteria.iscrizioni
    ADD CONSTRAINT fk_iscrizioni_studenti
    FOREIGN KEY (id_studente) REFERENCES segreteria.studenti (matricola)
    ON DELETE RESTRICT
    ON UPDATE CASCADE;

-- Riferimento circolare: impiegati ↔ dipartimenti
CREATE TABLE dipartimenti (cod_dip CHAR(3) PRIMARY KEY, direttore CHAR(6));
CREATE TABLE impiegati (matricola CHAR(6) PRIMARY KEY,
                        cod_dip CHAR(3) REFERENCES dipartimenti);
ALTER TABLE dipartimenti
    ADD CONSTRAINT fk_direttore FOREIGN KEY (direttore) REFERENCES impiegati(matricola);
```

Politiche di reazione (`CASCADE`, `SET NULL`, `RESTRICT`, …) → [[Vincoli di integrità referenziale]].

### 2. Valore di default

``` sql
ALTER TABLE segreteria.studenti ALTER COLUMN anno_corso SET DEFAULT 3;
ALTER TABLE impiegato ALTER stipendio DROP DEFAULT;     -- COLUMN è facoltativo
ALTER TABLE progetto ALTER budget SET DEFAULT 1000;
```

- Vale **solo per i futuri inserimenti**: le righe esistenti **non** vengono modificate.
- `DROP DEFAULT` riporta il default a `NULL`.

### 3. Aggiungere una colonna

``` sql
ALTER TABLE segreteria.docenti ADD COLUMN num_telefono VARCHAR(15);   -- NULL su tutte le righe esistenti
ALTER TABLE segreteria.corsi   ADD COLUMN crediti SMALLINT DEFAULT 6;  -- 6 su tutte le righe esistenti
ALTER TABLE dipartimento       ADD COLUMN num_uffici NUMERIC(4);
```

La nuova colonna deve avere un valore in **ogni riga già presente**:
- **con** `DEFAULT` → le righe esistenti ricevono il default;
- **senza** `DEFAULT` → ricevono `NULL`, quindi `NOT NULL` **non** è ammesso se la tabella non è vuota:

``` sql
ALTER TABLE studenti ADD COLUMN email VARCHAR(50) NOT NULL;              -- ERRORE se ci sono righe
ALTER TABLE studenti ADD COLUMN email VARCHAR(50) NOT NULL DEFAULT '';  -- OK
```

In alternativa: aggiungere la colonna senza `NOT NULL`, riempirla con `UPDATE`, poi `ALTER COLUMN email SET NOT NULL`.

### 4. Aggiungere vincoli (`CHECK`, `UNIQUE`, `PRIMARY KEY`)

``` sql
ALTER TABLE esame ADD CONSTRAINT voto_ok CHECK (voto >= 18 AND voto <= 30);
ALTER TABLE segreteria.studenti ADD CONSTRAINT chk_data_nascita CHECK (data_nascita > '1900-01-01');
ALTER TABLE segreteria.studenti ADD CONSTRAINT unq_cf UNIQUE (codice_fiscale);
ALTER TABLE vasca ADD PRIMARY KEY (codicev);                    -- se la tabella non ne ha già una
ALTER TABLE presenza ADD CONSTRAINT pk_presenza PRIMARY KEY (codicevasca, codicepesce);
```

> ⚠️ Il nuovo vincolo deve essere **soddisfatto dai dati già presenti**: altrimenti il comando viene rifiutato e il vincolo **non** viene aggiunto.

| Vincolo aggiunto | Fallisce se nei dati esistenti… |
| --- | --- |
| `CHECK` | una riga rende la condizione `FALSE` (i `NULL` passano) |
| `UNIQUE` | ci sono valori duplicati (i `NULL` ripetuti sono ammessi) |
| `PRIMARY KEY` | ci sono duplicati **o** `NULL`, oppure esiste già una PK |
| `FOREIGN KEY` | un valore non `NULL` non esiste nella tabella riferita, oppure i tipi sono incompatibili |

### 5. `NOT NULL` su colonna esistente

``` sql
ALTER TABLE segreteria.studenti ALTER COLUMN data_nascita SET NOT NULL;   -- obbligatorio
ALTER TABLE segreteria.docenti  ALTER COLUMN indirizzo DROP NOT NULL;     -- facoltativo
```

- `SET NOT NULL` fallisce se la colonna contiene già dei `NULL` (*"la colonna … contiene valori null"*): prima `UPDATE ... SET col = valore WHERE col IS NULL`.
- `DROP NOT NULL` non può essere usato sulle colonne della `PRIMARY KEY`.
- `NOT NULL` si aggiunge con `ALTER COLUMN ... SET NOT NULL`. La forma `ADD CONSTRAINT nome NOT NULL colonna` esiste solo da PostgreSQL 18: non usarla se il tuo server è 15–17.

### 6. Rimuovere vincoli e colonne

``` sql
ALTER TABLE segreteria.studenti DROP CONSTRAINT chk_data_nascita;
ALTER TABLE impiegato DROP CONSTRAINT stip_ok CASCADE;
ALTER TABLE segreteria.docenti DROP COLUMN indirizzo;
ALTER TABLE impiegato DROP COLUMN premio CASCADE;     -- elimina anche vincoli/viste che la usano
```

- Un vincolo si elimina **per nome**: se non è stato nominato con `CONSTRAINT`, cercare il nome generato da PostgreSQL (`\d tabella`), es. `studenti_pkey`, `esami_voto_check`, `esami_studente_fkey`.
- `RESTRICT` (default) / `CASCADE`: vedi [[Eliminazione di oggetti (DROP)]].
- Eliminare la colonna della PK o di un `UNIQUE` riferito da una FK di un'altra tabella richiede `CASCADE` (che elimina la FK).

### 7. Extra PostgreSQL: tipo e nomi

Non sono nelle slide ma tornano utili negli esercizi per correggere uno schema:

``` sql
ALTER TABLE studenti ALTER COLUMN cognome TYPE VARCHAR(40);       -- allargare una stringa
ALTER TABLE t ALTER COLUMN codice TYPE INTEGER USING codice::INTEGER;   -- conversione esplicita
ALTER TABLE studenti RENAME COLUMN citta TO citta_residenza;
ALTER TABLE studenti RENAME TO allievi;
```

Ridurre un tipo (es. `VARCHAR(40)` → `VARCHAR(5)`) fallisce se qualche valore non ci sta.
