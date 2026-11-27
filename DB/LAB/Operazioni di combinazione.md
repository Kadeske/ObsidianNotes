Schema di riferimento:
- `studenti(matricola, cognome, nome, citta_residenza, anno)`
- `corsi(codice, nome, docente)`
- `esami(studente, corso, data, voto)`, con FK `studente → studenti`, `corso → corsi`
- `impiegati(matricola, nome, cognome, mansione, stipendio, cod_dip)`, `dipartimenti(cod_dip, nome_dip, direttore)`

### 1. Prodotto cartesiano

Combina **ogni** riga di `T1` con **ogni** riga di `T2` ($T_1 \times T_2$): se `T1` ha $n$ righe e `T2` ne ha $m$, il risultato ha $n \cdot m$ righe e tutte le colonne di entrambe.

``` sql
SELECT * FROM t1, t2;              -- sintassi implicita
SELECT * FROM t1 CROSS JOIN t2;    -- sintassi esplicita
```

Da solo ha poco senso: diventa utile con una condizione che collega le righe (**theta-join**, $\bowtie_{\text{cond}}$). Se la condizione è un'uguaglianza si parla di **equi-join**.

### 2. Join

#### Sintassi implicita (condizione nel `WHERE`)

``` sql
-- Cognome e nome degli studenti che hanno sostenuto l'esame del corso C3
SELECT s.cognome, s.nome
FROM studenti s, esami e
WHERE s.matricola = e.studente     -- condizione di join
  AND e.corso = 'C3';              -- condizione di selezione
```

#### Sintassi esplicita (`JOIN ... ON`)

``` sql
SELECT s.cognome, s.nome
FROM studenti s JOIN esami e ON s.matricola = e.studente
WHERE e.corso = 'C3';
```

Separa la condizione di **join** (`ON`) dai **filtri** sulle righe (`WHERE`). Con l'`INNER JOIN` spostare un filtro tra `ON` e `WHERE` non cambia il risultato; con gli **outer join sì** (§7).

``` sql
-- Matricola degli studenti che hanno sostenuto Geometria
SELECT e.studente
FROM esami e JOIN corsi c ON e.corso = c.codice
WHERE c.nome = 'Geometria';
```

#### Self join (una tabella con se stessa)

Servono **alias diversi** per le due "copie":

``` sql
-- Coppie di studenti residenti nella stessa città
SELECT s1.matricola, s2.matricola
FROM studenti s1 JOIN studenti s2
  ON s1.citta_residenza = s2.citta_residenza
 AND s1.matricola < s2.matricola;
```

| Seconda condizione | Effetto |
| --- | --- |
| nessuna | compaiono anche le coppie (A, A) |
| `s1.matricola <> s2.matricola` (slide) | ogni coppia compare **due volte**: (A, B) e (B, A) |
| `s1.matricola < s2.matricola` | ogni coppia compare **una sola volta** |

### 3. Join interni ed esterni

``` sql
T1 [INNER | LEFT [OUTER] | RIGHT [OUTER] | FULL [OUTER]] JOIN T2 ON condizione
```

| Tipo | Mantiene | Righe senza corrispondenza |
| --- | --- | --- |
| `[INNER] JOIN` (default) | solo le righe che hanno corrispondenza | scartate |
| `LEFT JOIN` | **tutte** le righe di `T1` (sinistra) | colonne di `T2` a `NULL` |
| `RIGHT JOIN` | **tutte** le righe di `T2` (destra) | colonne di `T1` a `NULL` |
| `FULL JOIN` | tutte le righe di entrambe | `NULL` dal lato mancante |

`T1 RIGHT JOIN T2` ≡ `T2 LEFT JOIN T1` (cambia solo l'ordine delle colonne).

**Esempio** (dalle slide):

| T1.A | T1.B |
| --- | --- |
| a | 1 |
| b | 2 |
| e | 4 |

| T2.B | T2.C |
| --- | --- |
| 1 | x |
| 4 | v |
| 5 | z |

| Join su `T1.B = T2.B` | Risultato (A, T1.B, T2.B, C) |
| --- | --- |
| `INNER` | (a, 1, 1, x), (e, 4, 4, v) |
| `LEFT` | + (b, 2, NULL, NULL) |
| `RIGHT` | + (NULL, NULL, 5, z) |
| `FULL` | + (b, 2, NULL, NULL) + (NULL, NULL, 5, z) |

``` sql
-- Tutti gli studenti con i relativi esami, inclusi quelli che non hanno sostenuto esami
SELECT s.matricola, e.corso
FROM studenti s LEFT JOIN esami e ON s.matricola = e.studente;
```

### 4. Join multipli

Ogni join ha la propria condizione. Con $n$ tabelle servono (almeno) $n-1$ condizioni di join, altrimenti si ottiene un prodotto cartesiano.

``` sql
-- Matricola e cognome degli studenti che hanno sostenuto Analisi
SELECT s.matricola, s.cognome                       -- implicita
FROM studenti s, esami e, corsi c
WHERE s.matricola = e.studente AND e.corso = c.codice
  AND c.nome = 'Analisi';

SELECT s.matricola, s.cognome                       -- esplicita
FROM studenti s
JOIN esami e ON s.matricola = e.studente
JOIN corsi c ON e.corso = c.codice
WHERE c.nome = 'Analisi';

-- Per ogni esame con voto > 24: cognome dello studente e docente del corso
SELECT s.cognome, c.docente
FROM studenti s
JOIN esami e ON s.matricola = e.studente
JOIN corsi c ON e.corso = c.codice
WHERE e.voto > 24;

-- Matricole degli studenti che hanno sostenuto almeno un esame sostenuto da Mario Rossi
SELECT DISTINCT e1.studente
FROM esami e1
JOIN esami e2 ON e1.corso = e2.corso                 -- stessa tabella due volte
JOIN studenti s ON e2.studente = s.matricola
WHERE s.nome = 'Mario' AND s.cognome = 'Rossi';
```

Nell'ultimo esempio compare anche Mario Rossi stesso: per escluderlo aggiungere `AND e1.studente <> e2.studente`.

### 5. `NATURAL JOIN` e `USING`

``` sql
T1 NATURAL JOIN T2            -- uguaglianza su TUTTE le colonne con lo stesso nome
T1 JOIN T2 USING (col1, ...)  -- uguaglianza solo sulle colonne indicate (stesso nome in entrambe)
```

``` sql
-- Nome del dipartimento in cui lavora Mario Rossi
SELECT nome_dip FROM impiegati NATURAL JOIN dipartimenti
WHERE nome = 'Mario' AND cognome = 'Rossi';

SELECT nome_dip FROM impiegati JOIN dipartimenti USING (cod_dip)
WHERE nome = 'Mario' AND cognome = 'Rossi';
```

- Le colonne di join compaiono **una sola volta** nel risultato e si scrivono **senza prefisso** (`cod_dip`, non `impiegati.cod_dip`).
- Esistono anche le varianti esterne: `NATURAL LEFT JOIN`, `LEFT JOIN ... USING (...)`, ecc.
- In un `FULL JOIN ... USING (b)` la colonna `b` del risultato prende il valore da **quella delle due tabelle che non è `NULL`**.

### 6. Duplicati nei join

Un join restituisce una riga per **ogni coppia** che soddisfa la condizione: uno studente con 5 esami compare 5 volte in `studenti JOIN esami`. Se la traccia chiede *studenti* (non esami) → `SELECT DISTINCT` oppure `IN`/`EXISTS` ([[Query nidificate]]).

### 7. Casi limite

#### a. Filtro sulla tabella destra di un `LEFT JOIN`

``` sql
-- Studenti con i loro esami da almeno 28, mostrando anche chi non ne ha
SELECT s.matricola, e.corso
FROM studenti s LEFT JOIN esami e ON s.matricola = e.studente AND e.voto >= 28;   -- CORRETTO

SELECT s.matricola, e.corso
FROM studenti s LEFT JOIN esami e ON s.matricola = e.studente
WHERE e.voto >= 28;   -- SBAGLIATO: le righe con e.voto NULL vengono scartate → diventa un INNER JOIN
```

#### b. Filtro sulla tabella sinistra dentro l'`ON`

``` sql
SELECT * FROM t1 LEFT JOIN t2 ON t1.b = t2.b AND t1.a = 'a';
```

**Non** filtra `T1`: tutte le righe di `T1` restano, solo che quelle con `a <> 'a'` non si abbinano a nulla (colonne di `T2` a `NULL`). Per filtrare la tabella di sinistra, la condizione va nel `WHERE`.

#### c. Anti-join: righe **senza** corrispondenza

``` sql
-- Studenti che non hanno sostenuto esami
SELECT s.*
FROM studenti s LEFT JOIN esami e ON s.matricola = e.studente
WHERE e.studente IS NULL;      -- testare una colonna di T2 che non può essere NULL (es. la chiave)
```

#### d. Outer join seguito da inner join

`studenti LEFT JOIN esami ... JOIN corsi ON e.corso = c.codice`: il secondo join (interno) scarta gli studenti senza esami, perché `e.corso` è `NULL`. Usare `LEFT JOIN` anche per le tabelle successive.

#### e. `NULL` nelle colonne di join

`NULL = NULL` non è vero: righe con la colonna di join a `NULL` **non si abbinano mai** (né tra loro né con altro). In un outer join compaiono solo come righe "senza corrispondenza".

#### f. Trappole del `NATURAL JOIN`

- Confronta **tutte** le colonne omonime, anche quelle che non c'entrano: `studenti NATURAL JOIN corsi` confronta `nome` (nome dello studente = nome del corso!) → risultato vuoto o sbagliato.
- Se **non ci sono colonne in comune** diventa un **prodotto cartesiano**, senza errori.
- Aggiungere una colonna allo schema può cambiare in silenzio il risultato. Preferire `JOIN ... ON` o `USING`.

#### g. Errori di sintassi frequenti

| Errore | Causa |
| --- | --- |
| `il riferimento alla colonna "b" è ambiguo` | colonna presente in più tabelle senza prefisso |
| `errore di sintassi` con `FROM t1 JOIN t2;` | `JOIN` (non `CROSS`/`NATURAL`) senza `ON` o `USING` |
| `riferimento non valido all'elemento della clausola FROM per la tabella "t1"` | uso del nome originale dopo aver definito un alias, oppure un `ON` che fa riferimento a una tabella separata da virgola (`FROM t1, t2 JOIN t3 ON t1.x = ...`) |

Per l'ultimo caso: non mescolare virgole e `JOIN` espliciti, scegliere una sola sintassi.

### 8. Riepilogo: quale join usare

| La traccia chiede… | Soluzione |
| --- | --- |
| dati collegati presenti in entrambe le tabelle | `JOIN ... ON` |
| tutti gli X, anche quelli senza Y | `X LEFT JOIN Y` |
| gli X **senza** Y | `LEFT JOIN ... WHERE Y.chiave IS NULL` oppure `NOT EXISTS` |
| per ogni X il numero di Y (anche 0) | `LEFT JOIN` + `GROUP BY` + `COUNT(Y.colonna)` → [[Raggruppamento (GROUP BY e HAVING)]] |
| coppie di elementi della stessa tabella | self join con `<` |

Altri schemi ricorrenti → [[Pattern di query]].
