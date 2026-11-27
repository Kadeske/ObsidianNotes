### 1. Schemi (`CREATE SCHEMA`)

Uno schema è un contenitore di oggetti (tabelle, domini, viste…).

``` sql
CREATE SCHEMA [NomeSchema] [AUTHORIZATION NomeProprietario];
```

- `AUTHORIZATION` omesso → proprietario = utente che esegue il comando.
- `NomeSchema` omesso → nome = nome del proprietario.
- Creare schemi richiede un privilegio concesso dall'amministratore.

``` sql
CREATE SCHEMA azienda;                       -- proprietario: utente corrente
CREATE SCHEMA azienda AUTHORIZATION jsmith;  -- proprietario: jsmith
CREATE SCHEMA AUTHORIZATION jsmith;          -- schema di nome "jsmith"
```

**Riferirsi agli oggetti dello schema**: `schema.oggetto` (es. `segreteria.studenti`, `segreteria.votazione`). Senza prefisso PostgreSQL cerca nel `search_path` (di default lo schema `public`).

``` sql
CREATE TABLE azienda.impiegato (...);          -- creata in azienda
CREATE TABLE impiegato (...);                  -- creata in public!
SET search_path TO azienda;                    -- da ora "impiegato" = azienda.impiegato
```

> **Errore tipico**: creare lo schema e poi le tabelle senza prefisso → finiscono in `public`. Nelle FK verso tabelle di un altro schema serve sempre il prefisso: `REFERENCES segreteria.studenti(matricola)`.

### 2. Domini (`CREATE DOMAIN`)

Tipo personalizzato costruito su un tipo esistente, con default e vincoli riutilizzabili.

``` sql
CREATE DOMAIN NomeDominio AS TipoDiDato [DEFAULT valore] [NOT NULL] [CHECK (condizione su VALUE)];
```

Nel `CHECK` il valore da controllare si chiama **`VALUE`**.

``` sql
CREATE DOMAIN segreteria.voto AS SMALLINT
    DEFAULT NULL
    CHECK (VALUE >= 18 AND VALUE <= 30);

CREATE DOMAIN segreteria.anni_corso AS SMALLINT
    DEFAULT 3
    NOT NULL;

CREATE DOMAIN segreteria.codice_fiscale AS CHAR(16)
    CHECK (LENGTH(VALUE) = 16);

CREATE DOMAIN segreteria.mansione AS VARCHAR(20)
    CHECK (VALUE IN ('dirigente', 'ingegnere', 'tecnico', 'segretario'));

CREATE DOMAIN segreteria.importo AS DECIMAL(10,2)
    DEFAULT 0
    CHECK (VALUE >= 0);
```

Uso:

``` sql
CREATE TABLE segreteria.esami (
    studente CHAR(6),
    corso    CHAR(3),
    voto     segreteria.voto,           -- eredita tipo, default e CHECK
    PRIMARY KEY (studente, corso)
);
```

**Perché usarli**: riuso, schemi più leggibili, una modifica al dominio si propaga a tutte le colonne che lo usano.

#### Casi limite dei domini

- **`NULL` passa il `CHECK`**: `INSERT ... voto = NULL` è accettato (condizione `UNKNOWN`, non `FALSE`). Per vietarlo: `NOT NULL` nel dominio o sulla colonna.
- **Default della colonna vs del dominio**: un `DEFAULT` scritto sulla colonna **prevale** su quello del dominio.
- **`CHAR(n)` e `LENGTH`**: `LENGTH()` su un `CHAR` **ignora gli spazi di riempimento**. Con `CHAR(16)`:
    - `'ABC'` → riempito a 16 con spazi, ma `LENGTH = 3` → viola il `CHECK` ✔ (è proprio il motivo per cui l'esercizio usa `LENGTH(VALUE) = 16`).
    - stringa di 17 caratteri → errore "valore troppo lungo" (prima ancora del `CHECK`).
- Un dominio usato da una tabella non si elimina con `DROP DOMAIN` semplice → serve `CASCADE` (vedi [[Eliminazione di oggetti (DROP)]]).

### 3. Tipi di dato - casi limite utili

(La tabella completa dei tipi è in [[Creazione tabelle]].)

| Caso | Comportamento PostgreSQL |
| --- | --- |
| `CHAR(5)` con `'ab'` | memorizzato come `'ab   '`; confronti con `'ab'` → `TRUE` (spazi finali ignorati) |
| `VARCHAR(3)` con `'abcdef'` in `INSERT` | **errore** "valore troppo lungo" (non tronca) |
| `NUMERIC(5,2)` con `123.456` | arrotondato a `123.46` |
| `NUMERIC(5,2)` con `1234.5` | **errore overflow**: restano solo 3 cifre prima della virgola (5−2) |
| `NUMERIC(p)` | equivale a `NUMERIC(p,0)` → interi |
| `NUMERIC` senza parametri | precisione e scala arbitrarie |
| `DECIMAL` vs `NUMERIC` | **equivalenti** |
| `7 / 2` tra interi | `3` (**divisione intera**!) → usare `7 / 2.0` o `7::numeric / 2` |
| `SERIAL` / `BIGSERIAL` | intero auto-incrementante (4 / 8 byte), utile per chiavi artificiali: `codicev SERIAL PRIMARY KEY` |
| `BOOLEAN` | accetta `TRUE/'t'/'true'/'y'/'yes'/'1'` e `FALSE/'f'/'false'/'n'/'no'/'0'` |
| Date | formato ISO di default: `'2026-11-25'`; con `SET datestyle` si cambia lo stile di visualizzazione |
| `TIME(p)`, `TIMESTAMP(p)`, `INTERVAL(p)` | `p` = cifre decimali dei secondi (0–6) |

### 4. Esempio completo

``` sql
CREATE SCHEMA acquario;

CREATE DOMAIN acquario.litri AS INTEGER CHECK (VALUE > 0);

CREATE TABLE acquario.pesce (
    codicep  SMALLINT PRIMARY KEY,
    nome     VARCHAR(20) NOT NULL,
    famiglia VARCHAR(20)
);

CREATE TABLE acquario.vasca (
    codicev   SERIAL PRIMARY KEY,
    nomevasca VARCHAR(20) NOT NULL,
    volume    acquario.litri,
    sala      VARCHAR(15)
);

CREATE TABLE acquario.presenza (
    codicevasca INTEGER  REFERENCES acquario.vasca(codicev),
    codicepesce SMALLINT REFERENCES acquario.pesce(codicep),
    quantita    SMALLINT DEFAULT 1 CHECK (quantita > 0),
    PRIMARY KEY (codicevasca, codicepesce)
);
```

- `codicevasca` è `INTEGER` perché `SERIAL` è un intero a 4 byte: il tipo della FK deve essere compatibile con quello della colonna riferita (non si usa `SERIAL` nella FK).
