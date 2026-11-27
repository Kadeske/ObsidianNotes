Vincoli **inter-relazionali**: legano i valori di una tabella (*referente* / figlia / interna) a quelli di un'altra (*riferita* / padre / esterna).

### 1. Sintassi

``` sql
-- a livello di tabella (obbligatoria se la chiave è composta)
FOREIGN KEY (ListaAttributi) REFERENCES TabellaRiferita [(ListaAttributiRiferiti)]
    [ON DELETE politica] [ON UPDATE politica]

-- a livello di colonna (solo chiave di un attributo)
NomeColonna Tipo REFERENCES TabellaRiferita [(AttributoRiferito)] [ON DELETE ...] [ON UPDATE ...]
```

**Significato**: per ogni riga della tabella referente, i valori della FK, **se diversi da `NULL`**, devono comparire in qualche riga della tabella riferita.

### 2. Regole da ricordare

- Gli attributi riferiti devono essere **`PRIMARY KEY` o `UNIQUE`**. Altrimenti: *"non c'è alcun vincolo univoco che corrisponda alle chiavi indicate"*.
- Se si riferisce la **chiave primaria** si può omettere la lista: `REFERENCES studenti`.
- Numero, ordine e tipi degli attributi devono corrispondere: `FOREIGN KEY (prov, numero) REFERENCES auto(prov, numero)`.
- La tabella riferita **deve esistere già**. Con riferimenti circolari (es. `Impiegati.CodiceDip → Dipartimenti` e `Dipartimenti.Direttore → Impiegati`) si creano le tabelle senza una delle FK e la si aggiunge dopo con `ALTER TABLE ... ADD CONSTRAINT ... FOREIGN KEY` (vedi [[Alter tabelle]]).
- **`NULL` nella FK è sempre ammesso** (se la colonna non è `NOT NULL`). Con FK composta basta **un** attributo `NULL` perché la riga non venga controllata: `(5, NULL)` è accettata anche se nel padre non esiste nessuna riga con `a = 5`.
- Per rendere obbligatoria la partecipazione: `NOT NULL` sulla colonna FK.

### 3. Esempi

``` sql
CREATE TABLE vigili (
    matricola INTEGER PRIMARY KEY,
    cognome   VARCHAR(20),
    nome      VARCHAR(20)
);

CREATE TABLE auto (
    provincia CHAR(2),
    numero    CHAR(6),
    cognome   VARCHAR(20),
    nome      VARCHAR(20),
    PRIMARY KEY (provincia, numero)
);

CREATE TABLE infrazioni (
    codice    CHAR(5) PRIMARY KEY,
    data      DATE NOT NULL,
    vigile    INTEGER NOT NULL REFERENCES vigili(matricola),   -- FK semplice, inline
    provincia CHAR(2),
    numero    CHAR(6),
    FOREIGN KEY (provincia, numero) REFERENCES auto(provincia, numero)   -- FK composta
);
```

> **Errore tipico**: scrivere due FK separate `provincia REFERENCES auto(provincia)` e `numero REFERENCES auto(numero)`. È sbagliato: verifica i valori separatamente (e fallisce perché né `provincia` né `numero` da soli sono unici).

**FK composta verso attributi `UNIQUE` (non PK):**

``` sql
CREATE TABLE anagrafica (
    id      SERIAL PRIMARY KEY,
    nome    VARCHAR(20),
    cognome VARCHAR(20),
    UNIQUE (nome, cognome)
);
CREATE TABLE impiegato (
    matricola CHAR(6) PRIMARY KEY,
    nome      VARCHAR(20) NOT NULL,
    cognome   VARCHAR(20) NOT NULL,
    FOREIGN KEY (nome, cognome) REFERENCES anagrafica(nome, cognome)
);
```

**FK verso la stessa tabella (auto-riferimento):**

``` sql
CREATE TABLE impiegati (
    matricola CHAR(6) PRIMARY KEY,
    capo      CHAR(6) REFERENCES impiegati(matricola)   -- NULL per chi non ha un capo
);
```

### 4. Quando il vincolo può essere violato

| Operazione | Tabella | Cosa succede |
| --- | --- | --- |
| `INSERT` / `UPDATE` della FK | **referente** (figlia) | Se il valore non esiste nel padre → **sempre rifiutato** (nessuna politica applicabile) |
| `DELETE` / `UPDATE` della chiave | **riferita** (padre) | Si applica la **politica di reazione** scelta |

Le politiche (`ON DELETE` / `ON UPDATE`) servono **solo** per il secondo caso.

### 5. Politiche di reazione

``` sql
ON <DELETE | UPDATE> <CASCADE | SET NULL | SET DEFAULT | RESTRICT | NO ACTION>
```

| Politica | `ON DELETE` (riga padre cancellata) | `ON UPDATE` (chiave padre modificata) |
| --- | --- | --- |
| `CASCADE` | cancella le righe figlie | aggiorna la FK delle figlie al nuovo valore |
| `SET NULL` | FK delle figlie → `NULL` | FK delle figlie → `NULL` |
| `SET DEFAULT` | FK delle figlie → valore di default | FK delle figlie → valore di default |
| `RESTRICT` | operazione rifiutata (controllo **prima**) | operazione rifiutata |
| `NO ACTION` (**default**) | operazione rifiutata (controllo **dopo** il tentativo) | operazione rifiutata |

- Si possono combinare politiche diverse: `ON DELETE SET NULL ON UPDATE CASCADE`.
- `RESTRICT` e `NO ACTION` danno lo stesso risultato nei casi visti a lezione (la differenza emerge solo con vincoli differiti).

#### Esempio sulle tabelle `auto` / `infrazioni`

Se `infrazioni` ha `FOREIGN KEY (provincia, numero) REFERENCES auto ON UPDATE CASCADE ON DELETE CASCADE`:

``` sql
UPDATE auto SET numero = '567389' WHERE provincia = 'PR' AND numero = '839548';
-- le infrazioni 64521 e 73321 ora hanno numero = '567389'

DELETE FROM auto WHERE provincia = 'PR' AND numero = '567389';
-- le infrazioni 64521 e 73321 vengono cancellate
```

#### Esempio tipico d'esame

``` sql
CREATE TABLE relatore (
    cod_studente CHAR(6) PRIMARY KEY,
    cod_docente  CHAR(6) NOT NULL,
    FOREIGN KEY (cod_studente) REFERENCES studente
        ON DELETE CASCADE      -- se lo studente sparisce, sparisce anche il relatore
        ON UPDATE CASCADE,
    FOREIGN KEY (cod_docente) REFERENCES docente
        ON DELETE RESTRICT     -- non si può cancellare un docente che segue tesi
        ON UPDATE CASCADE
);
```

**Come scegliere** (lettura della traccia):

| La traccia dice… | Politica |
| --- | --- |
| "se si cancella X, vanno cancellati anche i suoi Y" | `ON DELETE CASCADE` |
| "non deve essere possibile cancellare X se ha Y" | `ON DELETE RESTRICT` (o `NO ACTION`) |
| "le modifiche al codice di X devono propagarsi" | `ON UPDATE CASCADE` |
| "se X viene cancellato, Y resta ma senza riferimento" | `ON DELETE SET NULL` |
| nessuna indicazione | default `NO ACTION` |

### 6. Casi limite

- **`SET NULL` su colonna `NOT NULL`** → la cancellazione/modifica del padre **fallisce** (errore di violazione `NOT NULL`). Tipico con FK che fa parte della `PRIMARY KEY` della figlia.
- **`SET DEFAULT` con default inesistente nel padre** → la cancellazione **fallisce** (il default deve essere a sua volta una chiave presente nel padre, oppure `NULL`).
- **`SET DEFAULT` senza `DEFAULT` sulla colonna** → il default è `NULL` → equivale a `SET NULL`.
- **Reazioni a catena**: `A ← B (ON DELETE CASCADE) ← C (ON DELETE CASCADE)`: cancellare una riga di `A` cancella le righe collegate di `B` e, a cascata, di `C`. Se però una tabella `C2` riferisce `B` con `RESTRICT` e ha righe collegate, **l'intera cancellazione su `A` viene annullata**.
- **Inserimento in ordine sbagliato**: inserire prima la riga figlia e poi il padre → errore. Ordine corretto: padri → figli. Per cancellare: figli → padri (o usare `CASCADE`).
- **Tipo diverso**: FK `INTEGER` verso `CHAR(6)` → errore alla creazione. Verso una PK `SERIAL` la FK va dichiarata `INTEGER`.
