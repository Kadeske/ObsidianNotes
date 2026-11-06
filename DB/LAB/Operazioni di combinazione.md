Ecco il riassunto schematico delle Lezioni 4-5 relative alle operazioni di combinazione di tabelle e ai vari tipi di Join in SQL.

### 1. Prodotto Cartesiano

Combina in tutti i modi possibili ogni tupla della prima tabella con ogni tupla della seconda tabella ($T_1 \times T_2$).

- **Sintassi implicita**: elencare le tabelle separate da virgola nella clausola `FROM`:

``` sql
SELECT *
FROM T1, T2;
```

- **Sintassi esplicita**: utilizzo della clausola `CROSS JOIN`:


``` sql
SELECT *
FROM T1 CROSS JOIN T2;
```

- Ha senso pratico solo se seguito da condizioni di selezione per correlare i record (trasformandosi in un _theta-join_).
### 2. Operazioni di Join (Theta-Join ed Equi-Join)

Permettono di correlare i dati di due o più tabelle in base a una condizione logica (generalmente l'uguaglianza tra chiave primaria e foreign key).

#### Sintassi Implicita (nel `WHERE`)

Le tabelle sono separate da virgole nel `FROM` e la condizione di correlazione è specificata nel `WHERE`:

``` sql
SELECT Studenti.Cognome, Studenti.Nome
FROM Studenti, Esami
WHERE Studenti.Matricola = Esami.Studente
  AND Esami.Corso = 'C3';
```

  
#### Sintassi Esplicita (nel `FROM` con clausola `ON`)

Consente di separare chiaramente la condizione di accoppiamento delle tabelle (`ON`) dalle condizioni di filtro sulle singole righe (`WHERE`):

``` sql
SELECT Studenti.Cognome, Studenti.Nome
FROM Studenti 
JOIN Esami ON Studenti.Matricola = Esami.Studente
WHERE Esami.Corso = 'C3';
```

#### Self Join (Join di una tabella con se stessa)

Richiede obbligatoriamente l'uso di **alias** distinti per fare riferimento alla stessa relazione:

``` sql
SELECT S1.Matricola, S2.Matricola
FROM Studenti S1 
JOIN Studenti S2 ON S1.CittàResid = S2.CittàResid 
                AND S1.Matricola > S2.Matricola;
```

### 3. Join Interni ed Esterni (`INNER` vs `OUTER`)

La sintassi generale per distinguere le modalità è:

``` sql
Tabella1 [INNER | LEFT | RIGHT | FULL] JOIN Tabella2 ON Condizione
```

- **`INNER JOIN` (Default)**:    
    - Restituisce solo le tuple che trovano corrispondenza in entrambe le tabelle secondo la condizione specificata nel predicato `ON`. Le righe senza corrispondenza vengono scartate.
    - La parola chiave `INNER` è opzionale (scrivere `JOIN` equivale a scrivere `INNER JOIN`).
- **`LEFT [OUTER] JOIN`**:
    - Mantiene **tutte** le tuple della tabella di sinistra (`Tabella1`).
    - Se una tupla non trova riscontro nella tabella di destra, gli attributi di quest'ultima vengono estesi con valori `NULL`.    
- **`RIGHT [OUTER] JOIN`**:
    - Mantiene **tutte** le tuple della tabella di destra (`Tabella2`), estendendo con `NULL` le colonne della tabella di sinistra qualora manchi una corrispondenza.
- **`FULL [OUTER] JOIN`**:
    - Mantiene **tutte** le tuple di entrambe le tabelle, estendendo con `NULL` i valori mancanti da ambo i lati.

### 4. Join Multipli (Più di due tabelle)

È possibile concatenare più operazioni di join nella stessa query, collegando entità collegate a cascata.

- **Sintassi implicita**:

``` sql
SELECT Studenti.Matricola, Studenti.Cognome
FROM Studenti, Esami, Corsi
WHERE Studenti.Matricola = Esami.Studente 
  AND Esami.Corso = Corsi.Codice
  AND Corsi.Nome = 'Analisi';
```

- **Sintassi esplicita**:

``` sql
SELECT Studenti.Matricola, Studenti.Cognome
FROM Studenti 
JOIN Esami ON Studenti.Matricola = Esami.Studente
JOIN Corsi ON Esami.Corso = Corsi.Codice
WHERE Corsi.Nome = 'Analisi';
```

### 5. Forme Alternative di Join (`NATURAL JOIN` e `USING`)

Sintassi compatte che semplificano la condizione di join quando le tabelle condividono attributi con lo stesso nome.

- **`NATURAL JOIN`**:
    - Esegue automaticamente l'equi-join confrontando tutti gli attributi che hanno lo **stesso identico nome** nelle due tabelle.
    - Nel risultato le colonne duplicate compaiono una sola volta.
    ``` sql
    SELECT NomeDip 
    FROM Impiegati NATURAL JOIN Dipartimenti
    WHERE Nome = 'Mario' AND Cognome = 'Rossi';
    ```
 
- **Clausola `USING (...)`**:
    
      
    
    - Si specifica esplicitamente su quale insieme di attributi omonimi effettuare il confronto di uguaglianza.
 
	``` sql
    SELECT NomeDip 
    FROM Impiegati JOIN Dipartimenti USING (CodiceDip)
    WHERE Nome = 'Mario' AND Cognome = 'Rossi';
    ```
- **Varianti con `OUTER`**: 
    - Sia il `NATURAL JOIN` sia la clausola `USING` ammettono le varianti esterne (es. `NATURAL LEFT JOIN`, `NATURAL RIGHT JOIN`, `NATURAL FULL JOIN`).

## Esempi


- `Studenti(matricola, cognome, nome, citta_residenza, anno)`
- `Corsi(codice, nome, docente)`
- `Esami(studente, corso, data, voto)`

### 1. Esempio di Join Multipli (3 tabelle)

Un **join multiplo** collega tra loro più di due tabelle collegando le chiavi corrispondenti in catena.

**Obiettivo:** Trovare il cognome dello studente, il nome del corso e il voto per tutti gli esami superati con voto almeno pari a 28.

``` sql
SELECT s.cognome, c.nome AS nome_corso, e.voto
FROM segreteria.studenti AS s
JOIN segreteria.esami AS e 
    ON s.matricola = e.studente
JOIN segreteria.corsi AS c 
    ON e.corso = c.codice
WHERE e.voto >= 28;
```

- **Primo join (`studenti` con `esami`):** collega lo studente al suo esame tramite `s.matricola = e.studente`. 
- **Secondo join (con `corsi`):** risale al nome del corso tramite `e.corso = c.codice`. 
- **`WHERE`:** filtra solo le righe che soddisfano la condizione sul voto. 

### 2. Esempio di Query Annidata (Subquery)

Una **query annidata** (o sotto-interrogazione) è un'istruzione `SELECT` posta all'interno di un'altra query principale (spesso nella clausola `WHERE` con operatori come `IN`, `NOT IN`, `EXISTS`, ecc.).

**Obiettivo:** Trovare la matricola, il cognome e il nome degli studenti che **non** hanno sostenuto alcun esame.

``` sql
SELECT matricola, cognome, nome
FROM segreteria.studenti
WHERE matricola NOT IN (
    SELECT studente
    FROM segreteria.esami
);
```

- **Subquery interna:** restituisce l'elenco di tutte le matricole presenti nella tabella `esami` (cioè chi ha fatto almeno un esame).
- **Query esterna:** seleziona gli studenti la cui matricola **non compare** in quell'elenco (`NOT IN`).