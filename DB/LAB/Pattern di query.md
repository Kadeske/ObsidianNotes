Raccolta di schemi risolutivi per le richieste più comuni negli esercizi, con le trappole da evitare.

Schema di riferimento:
- `studenti(matricola, cognome, nome, citta_residenza, anno)`
- `corsi(codice, nome, docente)`
- `esami(studente, corso, data, voto)`: PK `(studente, corso)`, FK `studente → studenti`, `corso → corsi`

### 1. Ordine di valutazione di una query

``` sql
SELECT DISTINCT ...   -- 5  (6: DISTINCT)
FROM ... JOIN ... ON  -- 1
WHERE ...             -- 2
GROUP BY ...          -- 3
HAVING ...            -- 4
ORDER BY ...          -- 7
```

Conseguenze pratiche:
- gli **alias** della `SELECT` si possono usare solo in `ORDER BY` (non in `WHERE`, `GROUP BY`, `HAVING`);
- gli **aggregati** non possono stare nel `WHERE`;
- il `WHERE` filtra **prima** di raggruppare, l'`HAVING` **dopo**.

### 2. Tabella "dalla traccia alla soluzione"

| La traccia dice… | Pattern | § |
| --- | --- | --- |
| "che hanno (fatto) almeno un…" | join / `IN` / `EXISTS` + `DISTINCT` | 3 |
| "che non hanno mai…", "nessun…" | `NOT IN` / `NOT EXISTS` / `LEFT JOIN … IS NULL` / `EXCEPT` | 4 |
| "solo…", "esclusivamente…" | almeno uno **e** `NOT EXISTS` di uno diverso | 5 |
| "tutti i…" | doppio `NOT EXISTS` oppure confronto di `COUNT` | 6 |
| "almeno N", "più di N" | `GROUP BY … HAVING COUNT(…) >= N` | 7 |
| "esattamente N", "un solo…" | `HAVING COUNT(…) = N` | 7 |
| "il massimo / il più alto / il più…" | `= (SELECT MAX…)` o `>= ALL` | 8 |
| "per ogni X, il massimo" | sub-query correlata | 8 |
| "il più frequente", "con più…" | `HAVING COUNT(*) >= ALL (…)` o vista | 8 |
| "sopra la media" | sub-query con `AVG` (correlata se "del suo gruppo") | 9 |
| "coppie di…" | self join con `<` | 10 |
| "sia A che B" | `INTERSECT` / due `IN` / self join | 11 |
| "A ma non B" | `EXCEPT` / `IN` + `NOT IN` | 11 |
| "per ogni X, il numero di Y (anche zero)" | `LEFT JOIN` + `COUNT(colonna destra)` | 12 |

### 3. "Almeno uno"

*Studenti che hanno sostenuto almeno un esame con voto ≥ 28.*

``` sql
-- join (servono DISTINCT: uno studente può avere più esami ≥ 28)
SELECT DISTINCT s.matricola, s.cognome
FROM studenti s JOIN esami e ON s.matricola = e.studente
WHERE e.voto >= 28;

-- IN (niente duplicati per costruzione)
SELECT matricola, cognome FROM studenti
WHERE matricola IN (SELECT studente FROM esami WHERE voto >= 28);

-- EXISTS
SELECT matricola, cognome FROM studenti s
WHERE EXISTS (SELECT * FROM esami e WHERE e.studente = s.matricola AND e.voto >= 28);
```

### 4. "Nessuno" / "mai"

*Studenti che non hanno mai sostenuto l'esame C1.*

``` sql
-- NOT IN (attenzione ai NULL nella sub-query!)
SELECT * FROM studenti
WHERE matricola NOT IN (SELECT studente FROM esami WHERE corso = 'C1');

-- NOT EXISTS (la più robusta)
SELECT * FROM studenti s
WHERE NOT EXISTS (SELECT * FROM esami e WHERE e.studente = s.matricola AND e.corso = 'C1');

-- Anti-join con LEFT JOIN: la condizione sul corso va nell'ON!
SELECT s.*
FROM studenti s LEFT JOIN esami e ON s.matricola = e.studente AND e.corso = 'C1'
WHERE e.studente IS NULL;

-- EXCEPT (solo sulle colonne chiave)
SELECT matricola FROM studenti
EXCEPT
SELECT studente FROM esami WHERE corso = 'C1';
```

❌ **Sbagliato**: `... JOIN esami e ON ... WHERE e.corso <> 'C1'` → trova chi ha fatto **almeno un esame diverso da C1** (incluso chi ha fatto anche C1) e perde chi non ha fatto esami.

### 5. "Solo" / "esclusivamente"

*Studenti che hanno sostenuto solo esami di corsi del docente D1.*

Due condizioni: **(a)** almeno un esame, **(b)** nessun esame di un docente diverso.

``` sql
SELECT * FROM studenti s
WHERE EXISTS (SELECT * FROM esami e WHERE e.studente = s.matricola)          -- (a)
  AND NOT EXISTS (SELECT * FROM esami e JOIN corsi c ON e.corso = c.codice    -- (b)
                  WHERE e.studente = s.matricola AND c.docente <> 'D1');
```

Senza (a) verrebbero inclusi anche gli studenti **senza esami** (per loro "non esiste un esame di altro docente" è vero). Leggere la traccia per decidere se vanno inclusi.

Variante con aggregati: *studenti i cui voti sono tutti ≥ 25*

``` sql
SELECT studente FROM esami GROUP BY studente HAVING MIN(voto) >= 25;
```

### 6. "Tutti" (divisione)

*Studenti che hanno sostenuto **tutti** i corsi del docente D1.*

Riformulazione: "studenti per cui **non esiste** un corso di D1 che **non** abbiano sostenuto".

``` sql
-- doppio NOT EXISTS
SELECT * FROM studenti s
WHERE NOT EXISTS (
    SELECT * FROM corsi c
    WHERE c.docente = 'D1'
      AND NOT EXISTS (SELECT * FROM esami e
                      WHERE e.studente = s.matricola AND e.corso = c.codice)
);

-- confronto di conteggi
SELECT e.studente
FROM esami e JOIN corsi c ON e.corso = c.codice
WHERE c.docente = 'D1'
GROUP BY e.studente
HAVING COUNT(DISTINCT e.corso) = (SELECT COUNT(*) FROM corsi WHERE docente = 'D1');
```

⚠️ **Caso limite**: se D1 **non ha corsi**, il doppio `NOT EXISTS` restituisce **tutti** gli studenti (verità "a vuoto"), il confronto di conteggi **nessuno**. Usare `COUNT(DISTINCT …)` se lo stesso corso può comparire più volte (es. più tentativi d'esame).

### 7. "Almeno N" / "esattamente N"

``` sql
-- Studenti con almeno 2 esami
SELECT studente FROM esami GROUP BY studente HAVING COUNT(*) >= 2;

-- Alternativa con self join (solo per N = 2)
SELECT DISTINCT e1.studente
FROM esami e1 JOIN esami e2 ON e1.studente = e2.studente AND e1.corso <> e2.corso;

-- Studenti con esattamente un esame
SELECT studente FROM esami GROUP BY studente HAVING COUNT(*) = 1;

-- Docenti con almeno 2 corsi DIVERSI che hanno esami con voto 30
SELECT c.docente
FROM corsi c JOIN esami e ON c.codice = e.corso
WHERE e.voto = 30
GROUP BY c.docente
HAVING COUNT(DISTINCT c.codice) >= 2;    -- DISTINCT: lo stesso corso ha più esami da 30
```

⚠️ "Meno di N" / "al più N" deve includere chi ha **0**: serve il `LEFT JOIN` (§12) o `NOT IN`.

### 8. Massimo / minimo

``` sql
-- Esame/i con il voto più alto (tutti i pari merito)
SELECT * FROM esami WHERE voto = (SELECT MAX(voto) FROM esami);
SELECT * FROM esami WHERE voto >= ALL (SELECT voto FROM esami WHERE voto IS NOT NULL);

-- Per ogni corso, lo studente col voto più alto (correlata)
SELECT e1.corso, e1.studente, e1.voto
FROM esami e1
WHERE e1.voto = (SELECT MAX(e2.voto) FROM esami e2 WHERE e2.corso = e1.corso);

-- Secondo voto più alto
SELECT MAX(voto) FROM esami WHERE voto < (SELECT MAX(voto) FROM esami);

-- Corso con più esami sostenuti
SELECT corso FROM esami
GROUP BY corso
HAVING COUNT(*) >= ALL (SELECT COUNT(*) FROM esami GROUP BY corso);

-- Studente con la media più alta (con vista o sub-query nel FROM)
SELECT studente FROM esami
GROUP BY studente
HAVING AVG(voto) >= ALL (SELECT AVG(voto) FROM esami GROUP BY studente);
```

⚠️ Il solo `SELECT corso, MAX(n) …` **non** funziona per sapere *chi* ha il massimo. E `ORDER BY … LIMIT 1` prende **un solo** risultato anche in caso di pareggi (e non fa parte del programma).

### 9. Confronto con la media

``` sql
-- Esami con voto sopra la media generale
SELECT * FROM esami WHERE voto > (SELECT AVG(voto) FROM esami);

-- Esami con voto sopra la media DEL PROPRIO CORSO (correlata)
SELECT * FROM esami e1
WHERE e1.voto > (SELECT AVG(e2.voto) FROM esami e2 WHERE e2.corso = e1.corso);
```

### 10. Coppie (self join)

*Coppie di studenti residenti nella stessa città.*

``` sql
SELECT s1.matricola, s2.matricola
FROM studenti s1 JOIN studenti s2
  ON s1.citta_residenza = s2.citta_residenza
 AND s1.matricola < s2.matricola;
```

- `<>` restituisce ogni coppia **due volte** (A,B) e (B,A); `<` una volta sola.
- Senza la seconda condizione compaiono anche le coppie (A,A).
- Studenti con città `NULL` non formano coppie (`NULL = NULL` non è vero).

### 11. "Sia A che B" / "A ma non B"

``` sql
-- Studenti che hanno sostenuto sia C1 che C2
SELECT studente FROM esami WHERE corso = 'C1'
INTERSECT
SELECT studente FROM esami WHERE corso = 'C2';

SELECT * FROM studenti
WHERE matricola IN (SELECT studente FROM esami WHERE corso = 'C1')
  AND matricola IN (SELECT studente FROM esami WHERE corso = 'C2');

-- Studenti che hanno sostenuto C1 ma non C2
SELECT studente FROM esami WHERE corso = 'C1'
EXCEPT
SELECT studente FROM esami WHERE corso = 'C2';
```

❌ `WHERE corso = 'C1' AND corso = 'C2'` → sempre vuoto (una riga ha un solo corso).

### 12. Outer join: trappole

#### a. Contare includendo gli zeri

``` sql
-- Per ogni studente, numero di esami (0 per chi non ne ha)
SELECT s.matricola, COUNT(e.corso) AS n_esami     -- NON COUNT(*), che darebbe 1
FROM studenti s LEFT JOIN esami e ON s.matricola = e.studente
GROUP BY s.matricola;
```

#### b. Filtro sulla tabella destra: `ON` vs `WHERE`

*Per ogni studente, numero di esami con voto ≥ 28 (anche 0).*

``` sql
-- CORRETTO: la condizione nell'ON non elimina gli studenti
SELECT s.matricola, COUNT(e.corso)
FROM studenti s LEFT JOIN esami e ON s.matricola = e.studente AND e.voto >= 28
GROUP BY s.matricola;

-- SBAGLIATO: il WHERE scarta le righe con e.voto NULL → il LEFT JOIN diventa un INNER JOIN
SELECT s.matricola, COUNT(e.corso)
FROM studenti s LEFT JOIN esami e ON s.matricola = e.studente
WHERE e.voto >= 28
GROUP BY s.matricola;
```

Regola: nel `LEFT JOIN`, condizioni sulla tabella di **destra** → nell'`ON`; condizioni sulla tabella di **sinistra** → nel `WHERE`.

#### c. Catena di join dopo un outer join

`studenti LEFT JOIN esami … JOIN corsi …`: il secondo join **interno** elimina gli studenti senza esami (`e.corso` è `NULL`). Usare `LEFT JOIN` anche per le tabelle successive.

### 13. Altre trappole frequenti

- **`NATURAL JOIN` con colonne omonime non volute**: `studenti NATURAL JOIN corsi` confronta anche `nome` (nome studente = nome corso!) → risultato vuoto o sbagliato. Preferire `JOIN … ON` o `USING (colonna)`.
- **Duplicati dai join**: se la traccia chiede "studenti" e non "esami", aggiungere `DISTINCT` o usare `IN`/`EXISTS`.
- **Ambiguità di colonne**: con colonne omonime in tabelle diverse (es. `studenti.nome` e `corsi.nome`) serve il prefisso, altrimenti errore *"il riferimento alla colonna è ambiguo"*.
- **Stringhe**: apici singoli `'C1'`; i doppi apici `"C1"` indicano un **nome di colonna**. Il confronto è case-sensitive: `'cagliari' ≠ 'Cagliari'`.
- **`AND`/`OR` senza parentesi**: `WHERE anno = 1 OR anno = 2 AND citta_residenza = 'Cagliari'` significa `anno = 1 OR (anno = 2 AND …)`. Mettere sempre le parentesi.
- **`LIKE`**: `_` = un carattere, `%` = zero o più caratteri. `LIKE 'A%'` è case-sensitive.
- **`BETWEEN` è inclusivo**: `voto BETWEEN 24 AND 28` include 24 e 28. Per le date di un anno: `BETWEEN '2001-01-01' AND '2001-12-31'`.
- **Divisione intera**: `voto * 100 / 30` con interi tronca. Usare `voto * 100.0 / 30` per i decimali.
- **`NULL`**: `= NULL` non funziona mai; `<>` e `NOT IN` scartano/bloccano i `NULL`. Vedi [[NULL e logica a tre valori]].

### 14. Checklist prima di consegnare

1. Ogni tabella nel `FROM` ha la sua condizione di join? (altrimenti prodotto cartesiano)
2. La traccia chiede entità (studenti) o occorrenze (esami)? → `DISTINCT`?
3. Ci sono negazioni ("non", "mai", "solo", "tutti")? → `NOT EXISTS`/`NOT IN`, non `<>` nel join.
4. Vanno inclusi i casi con zero occorrenze? → `LEFT JOIN` + `COUNT(colonna)`.
5. Le colonne usate in `NOT IN` / `ALL` possono essere `NULL`?
6. Ogni attributo della `SELECT` è nel `GROUP BY` o dentro un aggregato?
7. Le condizioni sugli aggregati sono nell'`HAVING`?
8. I pari merito sul massimo sono gestiti (niente `LIMIT 1`)?
9. Le tabelle hanno il prefisso dello schema (`segreteria.studenti`)?
