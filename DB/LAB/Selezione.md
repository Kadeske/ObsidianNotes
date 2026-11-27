Schema di riferimento (schema `segreteria`):
- `studenti(matricola, cognome, nome, citta_nascita, citta_residenza, anno, data_nascita)`
- `corsi(codice, nome, docente)`
- `esami(studente, corso, data, voto)`, con FK `studente → studenti`, `corso → corsi`

### 1. Proprietà del linguaggio SQL

- **Dichiarativo**: si descrive *cosa* si vuole (proprietà del risultato), non *come* ottenerlo (a differenza dell'algebra relazionale, procedurale).
- **Query optimizer**: il DBMS sceglie da solo la strategia di esecuzione migliore.
- **Relazionalmente completo**: esprime tutte le interrogazioni dell'algebra relazionale.
- **Multiset (bag)**: tabelle e risultati possono contenere **righe duplicate**.

### 2. Struttura di `SELECT`

``` sql
SELECT [DISTINCT] ListaAttributi     -- colonne, espressioni, *
FROM ListaTabelle                    -- tabelle o viste
[WHERE Condizione]                   -- combinazione booleana di predicati
[ORDER BY ListaOrder];
```

Significato in algebra relazionale:

$$\pi_{\text{ListaAttributi}} (\sigma_{\text{Condizione}} (T_1 \times \dots \times T_n))$$

1. prodotto cartesiano delle tabelle nel `FROM`;
2. il `WHERE` tiene solo le righe per cui la condizione è **`TRUE`** (scarta `FALSE` **e** `UNKNOWN`);
3. la `SELECT` proietta le colonne indicate (`*` = tutte);
4. senza `WHERE` tutte le righe fanno parte del risultato.

> Differenza con la proiezione $\pi$ dell'algebra: la `SELECT` **non elimina i duplicati** (serve `DISTINCT`).

### 3. Predicati del `WHERE`

| Operatore | Sintassi | Note |
| --- | --- | --- |
| Confronto | `=`, `<>`, `<`, `<=`, `>`, `>=` | tra colonne, espressioni o costanti |
| Intervallo | `attr [NOT] BETWEEN v1 AND v2` | **estremi inclusi**; se `v1 > v2` → sempre falso |
| Insieme | `attr [NOT] IN (v1, v2, ...)` | equivale a una catena di `=` in `OR` |
| Quantificati | `attr OP ANY / ALL ('{v1,v2,...}')` | in PostgreSQL serve un **array** (vedi sotto) |
| Stringhe | `attr [NOT] LIKE 'pattern'` | `_` = un carattere, `%` = zero o più caratteri |
| Valore nullo | `attr IS [NOT] NULL` | **mai** `= NULL` |
| Logici | `AND`, `OR`, `NOT` | usare le parentesi |

#### Confronti

``` sql
-- Matricola e cognome degli studenti del secondo anno
SELECT matricola, cognome FROM studenti WHERE anno = 2;

-- Studenti che risiedono nella città in cui sono nati (confronto tra colonne)
SELECT * FROM studenti WHERE citta_residenza = citta_nascita;

-- Esami con voto tra 24 e 28 (estremi inclusi): le due forme sono equivalenti
SELECT * FROM esami WHERE voto >= 24 AND voto <= 28;
SELECT * FROM esami WHERE voto BETWEEN 24 AND 28;
```

#### `IN` e `ANY`/`ALL` con liste di costanti

``` sql
-- Esami con voto 29, 30 o 33 (lode)
SELECT * FROM esami WHERE voto IN (29, 30, 33);
SELECT * FROM esami WHERE voto = ANY ('{29,30,33}');    -- PostgreSQL: array
SELECT * FROM esami WHERE voto = ANY (ARRAY[29,30,33]); -- forma equivalente

-- Esami con voto diverso da 29, 30 e 33
SELECT * FROM esami WHERE voto NOT IN (29, 30, 33);
SELECT * FROM esami WHERE voto <> ALL ('{29,30,33}');
```

- `= ANY` ≡ `IN`; `<> ALL` ≡ `NOT IN`.
- `voto = ANY (29, 30, 33)` (sintassi delle slide) in PostgreSQL dà **errore di sintassi**.
- Con le sub-query invece si scrive normalmente `voto = ANY (SELECT ...)` → [[Query nidificate]].

#### `LIKE`

| Pattern | Significato | Esempi che corrispondono |
| --- | --- | --- |
| `'A%o'` | inizia per A e finisce per o | `Antonio`, `Ao` |
| `'__r%'` | `r` come terza lettera | `Ferru`, `Carta` |
| `'%ar%'` | contiene "ar" | `Marco`, `Sardegna` |
| `'_____'` | esattamente 5 caratteri | `Rossi` |
| `'%'` | qualsiasi stringa non `NULL` | tutte tranne `NULL` |
| `'Rossi'` | senza jolly = uguaglianza | solo `Rossi` |

``` sql
-- Studenti il cui nome inizia con 'A' e termina con 'o'
SELECT * FROM studenti WHERE nome LIKE 'A%o';

-- Studenti con 'r' come terza lettera del cognome
SELECT * FROM studenti WHERE cognome LIKE '__r%';
```

- **Case-sensitive**: `'B%'` non trova `bianchi`. *Extra PostgreSQL*: `ILIKE` ignora maiuscole/minuscole.
- Per cercare un `_` o `%` letterale bisogna farlo precedere da `\`: `LIKE '%\_%'` (con `'%_%'` corrisponde qualsiasi stringa non vuota).
- `LIKE` lavora su stringhe: sulle date si usano confronti o `BETWEEN`.

#### `IS NULL`

``` sql
-- Studenti con città di residenza non specificata
SELECT * FROM studenti WHERE citta_residenza IS NULL;
```

Comportamento di `NULL` con `<>`, `NOT`, `IN`, `NOT IN` → [[NULL e logica a tre valori]].

#### `AND`, `OR`, `NOT` e parentesi

Le slide dicono che la sintassi non fissa una precedenza tra `AND` e `OR`. In PostgreSQL (e nello standard) **`AND` ha precedenza su `OR`**, ma conviene **sempre** mettere le parentesi:

``` sql
-- Studenti del 1° o 2° anno che risiedono a Cagliari
SELECT * FROM studenti
WHERE anno = 1 OR anno = 2 AND citta_residenza = 'Cagliari';     -- SBAGLIATO: = anno 1 (ovunque) OR (anno 2 a Cagliari)

SELECT * FROM studenti
WHERE (anno = 1 OR anno = 2) AND citta_residenza = 'Cagliari';   -- CORRETTO
SELECT * FROM studenti
WHERE anno IN (1, 2) AND citta_residenza = 'Cagliari';           -- CORRETTO e più compatto
```

### 4. Espressioni e alias

``` sql
-- Espressioni calcolate con alias
SELECT cognome, (stipendio + premio) AS entrate FROM impiegati ORDER BY entrate;

-- Voto in centesimi
SELECT studente, data, voto * 100 / 30 AS centesimi FROM esami;     -- divisione INTERA (troncata)
SELECT studente, data, voto * 100.0 / 30 AS centesimi FROM esami;   -- con decimali

-- Alias di colonna e di tabella
SELECT s.cognome, s.nome, s.citta_residenza AS citta
FROM studenti AS s            -- AS facoltativo: "studenti s"
WHERE s.anno = 1;
```

- **Alias di colonna**: utilizzabile in `ORDER BY`, **non** nel `WHERE` (*"la colonna … non esiste"*): nel `WHERE` va ripetuta l'espressione.
- **Alias di tabella**: indispensabile quando la stessa tabella compare due volte (self join, sub-query correlate). Una volta definito l'alias, il nome originale **non** si può più usare in quella query (`FROM studenti s WHERE studenti.anno = 1` → errore).
- **Operatore punto**: `tabella.colonna` o `alias.colonna`, obbligatorio se la colonna esiste in più tabelle del `FROM` (es. `studenti.nome` e `corsi.nome`).

### 5. Duplicati (`DISTINCT` / `ALL`)

``` sql
SELECT DISTINCT citta_residenza FROM studenti;          -- elenco delle città, senza ripetizioni
SELECT DISTINCT citta_residenza, anno FROM studenti;    -- coppie (città, anno) distinte
```

- Default `ALL`: i duplicati restano (eliminarli è costoso; con gli aggregati spesso servono).
- `DISTINCT` si scrive **una volta, subito dopo `SELECT`**, e si applica all'**intera riga** selezionata, non alla prima colonna.
- Più `NULL` vengono considerati uguali: compare un solo `NULL`.
- Selezionando la chiave primaria `DISTINCT` è inutile (le righe sono già tutte diverse).

### 6. Ordinamento (`ORDER BY`)

``` sql
-- Studenti di Cagliari in ordine di anno (crescente è il default)
SELECT * FROM studenti WHERE citta_residenza = 'Cagliari' ORDER BY anno;

-- Esami di C1 per voto in centesimi decrescente, a parità per matricola crescente
SELECT studente, data, (100 * voto) / 30
FROM esami WHERE corso = 'C1'
ORDER BY 3 DESC, studente;          -- 3 = terza espressione della SELECT
```

- Ordinamento gerarchico: a parità del primo criterio si usa il secondo, e così via.
- `ASC` (default) / `DESC` vanno indicati **per ogni** criterio: `ORDER BY a DESC, b` ordina `b` in modo crescente.
- Si può ordinare per nome di colonna, alias, posizione (`ORDER BY 1`) o espressione. Posizione inesistente → errore.
- Un alias **dentro un'espressione** non funziona: `ORDER BY entrate + 1` → errore.
- Con `SELECT DISTINCT` si può ordinare solo per colonne presenti nella `SELECT`.
- `NULL` in fondo con `ASC`, in cima con `DESC` (*extra PostgreSQL*: `NULLS FIRST` / `NULLS LAST`).
- Senza `ORDER BY` l'ordine delle righe **non è garantito**.

#### Tabella usata negli esempi

| matricola | cognome | nome | citta_residenza | anno |
| --- | --- | --- | --- | --- |
| S1 | Rossi | Mario | Cagliari | 2 |
| S2 | Bianchi | Anna | Sassari | 1 |
| S3 | Rossi | Luca | Cagliari | 1 |
| S4 | Verdi | Sara | NULL | 3 |
| S5 | Atzeni | Paolo | Cagliari | 2 |

#### Esempi con un campo

``` sql
-- Studenti in ordine alfabetico di cognome
SELECT cognome, nome FROM studenti ORDER BY cognome;
```

| cognome | nome |
| --- | --- |
| Atzeni | Paolo |
| Bianchi | Anna |
| Rossi | Mario *(oppure Luca)* |
| Rossi | Luca *(oppure Mario)* |
| Verdi | Sara |

> ⚠️ A parità di cognome l'ordine tra i due Rossi **non è garantito**: può cambiare da un'esecuzione all'altra. Se la traccia vuole un ordine preciso anche a parità, serve un secondo campo (vedi sotto).

``` sql
-- Studenti dall'anno più alto al più basso
SELECT matricola, anno FROM studenti ORDER BY anno DESC;
-- S4 (3), poi S1 e S5 (2) in ordine qualsiasi, poi S2 e S3 (1) in ordine qualsiasi

-- Ordinamento su una colonna con NULL
SELECT matricola, citta_residenza FROM studenti ORDER BY citta_residenza;
-- Cagliari (S1, S3, S5), Sassari (S2), NULL (S4) in fondo
-- con ORDER BY citta_residenza DESC il NULL sarebbe in cima

-- Ordinamento per un'espressione, tramite alias
SELECT studente, voto * 100 / 30 AS centesimi FROM esami ORDER BY centesimi DESC;

-- Ordinamento per posizione: 2 = seconda colonna della SELECT (nome)
SELECT cognome, nome FROM studenti ORDER BY 2;

-- Ordinamento per una colonna che non è nella SELECT (permesso senza DISTINCT)
SELECT cognome FROM studenti ORDER BY anno;
```

#### Esempi con due o più campi

Il **secondo** campo conta solo **a parità del primo**, il terzo solo a parità dei primi due, e così via. `ASC`/`DESC` si scrivono separatamente per ogni campo.

``` sql
-- Cognome e, a parità di cognome, nome
SELECT cognome, nome FROM studenti ORDER BY cognome, nome;
```

| cognome | nome |
| --- | --- |
| Atzeni | Paolo |
| Bianchi | Anna |
| Rossi | **Luca** |
| Rossi | **Mario** |
| Verdi | Sara |

Ora l'ordine dei due Rossi è garantito: Luca prima di Mario.

``` sql
-- Anno decrescente e, a parità di anno, cognome crescente
SELECT cognome, nome, anno FROM studenti ORDER BY anno DESC, cognome;
```

| cognome | nome | anno |
| --- | --- | --- |
| Verdi | Sara | 3 |
| Atzeni | Paolo | 2 |
| Rossi | Mario | 2 |
| Bianchi | Anna | 1 |
| Rossi | Luca | 1 |

`DESC` vale **solo** per `anno`: `cognome` resta crescente (A → Z) dentro ogni anno.

``` sql
-- Stesso esempio con i versi invertiti: anno crescente, cognome decrescente
SELECT cognome, nome, anno FROM studenti ORDER BY anno, cognome DESC;
```

| cognome | nome | anno |
| --- | --- | --- |
| Rossi | Luca | 1 |
| Bianchi | Anna | 1 |
| Rossi | Mario | 2 |
| Atzeni | Paolo | 2 |
| Verdi | Sara | 3 |

``` sql
-- Tre campi: città, poi anno decrescente, poi cognome
SELECT citta_residenza, anno, cognome FROM studenti
ORDER BY citta_residenza, anno DESC, cognome;
```

| citta_residenza | anno | cognome |
| --- | --- | --- |
| Cagliari | 2 | Atzeni |
| Cagliari | 2 | Rossi |
| Cagliari | 1 | Rossi |
| Sassari | 1 | Bianchi |
| NULL | 3 | Verdi |

Dentro Cagliari si ordina per anno (2 prima di 1); dentro "Cagliari, anno 2" si ordina per cognome.

``` sql
-- Due campi misti: nome di colonna e posizione
SELECT cognome, nome, anno FROM studenti ORDER BY 3 DESC, cognome;   -- = ORDER BY anno DESC, cognome

-- Con aggregati: studenti per media decrescente, a parità per matricola
SELECT studente, AVG(voto) AS media
FROM esami
GROUP BY studente
ORDER BY media DESC, studente;

-- Con join: esami per nome del corso, poi per voto decrescente
SELECT c.nome AS corso, s.cognome, e.voto
FROM esami e
JOIN corsi c ON e.corso = c.codice
JOIN studenti s ON e.studente = s.matricola
ORDER BY c.nome, e.voto DESC;
```

#### Errori tipici

| Scritto | Problema | Corretto |
| --- | --- | --- |
| `ORDER BY anno ORDER BY cognome` | un solo `ORDER BY` per query → errore di sintassi | `ORDER BY anno, cognome` |
| `ORDER BY cognome, nome DESC` pensando che siano entrambi decrescenti | `DESC` vale solo per `nome` | `ORDER BY cognome DESC, nome DESC` |
| `ORDER BY anno DESC cognome` | manca la virgola → errore | `ORDER BY anno DESC, cognome` |
| `ORDER BY nome, cognome` per "ordinare per cognome e nome" | l'ordine dei campi conta: ordina prima per nome | `ORDER BY cognome, nome` |
| `ORDER BY` prima di `WHERE` o `GROUP BY` | `ORDER BY` va sempre **per ultimo** | `... WHERE ... GROUP BY ... HAVING ... ORDER BY ...` |

### 7. Costanti: stringhe e date

- Stringhe tra **apici singoli**: `'Cagliari'`. I doppi apici indicano un **identificatore**: `WHERE citta = "Cagliari"` → *"la colonna Cagliari non esiste"*.
- Confronto tra stringhe **case-sensitive**: `'Cagliari' <> 'cagliari'`.
- Date nel formato ISO `'AAAA-MM-GG'`, confrontabili con `=`, `<`, `BETWEEN`.

``` sql
-- Tutti gli studenti nati nel 2001 (forme equivalenti)
SELECT * FROM studenti WHERE data_nascita BETWEEN '2001-01-01' AND '2001-12-31';
SELECT * FROM studenti WHERE data_nascita >= '2001-01-01' AND data_nascita < '2002-01-01';
SELECT * FROM studenti WHERE EXTRACT(YEAR FROM data_nascita) = 2001;   -- extra PostgreSQL

-- Esami sostenuti a gennaio 2026 con voto almeno 27
SELECT * FROM esami
WHERE data BETWEEN '2026-01-01' AND '2026-01-31' AND voto >= 27;
```

> Con colonne `TIMESTAMP` (data + ora) `BETWEEN '2001-01-01' AND '2001-12-31'` **esclude** quasi tutto il 31 dicembre (si ferma alle 00:00). Usare `< '2002-01-01'`.

### 8. Selezione su più tabelle (anticipazione)

``` sql
SELECT studenti.cognome, studenti.nome, esami.corso, esami.voto
FROM studenti, esami
WHERE esami.studente = studenti.matricola;
```

Senza la condizione `esami.studente = studenti.matricola` si otterrebbe il prodotto cartesiano (ogni studente abbinato a ogni esame). Join in dettaglio → [[Operazioni di combinazione]].
