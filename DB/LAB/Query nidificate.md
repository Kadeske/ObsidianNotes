Schema di riferimento:
- `studenti(matricola, cognome, nome, citta_nascita, citta_residenza, anno)`
- `corsi(codice, nome, docente)`
- `docenti(cod_docente, cognome, nome)`
- `esami(studente, corso, data, voto)`, con FK `studente → studenti` e `corso → corsi`

Una query è **nidificata** quando la sua condizione usa il risultato di un'altra query (**sub-query**), scritta tra parentesi.

### 1. Operatori disponibili

| Operatore | Vero quando… | Sub-query vuota |
| --- | --- | --- |
| `attr OP ANY (sq)` | il confronto è vero per **almeno un** valore | `FALSE` |
| `attr OP ALL (sq)` | il confronto è vero per **tutti** i valori | **`TRUE`** ⚠️ |
| `attr IN (sq)` | il valore compare nel risultato (≡ `= ANY`) | `FALSE` |
| `attr NOT IN (sq)` | il valore non compare (≡ `<> ALL`) | **`TRUE`** |
| `EXISTS (sq)` | la sub-query restituisce **almeno una riga** | `FALSE` |
| `NOT EXISTS (sq)` | la sub-query non restituisce righe | `TRUE` |
| `attr OP (sq)` | solo se `sq` restituisce **un unico valore** | `NULL` → riga scartata |

`OP ∈ { =, <>, <, <=, >, >= }`

#### `ANY`

``` sql
-- Studenti che hanno sostenuto l'esame del corso C3
SELECT cognome, nome
FROM studenti
WHERE matricola = ANY (SELECT studente FROM esami WHERE corso = 'C3');

-- Esami con voto superiore ad almeno un voto preso da Rossi (= superiore al suo minimo)
SELECT * FROM esami
WHERE voto > ANY (SELECT e.voto FROM esami e JOIN studenti s ON e.studente = s.matricola
                  WHERE s.cognome = 'Rossi');
```

#### `ALL`

``` sql
-- Studenti con l'anno di corso più basso
SELECT * FROM studenti
WHERE anno <= ALL (SELECT anno FROM studenti);

-- Esami con voto più alto di tutti quelli del corso C1 (= superiore al massimo di C1)
SELECT * FROM esami
WHERE voto > ALL (SELECT voto FROM esami WHERE corso = 'C1' AND voto IS NOT NULL);
```

#### `IN` / `NOT IN`

``` sql
-- Studenti che hanno sostenuto almeno un esame
SELECT cognome, nome FROM studenti
WHERE matricola IN (SELECT studente FROM esami);

-- Studenti che NON hanno sostenuto alcun esame
SELECT cognome, nome FROM studenti
WHERE matricola NOT IN (SELECT studente FROM esami);
```

#### Costruttore di tupla

Confronta **più attributi** insieme con il risultato della sub-query (stesso numero di colonne, per posizione):

``` sql
-- Studenti con stesso nome E cognome di un docente
SELECT * FROM studenti
WHERE (nome, cognome) IN (SELECT nome, cognome FROM docenti);
```

> Diverso da `nome IN (SELECT nome ...) AND cognome IN (SELECT cognome ...)`, che accetterebbe "Mario" preso da un docente e "Rossi" da un altro.

#### `EXISTS` / `NOT EXISTS`

``` sql
-- Studenti che hanno sostenuto l'esame del corso C3
SELECT cognome, nome FROM studenti s
WHERE EXISTS (SELECT * FROM esami e
              WHERE e.corso = 'C3' AND e.studente = s.matricola);

-- Studenti che NON hanno sostenuto l'esame del corso C3
SELECT cognome, nome FROM studenti s
WHERE NOT EXISTS (SELECT * FROM esami e
                  WHERE e.corso = 'C3' AND e.studente = s.matricola);
```

Con `EXISTS` conta solo **se** ci sono righe, non cosa contengono: `SELECT *`, `SELECT 1` o `SELECT e.voto` sono equivalenti.

#### Confronto con valore singolo (sub-query scalare)

``` sql
-- Esami con il voto massimo
SELECT * FROM esami
WHERE voto = (SELECT MAX(voto) FROM esami);       -- OK: MAX restituisce un solo valore

-- Studenti dello stesso anno di Mario Rossi
SELECT * FROM studenti
WHERE anno = (SELECT anno FROM studenti WHERE nome = 'Mario' AND cognome = 'Rossi');
-- OK solo se esiste un unico Mario Rossi! Più sicuro: anno IN (...)
```

Se la sub-query restituisce **più righe** → errore *"più di una riga restituita da una sottoquery usata come espressione"*. Nel dubbio usare `IN` / `ANY` / `ALL`.

### 2. Sub-query correlate

Una sub-query è **correlata** quando usa alias/tabelle della query esterna: viene (concettualmente) rivalutata **per ogni riga** della query esterna.

``` sql
-- Studenti che hanno sostenuto C3 (C3 tra i corsi dello studente corrente)
SELECT * FROM studenti s
WHERE 'C3' IN (SELECT corso FROM esami e WHERE e.studente = s.matricola);

-- Per ogni corso, lo studente (o gli studenti) col voto più alto
SELECT e1.corso, e1.studente, e1.voto
FROM esami e1
WHERE e1.voto >= ALL (SELECT e2.voto FROM esami e2
                      WHERE e2.corso = e1.corso AND e2.voto IS NOT NULL);

-- Studenti con almeno un voto sopra la propria media
SELECT DISTINCT e1.studente
FROM esami e1
WHERE e1.voto > (SELECT AVG(e2.voto) FROM esami e2 WHERE e2.studente = e1.studente);
```

**Regole**:
- **Visibilità (scope)**: la sub-query vede gli alias della query esterna, **non viceversa**. Una query non può usare alias definiti in sub-query interne o parallele.
- Se la **stessa tabella** compare dentro e fuori, gli **alias sono obbligatori** (come nel self join).
- Con `[NOT] EXISTS` la correlazione è **indispensabile**. Senza:

``` sql
SELECT * FROM studenti s
WHERE EXISTS (SELECT * FROM esami e WHERE e.corso = 'C3');
-- restituisce TUTTI gli studenti se esiste almeno un esame di C3, NESSUNO altrimenti
```

### 3. Più livelli e più sub-query

``` sql
-- Studenti che hanno sostenuto un esame di un corso del docente D1 (3 livelli)
SELECT * FROM studenti
WHERE matricola IN (SELECT studente FROM esami
                    WHERE corso IN (SELECT codice FROM corsi WHERE docente = 'D1'));

-- Studenti che hanno sostenuto C3 ma non C4 (2 sub-query allo stesso livello)
SELECT * FROM studenti
WHERE matricola IN     (SELECT studente FROM esami WHERE corso = 'C3')
  AND matricola NOT IN (SELECT studente FROM esami WHERE corso = 'C4');
```

### 4. Riducibilità a join

| Riducibili a una query con join | **Non** riducibili |
| --- | --- |
| `IN` | `NOT IN` |
| `ANY` (qualsiasi operatore) | `ALL` (qualsiasi operatore) |
| `EXISTS` + correlazione | `NOT EXISTS` + correlazione |

``` sql
-- Queste 4 query sono equivalenti
... WHERE matricola = ANY (SELECT studente FROM esami WHERE corso = 'C3')
... WHERE matricola IN    (SELECT studente FROM esami WHERE corso = 'C3')
... WHERE EXISTS (SELECT * FROM esami e WHERE e.corso = 'C3' AND e.studente = s.matricola)
SELECT s.* FROM studenti s JOIN esami e ON s.matricola = e.studente WHERE e.corso = 'C3';
```

> Equivalenti **a meno dei duplicati**: con il join uno studente compare una volta per ogni riga di `esami` che soddisfa la condizione (qui al massimo una perché `(studente, corso)` è chiave; con `WHERE e.voto > 25` invece potrebbe ripetersi → serve `DISTINCT`).

⚠️ **Trappola classica**: la negazione **non** si ottiene negando la condizione del join.

``` sql
-- SBAGLIATO: studenti che hanno sostenuto ALMENO UN esame DIVERSO da C3
SELECT DISTINCT s.* FROM studenti s JOIN esami e ON s.matricola = e.studente
WHERE e.corso <> 'C3';

-- CORRETTO: studenti che NON hanno sostenuto C3
SELECT * FROM studenti
WHERE matricola NOT IN (SELECT studente FROM esami WHERE corso = 'C3');
```

Il primo restituisce anche chi ha fatto C3 + un altro esame, ed esclude chi non ha fatto nessun esame.

### 5. Casi limite

#### a. `NOT IN` con `NULL` nella sub-query → risultato vuoto

``` sql
-- Se anche un solo esame ha voto NULL, questa query restituisce 0 righe
SELECT * FROM studenti WHERE 30 NOT IN (SELECT voto FROM esami);
```

Soluzioni: aggiungere `WHERE voto IS NOT NULL` nella sub-query, oppure riscrivere con `NOT EXISTS` (che non soffre del problema). Non succede quando la colonna è una PK o `NOT NULL` (es. `SELECT studente FROM esami` se `studente` fa parte della chiave).

#### b. `ALL` con `NULL` nella sub-query → risultato vuoto

``` sql
SELECT * FROM esami WHERE voto >= ALL (SELECT voto FROM esami);
-- se c'è un voto NULL: voto >= NULL è UNKNOWN → nessuna riga!
```

Soluzione: `... ALL (SELECT voto FROM esami WHERE voto IS NOT NULL)` oppure `voto = (SELECT MAX(voto) FROM esami)` (`MAX` ignora i `NULL`).

#### c. `ALL` con sub-query vuota → sempre vero

``` sql
-- Esami con voto superiore a tutti i voti del corso C4. Se C4 non ha esami → TUTTI gli esami
SELECT * FROM esami WHERE voto > ALL (SELECT voto FROM esami WHERE corso = 'C4');
```

Invece `voto > (SELECT MAX(voto) FROM esami WHERE corso = 'C4')` → `MAX` di insieme vuoto è `NULL` → **nessuna** riga. Le due formulazioni differiscono proprio nel caso vuoto.

#### d. Massimo/minimo con pareggi

`>= ALL` e `= (SELECT MAX ...)` restituiscono **tutte** le righe a pari merito. `> ALL` sulla stessa tabella invece restituisce **nulla** (un valore non è strettamente maggiore di se stesso), a meno di escludere la riga corrente.

#### e. `IN` con lista vs con sub-query

`IN (1, 2, 3)` accetta una lista di costanti. Con `ANY/ALL` e una lista di costanti, PostgreSQL vuole un **array**: `voto = ANY ('{29,30,33}')` o `voto = ANY (ARRAY[29,30,33])`. `voto = ANY (29, 30, 33)` → errore di sintassi. Con una sub-query si scrive normalmente: `voto = ANY (SELECT ...)`.

#### f. Sub-query nel `FROM`

Una sub-query può stare anche nel `FROM` come tabella temporanea (in PostgreSQL < 16 l'alias è obbligatorio, conviene metterlo sempre). Utile per comporre aggregati, alternativa alle [[Viste]]:

``` sql
SELECT AVG(n_esami)
FROM (SELECT studente, COUNT(*) AS n_esami FROM esami GROUP BY studente) AS t;
```
