`NULL` **non è un valore**: rappresenta l'assenza di informazione (valore non noto o non applicabile). È compatibile con qualsiasi tipo di dato.

### 1. Le tre regole fondamentali

| Situazione | Risultato | Esempio |
| --- | --- | --- |
| `NULL` in un **confronto** | `UNKNOWN` | `NULL = 5`, `NULL = NULL`, `NULL <> NULL` → `UNKNOWN` |
| `NULL` in un'**operazione algebrica** o concatenazione | `NULL` | `NULL + 1` → `NULL`, `'x' \|\| NULL` → `NULL` |
| Due `NULL` | considerati **diversi** tra loro (in generale) | `NULL = NULL` non è `TRUE` |

> In PostgreSQL `UNKNOWN` di tipo `BOOLEAN` coincide con `NULL` (viene mostrato come cella vuota).

### 2. Tabelle di verità (logica a tre valori)

**`NOT`**

| x | NOT x |
| --- | --- |
| TRUE | FALSE |
| FALSE | TRUE |
| UNKNOWN | UNKNOWN |

**`AND`** (basta un `FALSE` per avere `FALSE`)

| AND | TRUE | FALSE | UNKNOWN |
| --- | --- | --- | --- |
| **TRUE** | TRUE | FALSE | UNKNOWN |
| **FALSE** | FALSE | FALSE | FALSE |
| **UNKNOWN** | UNKNOWN | FALSE | UNKNOWN |

**`OR`** (basta un `TRUE` per avere `TRUE`)

| OR | TRUE | FALSE | UNKNOWN |
| --- | --- | --- | --- |
| **TRUE** | TRUE | TRUE | TRUE |
| **FALSE** | TRUE | FALSE | UNKNOWN |
| **UNKNOWN** | TRUE | UNKNOWN | UNKNOWN |

> **Regola chiave del `WHERE`**: passano **solo** le righe per cui la condizione è `TRUE`. Le righe con `FALSE` **e** quelle con `UNKNOWN` vengono scartate.

### 3. Come testare `NULL`

``` sql
-- CORRETTO
SELECT * FROM studenti WHERE citta_residenza IS NULL;
SELECT * FROM studenti WHERE citta_residenza IS NOT NULL;

-- SBAGLIATO: restituisce sempre 0 righe (il confronto vale UNKNOWN)
SELECT * FROM studenti WHERE citta_residenza = NULL;
```

### 4. Casi limite

#### a. La negazione non "recupera" i `NULL`

``` sql
-- Cittadini con stipendio > 25000
SELECT * FROM cittadini WHERE stipendio > 25000;
-- Cittadini con stipendio <= 25000
SELECT * FROM cittadini WHERE NOT (stipendio > 25000);
```

Chi ha `stipendio = NULL` **non compare in nessuna delle due**: `NOT UNKNOWN = UNKNOWN`. Le due query **non** partizionano la tabella.
Per includerli: `WHERE stipendio <= 25000 OR stipendio IS NULL`.

#### b. `<>` esclude i `NULL`

``` sql
-- Studenti NON residenti a Cagliari
SELECT * FROM studenti WHERE citta_residenza <> 'Cagliari';
```

Gli studenti con città `NULL` **non** compaiono. Se la traccia dice "che non risultano residenti a Cagliari" aggiungere `OR citta_residenza IS NULL`.

#### c. Condizioni composte con `NULL`

Mario Rossi: `dipartimento = 'Informatica'`, `stipendio = NULL`.

``` sql
WHERE dipartimento = 'Informatica' AND stipendio > 20000   -- TRUE AND UNKNOWN = UNKNOWN → escluso
WHERE dipartimento = 'Informatica' OR  stipendio > 20000   -- TRUE OR UNKNOWN  = TRUE    → incluso
```

#### d. `NOT IN` con un `NULL` nella lista → **0 righe** ⚠️

``` sql
SELECT * FROM studenti WHERE anno NOT IN (2, NULL);   -- sempre vuoto!
```

`anno NOT IN (2, NULL)` ≡ `anno <> 2 AND anno <> NULL` → al massimo `TRUE AND UNKNOWN = UNKNOWN`.
Vale anche con le sottoquery: se `SELECT voto FROM esami` contiene anche un solo `NULL`, allora `x NOT IN (SELECT voto FROM esami)` non è mai vero. Soluzione: filtrare `WHERE voto IS NOT NULL` nella sottoquery oppure usare `NOT EXISTS`. Vedi [[Query nidificate]].

#### e. `IN` con `NULL`, invece, funziona "normalmente"

`3 IN (3, NULL)` → `TRUE` (basta un `TRUE` nell'`OR`). `4 IN (3, NULL)` → `UNKNOWN` (riga scartata, come se fosse `FALSE`).

#### f. `NULL` nei vincoli

| Vincolo | Comportamento con `NULL` |
| --- | --- |
| `CHECK (voto BETWEEN 18 AND 30)` | `NULL` **accettato**: il `CHECK` fallisce solo se la condizione è `FALSE`, non se è `UNKNOWN` |
| `UNIQUE (a)` | Ammessi **più** `NULL` (sono "diversi" tra loro) |
| `UNIQUE (b, c)` | Ammesse più righe `(1, NULL)`: basta un `NULL` nella combinazione |
| `PRIMARY KEY` | `NULL` **vietato** (implica `NOT NULL`) |
| `FOREIGN KEY` | `NULL` **accettato** e non controllato. Con FK composta basta un attributo `NULL` per saltare il controllo |

#### g. `NULL` in ordinamento, raggruppamento, insiemi

- **`ORDER BY`** (PostgreSQL): i `NULL` sono considerati **più grandi** di ogni valore → in fondo con `ASC`, in cima con `DESC`.
- **`GROUP BY`**: tutti i `NULL` finiscono in **un unico gruppo**.
- **`DISTINCT`, `UNION`, `INTERSECT`, `EXCEPT`**: due `NULL` sono considerati **uguali** (vengono deduplicati / fatti corrispondere). È l'opposto del comportamento di `=` e `NOT IN`.
- **Funzioni aggregate**: ignorano i `NULL` (tranne `COUNT(*)`). Vedi [[Funzioni aggregate]].

#### h. `NULL` e outer join

Le colonne della tabella "mancante" in un `LEFT/RIGHT/FULL JOIN` vengono riempite con `NULL`: un `WHERE` su quelle colonne può eliminare proprio le righe aggiunte dall'outer join. Vedi [[Pattern di query]].
