### 1. Proprietà Generali del Linguaggio SQL

- **Natura dichiarativa**: si specifica _cosa_ si vuole ottenere (le proprietà del risultato) e non la sequenza di passi per ottenerlo (a differenza dell'algebra relazionale che è procedurale).
- **Query Optimizer**: componente del DBMS che individua in modo autonomo la strategia ottimale di esecuzione e di accesso ai dati.
- **Relazionalmente completo**: consente di esprimere tutte le interrogazioni formulabili con l'algebra relazionale.
- **Semantica su Multiset (Bag)**: a differenza delle relazioni matematiche, le tabelle e i risultati delle query possono ammettere **duplicati** di tuple.

### 2. Struttura dell'Istruzione `SELECT`

La sintassi fondamentale per l'interrogazione è:

``` sql
SELECT ListaAttributi
FROM ListaTabelle
[WHERE Condizione];
```

- **Corrispondenza con l'algebra relazionale**:
    $$\pi_{\text{ListaAttributi}} (\sigma_{\text{Condizione}} (T_1 \times \dots \times T_n))$$
    
    1. Si genera il prodotto cartesiano delle tabelle presenti nella clausola `FROM`.
    2. La clausola `WHERE` filtra le sole tuple che soddisfano la condizione booleana (selezione $\sigma$).
    3. La clausola `SELECT` proietta esclusivamente gli attributi specificati (proiezione $\pi$). L'uso di `*` estrae tutte le colonne.
    4. In assenza della clausola `WHERE`, tutte le righe concorrono al risultato.

### 3. Predicati e Operatori nella Clausola `WHERE`

La clausola `WHERE` supporta combinazioni logiche di predicati elementari:

- **Operatori relazionali standard** (`=`, `<>`, `>`, `>=`, `<`, `<=`) tra attributi, espressioni o costanti.
- **Operatore di intervallo (`BETWEEN ... AND ...`)**:
    - Verifica l'inclusione in un intervallo chiuso: `attributo [NOT] BETWEEN v1 AND v2`.
- **Operatore di appartenenza a un insieme (`IN`)**:
    - Verifica se il valore appartiene a un elenco esplicito: `attributo [NOT] IN (v1, v2, ...)`.
- **Operatori quantificati (`ANY` / `ALL`)**:
    - Confrontano l'attributo rispetto a tutti o ad almeno uno dei valori di una lista: `attributo OP ANY/ALL (...)`.
    - _Nota per PostgreSQL_: richiede espressioni in formato array (es. `WHERE voto = ANY('{29,30,33}')`).
- **Pattern matching su stringhe (`LIKE`)**:
    - `_` (underscore): rappresenta un singolo carattere arbitrario.
    - `%` (percentuale): rappresenta una sequenza arbitraria di caratteri (anche vuota).
    - Esempi: `'A%o'` (inizia per A e finisce per o), `'__r%'` (ha 'r' come terzo carattere).
- **Verifica del valore nullo (`IS NULL` / `IS NOT NULL`)**:
    - Non si usa mai `= NULL`, bensì la sintassi dedicata `attributo IS NULL` o `attributo IS NOT NULL`.
- **Operatori booleani (`AND`, `OR`, `NOT`)**:  
    - Poiché non esiste precedenza implicita univoca tra `AND` e `OR`, è opportuno esplicitare l'ordine di valutazione usando le **parentesi tonde**.
### 4. Gestione dei Duplicati (`DISTINCT` / `ALL`)

Di default le query conservano le righe duplicate (`ALL` implicito) per ragioni di efficienza di calcolo.
``` sql
SELECT DISTINCT colonna1, colonna2
FROM tabella;
```

- **`DISTINCT`**: rimuove tutte le tuple identiche dal risultato finale.

### 5. Ordinamento del Risultato (`ORDER BY`)

Permette di specificare l'ordine delle tuple restituite:

``` sql
SELECT col1, col2, (col3 * 2) AS valore
FROM tabella
ORDER BY col1 ASC, valore DESC;
```

- **Criterio gerarchico**: le righe vengono ordinate rispetto al primo attributo; a parità di valore si procede con i successivi.

- **Modificatori di verso**:
    - `ASC`: crescente (predefinito se omesso).
    - `DESC`: decrescente.
- **Riferimenti ammessi**: l'ordinamento può essere effettuato indicando il nome della colonna, l'alias oppure la posizione numerica dell'elemento nella clausola `SELECT` (es. `ORDER BY 1 DESC`).

### 6. Ridenominazione e Qualificazione dei Nomi

- **Alias di attributo (`AS`)**: 
    - Rinomina la colonna nel risultato, utile specialmente per espressioni calcolate (es. `SELECT (Stipendio + Premio) AS Entrate`).
- **Alias di tabella**:  
    - Rinomina le tabelle nella clausola `FROM` (es. `FROM Studenti S`), utile per abbreviare o indispensabile quando si confronta una tabella con se stessa.
- **Operatore punto (`.`)**:
    - Consente di disambiguare colonne aventi lo stesso nome provenienti da tabelle diverse (es. `NomeTabella.NomeColonna` o `Alias.NomeColonna`).

#### Esempio uso fk
```sql
SELECT Studenti.Cognome, Studenti.Nome, Esami.Corso, Esami.Voto
FROM Studenti, Esami
WHERE Esami.Studente = Studenti.Matricola;
```

### Esempio controllo data
``` sql
-- Tutti gli studenti nati nel 2001
SELECT *
FROM segreteria.studenti
WHERE data_nascita BETWEEN '2001-01-01' AND '2001-12-31';

SELECT *
FROM segreteria.studenti
WHERE data_nascita >= '2001-01-01' AND data_nascita <= '2001-12-31';
```