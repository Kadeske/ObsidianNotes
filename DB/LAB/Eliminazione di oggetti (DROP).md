### 1. `DROP` di oggetti di schema

``` sql
DROP <SCHEMA | DOMAIN | TABLE | VIEW> [IF EXISTS] NomeElemento [RESTRICT | CASCADE];
```

| Opzione | Effetto |
| --- | --- |
| `RESTRICT` (**default**) | il comando fallisce se altri oggetti dipendono dall'elemento |
| `CASCADE` | l'elemento viene rimosso **insieme a tutto ciò che ne dipende** (attenzione alle reazioni a catena) |
| `IF EXISTS` (PostgreSQL) | nessun errore se l'oggetto non esiste: utile in testa agli script da rieseguire |

``` sql
DROP TABLE dipartimento;                -- = RESTRICT
DROP TABLE dipartimento CASCADE;        -- rimuove anche le FK/viste che la usano
DROP SCHEMA azienda CASCADE;            -- rimuove lo schema e tutto il contenuto
DROP DOMAIN segreteria.voto CASCADE;    -- rimuove anche le colonne che usano il dominio!
DROP VIEW studenti_biennio;
DROP TABLE IF EXISTS segreteria.esami;
```

### 2. Cosa blocca un `DROP ... RESTRICT` (PostgreSQL)

| Oggetto | Bloccato se… |
| --- | --- |
| `TABLE` | una **FK** di un'altra tabella la riferisce, oppure una **vista** la usa |
| `SCHEMA` | contiene **qualsiasi** oggetto (anche una tabella vuota) |
| `DOMAIN` | è usato da almeno una colonna |
| `VIEW` | un'altra vista è definita su di essa |

> ⚠️ **Differenza slide / PostgreSQL**: le slide dicono che `DROP TABLE ... RESTRICT` fallisce se la tabella **contiene righe**. In PostgreSQL **non è così**: una tabella piena senza dipendenze viene eliminata anche con `RESTRICT`. Contano solo gli oggetti dipendenti.

**Cosa elimina `CASCADE` su una tabella riferita da FK**: elimina il **vincolo** di FK nella tabella figlia, **non** la tabella figlia né i suoi dati. Le viste che la usano invece vengono eliminate.

### 3. Ordine di eliminazione

Con `RESTRICT` bisogna eliminare prima chi dipende:

``` sql
DROP VIEW  esami_superati;   -- viste
DROP TABLE esami;            -- tabelle figlie (contengono le FK)
DROP TABLE studenti;         -- tabelle padre
DROP TABLE corsi;
DROP DOMAIN voto;            -- domini non più usati
DROP SCHEMA segreteria;      -- schema ormai vuoto
```

Oppure in un colpo solo: `DROP SCHEMA segreteria CASCADE;`.

### 4. Eliminare colonne e vincoli (`ALTER TABLE`)

``` sql
ALTER TABLE NomeTabella DROP [COLUMN] NomeAttributo [RESTRICT | CASCADE];
ALTER TABLE NomeTabella DROP CONSTRAINT NomeVincolo [RESTRICT | CASCADE];
```

``` sql
ALTER TABLE impiegato DROP COLUMN premio CASCADE;   -- elimina anche vincoli e viste che usano "premio"
ALTER TABLE impiegato DROP CONSTRAINT stip_ok;
```

- `DROP COLUMN ... RESTRICT` fallisce se una **vista** o un vincolo di altre tabelle usa la colonna.
- Un vincolo si rimuove **per nome**. Se non gli è stato dato un nome con `CONSTRAINT`, PostgreSQL ne genera uno automatico, che si legge con `\d tabella` in psql (o nella scheda *Constraints* di pgAdmin):

| Vincolo | Nome generato |
| --- | --- |
| `PRIMARY KEY` | `tabella_pkey` |
| `UNIQUE (col)` | `tabella_col_key` |
| `CHECK` su `col` | `tabella_col_check` |
| `FOREIGN KEY (col)` | `tabella_col_fkey` |

- `NOT NULL` e `DEFAULT` non sono vincoli con nome: si rimuovono con `ALTER COLUMN col DROP NOT NULL` / `DROP DEFAULT` (vedi [[Alter tabelle]]).

### 5. `DROP` vs `DELETE`

| | `DROP TABLE t` | `DELETE FROM t` |
| --- | --- | --- |
| Tipo | DDL | DML |
| Effetto | elimina **struttura e dati** | elimina **solo le righe** (la tabella resta, vuota) |
| Filtri | — | `DELETE FROM t WHERE ...` |
| FK | `RESTRICT`/`CASCADE` del `DROP` | politiche `ON DELETE` delle FK |
