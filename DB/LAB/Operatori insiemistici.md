Schema di riferimento: `studenti(matricola, cognome, nome, citta_residenza, anno)`, `docenti(cod_docente, cognome, nome)`, `corsi(codice, nome, docente)`, `esami(studente, corso, data, voto)`, `paternita(padre, figlio)`, `maternita(madre, figlio)`.

### 1. Sintassi

``` sql
SELECT ... UNION     [ALL] SELECT ...   -- unione        A ∪ B
SELECT ... INTERSECT [ALL] SELECT ...   -- intersezione  A ∩ B
SELECT ... EXCEPT    [ALL] SELECT ...   -- differenza    A − B
```

- Per default i **duplicati vengono eliminati** (come un `DISTINCT` sul risultato).
- Con **`ALL`** i duplicati vengono mantenuti (semantica multiset).

### 2. Compatibilità dei due operandi

- Stesso **numero di colonne**. Altrimenti: *"ogni query in UNION deve avere lo stesso numero di colonne"*.
- **Domini compatibili** colonna per colonna. Altrimenti: *"in UNION i tipi text e integer non combaciano"*.
- I nomi possono essere diversi: la corrispondenza è **per posizione**, non per nome.
- I nomi delle colonne del risultato sono quelli del **primo** operando (si rinominano con `AS` nel primo `SELECT`).

``` sql
SELECT padre AS genitore FROM paternita
UNION
SELECT madre FROM maternita;          -- colonna risultato: "genitore"
```

⚠️ Corrispondenza per posizione:

``` sql
SELECT nome, cognome FROM studenti
UNION
SELECT cognome, nome FROM docenti;    -- NESSUN errore, ma mescola nomi e cognomi!
```

### 3. Esempi

``` sql
-- Cognomi di studenti O di docenti
SELECT cognome FROM studenti
UNION
SELECT cognome FROM docenti;

-- Cognomi di studenti E di docenti
SELECT cognome FROM studenti
INTERSECT
SELECT cognome FROM docenti;

-- Cognomi di studenti MA NON di docenti
SELECT cognome FROM studenti
EXCEPT
SELECT cognome FROM docenti;

-- Matricole degli studenti che non hanno mai sostenuto esami
SELECT matricola FROM studenti
EXCEPT
SELECT studente FROM esami;

-- Studenti che hanno sostenuto sia C1 che C2
SELECT studente FROM esami WHERE corso = 'C1'
INTERSECT
SELECT studente FROM esami WHERE corso = 'C2';

-- Studenti che hanno sostenuto C1 ma non C2
SELECT studente FROM esami WHERE corso = 'C1'
EXCEPT
SELECT studente FROM esami WHERE corso = 'C2';

-- Persone (studenti e docenti) con etichetta del ruolo, ordinate
SELECT cognome, nome, 'studente' AS ruolo FROM studenti
UNION
SELECT cognome, nome, 'docente' FROM docenti
ORDER BY cognome, nome;
```

### 4. Equivalenze con le query nidificate

`INTERSECT` ed `EXCEPT` si possono riscrivere con [[Query nidificate]]; `UNION` **no** (in alcune implementazioni SQL esiste solo `UNION`).

``` sql
-- EXCEPT
SELECT cognome FROM studenti WHERE cognome NOT IN (SELECT cognome FROM docenti);
-- INTERSECT
SELECT cognome FROM studenti WHERE cognome IN (SELECT cognome FROM docenti);
```

Equivalenti **a meno di duplicati e `NULL`**:
- la versione nidificata **mantiene i duplicati** (due studenti "Rossi" → due righe); l'operatore insiemistico li elimina. Per avere lo stesso risultato: `SELECT DISTINCT`.
- con un `NULL` nella sub-query il `NOT IN` restituisce **0 righe**, `EXCEPT` no (vedi sotto).

Con più colonne si usa il costruttore di tupla: `WHERE (nome, cognome) NOT IN (SELECT nome, cognome FROM docenti)`.

### 5. Casi limite

#### a. `NULL` nelle operazioni insiemistiche

Negli operatori insiemistici due `NULL` sono considerati **uguali** (a differenza di `=`):

``` sql
SELECT citta_residenza FROM studenti
INTERSECT
SELECT NULL;          -- restituisce una riga NULL se qualche studente ha città NULL
```

`x EXCEPT` toglie anche i `NULL` se compaiono nel secondo operando; `NOT IN` invece "si blocca" del tutto.

#### b. Semantica di `ALL`

Dato A = {1, 1, 1, 2} e B = {1, 2}:

| Operazione | Risultato |
| --- | --- |
| `A UNION B` | {1, 2} |
| `A UNION ALL B` | {1, 1, 1, 2, 1, 2} |
| `A INTERSECT B` | {1, 2} |
| `A INTERSECT ALL B` | {1, 2} (minimo delle occorrenze: min(3,1)=1 volta 1, min(1,1)=1 volta 2) |
| `A EXCEPT B` | {} (1 e 2 compaiono in B) |
| `A EXCEPT ALL B` | {1, 1} (3 − 1 = 2 occorrenze di 1, 1 − 1 = 0 di 2) |

#### c. `ORDER BY` una sola volta, alla fine

`ORDER BY` si scrive **dopo l'ultimo** `SELECT` e si applica all'intero risultato; si riferisce ai nomi del primo operando o alle posizioni (`ORDER BY 1`).

``` sql
SELECT cognome AS c FROM studenti
UNION
SELECT cognome FROM docenti
ORDER BY c;
```

#### d. Precedenza e parentesi

`INTERSECT` ha precedenza su `UNION` ed `EXCEPT`, che vengono valutati da sinistra a destra. Con più operatori conviene usare le parentesi:

``` sql
(SELECT studente FROM esami WHERE corso = 'C1'
 UNION
 SELECT studente FROM esami WHERE corso = 'C2')
EXCEPT
SELECT studente FROM esami WHERE corso = 'C3';
```

#### e. `UNION` vs `OR`

Sulla **stessa** tabella `UNION` equivale a un `OR` (a meno dei duplicati): 
`SELECT studente FROM esami WHERE corso='C1' OR corso='C2'` (con duplicati) ≈ `... UNION ...` (senza).
Ma **`INTERSECT` ≠ `AND`** riga per riga: `WHERE corso = 'C1' AND corso = 'C2'` è sempre falso (una riga ha un solo corso), mentre l'`INTERSECT` trova chi ha **due righe diverse**, una per C1 e una per C2.
