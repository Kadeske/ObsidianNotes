Schema di riferimento: `studenti(matricola, ...)`, `corsi(codice, nome, docente)`, `esami(studente, corso, data, voto)`, `impiegati(matricola, nome, cognome, stipendio, cod_dip)`.

Le funzioni aggregate calcolano **un solo valore** a partire da un insieme di righe (tutta la tabella filtrata dal `WHERE`, oppure ogni gruppo del [[Raggruppamento (GROUP BY e HAVING)|GROUP BY]]). Sono un'estensione di SQL rispetto all'algebra relazionale.

### 1. Le funzioni

| Funzione                            | Calcola                                   | Argomento                                             | `NULL`   |
| ----------------------------------- | ----------------------------------------- | ----------------------------------------------------- | -------- |
| `COUNT(*)`                          | numero di **righe**                       | —                                                     | contati  |
| `COUNT(attr)` (= `COUNT(ALL attr)`) | numero di valori **non nulli**            | qualsiasi                                             | ignorati |
| `COUNT(DISTINCT attr)`              | numero di valori **non nulli e distinti** | qualsiasi                                             | ignorati |
| `SUM(expr)`                         | somma                                     | numerico / intervallo                                 | ignorati |
| `AVG(expr)`                         | media                                     | numerico / intervallo                                 | ignorati |
| `MAX(expr)` / `MIN(expr)`           | massimo / minimo                          | qualsiasi tipo **ordinabile** (anche stringhe e date) | ignorati |

- `SUM`, `AVG`, `MAX`, `MIN` accettano `ALL` (default) o `DISTINCT`. Con `MAX`/`MIN` `DISTINCT` non cambia nulla.
- L'argomento può essere un'espressione: `SUM(stipendio * 12)`, `AVG(voto * 100 / 30)`.

### 2. Esempi

``` sql
-- Numero di studenti
SELECT COUNT(*) FROM studenti;

-- Numero di studenti che hanno sostenuto almeno un esame
SELECT COUNT(DISTINCT studente) FROM esami;

-- Numero di esami con voto registrato
SELECT COUNT(voto) FROM esami;

-- Media dei voti dello studente 'S001'
SELECT AVG(voto) FROM esami WHERE studente = 'S001';

-- Stipendio minimo e massimo
SELECT MIN(stipendio), MAX(stipendio) FROM impiegati;

-- Primo cognome in ordine alfabetico e data dell'ultimo esame
SELECT MIN(cognome) FROM studenti;
SELECT MAX(data) FROM esami;

-- Più aggregati con alias
SELECT COUNT(*) AS n_esami, AVG(voto) AS media, MAX(voto) - MIN(voto) AS escursione
FROM esami WHERE corso = 'C1';

-- Aggregato dopo un join: numero di esami dei corsi del docente D1
SELECT COUNT(*)
FROM esami e JOIN corsi c ON e.corso = c.codice
WHERE c.docente = 'D1';
```

### 3. Regole sintattiche

#### a. Niente attributi "sciolti" insieme agli aggregati (senza `GROUP BY`)

Più aggregati affiancati vanno **benissimo**: ognuno produce un solo valore, quindi il risultato è una riga sola.

``` sql
SELECT MAX(voto), MIN(voto), AVG(voto), COUNT(*) FROM esami;   -- OK: una riga, quattro colonne
SELECT MAX(voto) - MIN(voto) AS escursione FROM esami;         -- OK: espressione tra aggregati
```

Il problema nasce solo mescolando un aggregato con una colonna **non** aggregata:

``` sql
SELECT studente, MAX(voto) FROM esami;   -- ERRORE
```

Errore: *"la colonna esami.studente deve comparire nella clausola GROUP BY o essere usata in una funzione di aggregazione"*. Il `MAX` produce una sola riga, mentre `studente` ne ha tante. Per sapere **chi** ha il voto massimo serve una sub-query:

``` sql
SELECT studente, voto FROM esami
WHERE voto = (SELECT MAX(voto) FROM esami);
```

#### b. Niente aggregati nel `WHERE`

``` sql
SELECT * FROM esami WHERE voto = MAX(voto);   -- ERRORE: aggregati non ammessi in WHERE
```

Il `WHERE` lavora riga per riga, prima che gli aggregati siano calcolati. Si usa una sub-query (come sopra) oppure `HAVING` per condizioni sui gruppi.

#### c. Niente aggregati annidati

``` sql
SELECT AVG(COUNT(*)) FROM esami GROUP BY studente;   -- ERRORE
```

Si risolve con una [[Viste|vista]] o una sub-query nel `FROM`:

``` sql
SELECT AVG(n) FROM (SELECT studente, COUNT(*) AS n FROM esami GROUP BY studente) AS t;
```

### 4. Casi limite

#### a. Insieme vuoto

| Funzione | Risultato su 0 righe |
| --- | --- |
| `COUNT(*)`, `COUNT(attr)` | `0` |
| `SUM`, `AVG`, `MAX`, `MIN` | **`NULL`** (non 0!) |

``` sql
SELECT SUM(voto) FROM esami WHERE corso = 'CORSO_SENZA_ESAMI';   -- NULL
```

Quindi `WHERE x > (SELECT MAX(...) ...)` con sub-query vuota non restituisce nulla.

#### b. `COUNT(*)` vs `COUNT(attr)` vs `COUNT(DISTINCT attr)`

Tabella `esami` con voti: `28, 30, NULL, 24, 28`

| Espressione | Risultato |
| --- | --- |
| `COUNT(*)` | 5 |
| `COUNT(voto)` | 4 |
| `COUNT(DISTINCT voto)` | 3 (28, 30, 24) |
| `SUM(voto)` | 110 |
| `AVG(voto)` | 27.5 (= 110 / **4**, non / 5) |
| `SUM(voto) / COUNT(*)` | 22 (≠ media! divide per 5) |

#### c. Divisione intera

In PostgreSQL `SUM(voto) / COUNT(voto)` con colonne intere fa una **divisione intera** (troncata): `55 / 2 = 27`, mentre `AVG(voto) = 27.5`. Per la media usare `AVG`, oppure `SUM(voto)::numeric / COUNT(voto)`.

#### d. `COUNT` e outer join

Per contare includendo chi ha **zero** occorrenze serve `LEFT JOIN` + `COUNT(colonna della tabella destra)`:

``` sql
SELECT c.codice, COUNT(e.studente) AS n_esami      -- 0 per i corsi senza esami
FROM corsi c LEFT JOIN esami e ON c.codice = e.corso
GROUP BY c.codice;
```

Con `COUNT(*)` i corsi senza esami risulterebbero con **1** (la riga estesa con `NULL` viene contata).

#### e. Duplicati introdotti dai join

Un join può moltiplicare le righe: `SUM` / `COUNT(*)` contano anche i duplicati.

``` sql
-- Numero di studenti che hanno un esame con voto >= 28
SELECT COUNT(*) FROM studenti s JOIN esami e ON s.matricola = e.studente WHERE e.voto >= 28;
-- SBAGLIATO: conta gli ESAMI, uno studente con due 30 conta 2 volte

SELECT COUNT(DISTINCT s.matricola) FROM studenti s JOIN esami e ON s.matricola = e.studente
WHERE e.voto >= 28;   -- CORRETTO
```

#### f. `DISTINCT` dentro e fuori

- `SELECT DISTINCT COUNT(voto)` → `DISTINCT` sul risultato (una riga): inutile.
- `SELECT COUNT(DISTINCT voto)` → conta i valori distinti: è quasi sempre questo che si vuole.
