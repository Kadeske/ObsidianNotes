Schema di riferimento: `studenti(matricola, cognome, nome, citta_residenza, anno)`, `corsi(codice, nome, docente)`, `esami(studente, corso, data, voto)`, `impiegati(codice, nome, eta, dipart, stipendio)`.

### 1. Sintassi e ordine di valutazione

``` sql
SELECT    ...                 -- 5. proiezione (attributi del GROUP BY + aggregati)
FROM      ...                 -- 1. prodotto/join delle tabelle
WHERE     condizione_righe    -- 2. filtra le RIGHE (niente aggregati)
GROUP BY  ListaGroup          -- 3. forma i gruppi
HAVING    condizione_gruppi   -- 4. filtra i GRUPPI (con aggregati)
ORDER BY  ...                 -- 6. ordina il risultato
```

Il risultato contiene **una riga per gruppo**.

### 2. Cosa può stare nella `SELECT` (e nell'`HAVING`)

Solo:
- attributi presenti nel **`GROUP BY`** (hanno lo stesso valore in tutto il gruppo);
- **funzioni aggregate** (un valore per gruppo).

``` sql
SELECT dipart, MAX(eta) FROM impiegati GROUP BY dipart;         -- OK
SELECT dipart, nome, MAX(eta) FROM impiegati GROUP BY dipart;   -- ERRORE: "nome" non è nel GROUP BY
```

> **Eccezione PostgreSQL**: se si raggruppa per la **chiave primaria** di una tabella, si possono selezionare anche gli altri attributi di quella tabella (dipendono funzionalmente dalla PK).
> `SELECT s.matricola, s.cognome, COUNT(*) FROM studenti s JOIN esami e ON ... GROUP BY s.matricola` → **OK**.
> Raggruppando per un attributo che non è chiave (es. `s.nome`) → errore. Nel dubbio aggiungere al `GROUP BY` tutti gli attributi selezionati: `GROUP BY s.matricola, s.cognome`.

L'inverso è lecito: un attributo può stare nel `GROUP BY` senza comparire nella `SELECT`.

### 3. Esempio passo-passo

| codice | nome | eta | dipart |
| --- | --- | --- | --- |
| IMP1 | Anna | 29 | DIP1 |
| IMP2 | Alice | 43 | DIP1 |
| IMP3 | Barbara | 27 | DIP2 |
| IMP4 | Bruno | 23 | DIP2 |
| IMP5 | Carlo | 31 | DIP2 |
| IMP6 | Camilla | 44 | DIP3 |

``` sql
SELECT dipart, MAX(eta), COUNT(*)
FROM impiegati
WHERE eta > 28           -- scarta Barbara e Bruno PRIMA di raggruppare
GROUP BY dipart
HAVING COUNT(*) > 1;     -- tiene solo i gruppi con più di 1 impiegato (rimasto)
```

| Fase | Risultato |
| --- | --- |
| `WHERE` | Anna, Alice, Carlo, Camilla |
| `GROUP BY` | DIP1 {Anna, Alice} · DIP2 {Carlo} · DIP3 {Camilla} |
| `HAVING` | DIP1 |
| `SELECT` | `DIP1 · 43 · 2` |

Senza `WHERE`, DIP2 avrebbe 3 impiegati e passerebbe l'`HAVING`: **la posizione della condizione cambia il risultato**.

### 4. `WHERE` vs `HAVING`

| | `WHERE` | `HAVING` |
| --- | --- | --- |
| Filtra | singole righe | gruppi |
| Quando | prima del raggruppamento | dopo |
| Aggregati | ❌ vietati | ✔ ammessi |
| Attributi | qualsiasi colonna | solo attributi del `GROUP BY` (o dentro aggregati) |

``` sql
-- Numero di esami per ogni voto tra 27 e 30
SELECT voto, COUNT(*) FROM esami GROUP BY voto HAVING voto BETWEEN 27 AND 30;   -- lecito
SELECT voto, COUNT(*) FROM esami WHERE voto BETWEEN 27 AND 30 GROUP BY voto;    -- equivalente e più efficiente
```

Regola pratica: condizione su un **singolo esame/riga** → `WHERE`; condizione su **conteggi, somme, medie del gruppo** → `HAVING`.

### 5. Esempi

``` sql
-- Somma degli stipendi per dipartimento
SELECT dipart, SUM(stipendio) FROM impiegati GROUP BY dipart;

-- Voto massimo e minimo di ogni studente
SELECT studente, MAX(voto), MIN(voto) FROM esami GROUP BY studente;

-- Media dei voti per ogni corso sostenuto da più di due studenti
SELECT corso, AVG(voto) FROM esami GROUP BY corso HAVING COUNT(*) > 2;

-- Studenti che hanno sostenuto almeno due esami
SELECT studente FROM esami GROUP BY studente HAVING COUNT(corso) >= 2;

-- Studenti con media >= 27, ordinati per media decrescente
SELECT studente, AVG(voto) AS media
FROM esami
GROUP BY studente
HAVING AVG(voto) >= 27
ORDER BY media DESC;

-- Raggruppamento su più attributi: numero di studenti per città e anno
SELECT citta_residenza, anno, COUNT(*)
FROM studenti
GROUP BY citta_residenza, anno;

-- GROUP BY dopo un join: cognome e numero di esami di ogni studente
SELECT s.matricola, s.cognome, COUNT(*) AS n_esami
FROM studenti s JOIN esami e ON s.matricola = e.studente
GROUP BY s.matricola, s.cognome;

-- Docenti che hanno più di 2 corsi con media voti superiore a 25
SELECT c.docente
FROM corsi c
WHERE c.codice IN (SELECT corso FROM esami GROUP BY corso HAVING AVG(voto) > 25)
GROUP BY c.docente
HAVING COUNT(*) > 2;
```

### 6. `HAVING` con sub-query (massimo dei gruppi)

``` sql
-- Studente che ha sostenuto il maggior numero di esami
SELECT studente
FROM esami
GROUP BY studente
HAVING COUNT(*) >= ALL (SELECT COUNT(*) FROM esami GROUP BY studente);

-- Dipartimento che spende di più in stipendi
SELECT dipart
FROM impiegati
GROUP BY dipart
HAVING SUM(stipendio) >= ALL (SELECT SUM(stipendio) FROM impiegati GROUP BY dipart);

-- Corsi con media superiore alla media generale
SELECT corso, AVG(voto)
FROM esami
GROUP BY corso
HAVING AVG(voto) > (SELECT AVG(voto) FROM esami);
```

Restituiscono **tutti** i gruppi a pari merito. Alternativa con vista: vedi [[Viste]].

### 7. Casi limite

- **Alias nel `HAVING`/`WHERE`**: `HAVING n >= 2` con `COUNT(*) AS n` → **errore** (colonna "n" non esiste). Bisogna riscrivere l'aggregato: `HAVING COUNT(*) >= 2`. Nell'`ORDER BY` invece l'alias è ammesso: `ORDER BY n DESC`.
- **`NULL` forma un gruppo**: gli studenti con `citta_residenza = NULL` finiscono tutti in **un** gruppo con chiave `NULL`.
- **Gruppi con zero elementi non esistono**: `SELECT corso, COUNT(*) FROM esami GROUP BY corso` non mostra i corsi senza esami (non compaiono in `esami`). Per vederli con `0`:

``` sql
SELECT c.codice, COUNT(e.corso) AS n
FROM corsi c LEFT JOIN esami e ON c.codice = e.corso
GROUP BY c.codice;
```

  (con `COUNT(*)` risulterebbe 1 invece di 0)
- **"Meno di N" deve includere gli zeri**: "corsi con meno di 3 esami" → con `FROM esami GROUP BY corso HAVING COUNT(*) < 3` si perdono i corsi con 0 esami. Usare il `LEFT JOIN` di sopra + `HAVING COUNT(e.corso) < 3`.
- **`HAVING` senza `GROUP BY`**: tutta la tabella è un unico gruppo. `SELECT COUNT(*) FROM esami HAVING COUNT(*) > 100` → una riga o nessuna.
- **Join che duplicano**: se nel `FROM` ci sono più tabelle "molti", i conteggi si moltiplicano. Usare `COUNT(DISTINCT ...)`.
- **`GROUP BY` senza aggregati** equivale a `SELECT DISTINCT`: `SELECT anno FROM studenti GROUP BY anno`.
- **Raggruppare per il nome invece che per la chiave**: `GROUP BY s.cognome` unisce omonimi diversi in un solo gruppo. Raggruppare per la chiave (`s.matricola`) e mostrare il cognome.
