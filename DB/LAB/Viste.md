Una **vista** è una tabella **virtuale**: è definita da un'interrogazione su tabelle base (o altre viste) e non esiste necessariamente in forma fisica. La query **non** viene eseguita alla creazione ma **ogni volta che la vista viene usata**, quindi riflette sempre i dati aggiornati.

### 1. Sintassi

``` sql
CREATE VIEW NomeVista [(ListaAttributi)] AS SELECT ...;

CREATE OR REPLACE VIEW NomeVista ... AS SELECT ...;   -- ridefinisce una vista esistente (PostgreSQL)
DROP VIEW NomeVista [RESTRICT | CASCADE];
```

``` sql
-- Studenti del primo e secondo anno
CREATE VIEW studenti_biennio AS
    SELECT * FROM studenti WHERE anno <= 2;

-- Rinominando le colonne
CREATE VIEW impiegati_poveri (matricola, cognome, nome, stipendio) AS
    SELECT matricola, cognome, nome, stipendio
    FROM impiegati
    WHERE stipendio < 1000;

-- Vista con join e colonne calcolate
CREATE VIEW esami_dettaglio (matricola, cognome, corso, voto_centesimi) AS
    SELECT s.matricola, s.cognome, c.nome, e.voto * 100 / 30
    FROM studenti s
    JOIN esami e ON s.matricola = e.studente
    JOIN corsi c ON e.corso = c.codice;
```

Una vista si usa esattamente come una tabella:

``` sql
SELECT cognome FROM studenti_biennio WHERE citta_residenza = 'Cagliari';
```

**A cosa servono**: semplificare/modularizzare query complesse, nascondere modifiche allo schema alle applicazioni, controllare gli accessi (mostrare solo alcune colonne/righe).

### 2. Lista degli attributi

- **Obbligatoria** quando la `SELECT` contiene espressioni o aggregati senza alias, oppure colonne con lo stesso nome (es. `s.nome` e `c.nome`).
- Più nomi che colonne → **errore** *"CREATE VIEW specifica più nomi di colonne che colonne"*.
- Meno nomi che colonne → accettato in PostgreSQL: le colonne restanti mantengono il nome originale.
- In alternativa alla lista si possono usare gli alias: `SELECT dipart AS dip, SUM(stipendio) AS tot ...`.

### 3. Uso tipico: comporre aggregati

#### a. Massimo di una somma

*Dipartimento che spende di più in stipendi.*

``` sql
CREATE VIEW budget_stipendi (dip, tot_stipendi) AS
    SELECT dipart, SUM(stipendio)
    FROM impiegati
    GROUP BY dipart;

SELECT dip
FROM budget_stipendi
WHERE tot_stipendi = (SELECT MAX(tot_stipendi) FROM budget_stipendi);
```

Equivalente senza vista: `GROUP BY dipart HAVING SUM(stipendio) >= ALL (SELECT SUM(stipendio) FROM impiegati GROUP BY dipart)`.

#### b. Media di un conteggio

*Numero medio di uffici per dipartimento.* `AVG(COUNT(...))` **non è ammesso** (aggregati non componibili):

``` sql
CREATE VIEW dipart_uffici (dip, nro_uffici) AS
    SELECT dipart, COUNT(DISTINCT ufficio)
    FROM impiegati
    GROUP BY dipart;

SELECT AVG(nro_uffici) FROM dipart_uffici;
```

#### c. Altri esempi tipici

``` sql
-- Media voti per studente, poi studenti sopra la media delle medie
CREATE VIEW medie_studenti (studente, media) AS
    SELECT studente, AVG(voto) FROM esami GROUP BY studente;

SELECT studente, media
FROM medie_studenti
WHERE media > (SELECT AVG(media) FROM medie_studenti);

-- Corso con più iscritti all'esame (a pari merito tutti)
CREATE VIEW esami_per_corso (corso, n) AS
    SELECT corso, COUNT(*) FROM esami GROUP BY corso;

SELECT corso FROM esami_per_corso
WHERE n = (SELECT MAX(n) FROM esami_per_corso);

-- Viste costruite su viste
CREATE VIEW studenti_bravi AS
    SELECT studente FROM medie_studenti WHERE media >= 27;
```

> Alternativa senza vista (stesso effetto, utile se la traccia non chiede di creare viste): sub-query nel `FROM` con alias, es. `SELECT AVG(n) FROM (SELECT ... GROUP BY dipart) AS t`.

### 4. Viste aggiornabili (cenni)

`INSERT`/`UPDATE`/`DELETE` su una vista sono tradotti in operazioni sulle tabelle base, ma **solo** se a ogni riga della vista corrisponde **una sola riga** di una tabella base.

**Non** aggiornabili (in PostgreSQL) le viste con: `GROUP BY`, `HAVING`, aggregati, `DISTINCT`, `UNION`/`INTERSECT`/`EXCEPT`, più tabelle nel `FROM` (join), colonne calcolate (quelle colonne non sono modificabili).

``` sql
CREATE VIEW num_imp (sede, n_imp) AS
    SELECT sede, COUNT(*) FROM imp GROUP BY sede;

UPDATE num_imp SET n_imp = n_imp + 1 WHERE sede = 'S03';
-- ERRORE: non si può modificare un valore aggregato (quale impiegato andrebbe aggiunto?)
```

#### Casi limite delle viste aggiornabili

Con `impiegati_poveri` (filtro `stipendio < 1000`):

``` sql
INSERT INTO impiegati_poveri VALUES (6, 'Neri', 'Ugo', 5000);
```

- L'inserimento **riesce**, ma la riga **non è visibile nella vista** (non soddisfa `stipendio < 1000`).
- Le colonne della tabella base non presenti nella vista ricevono il loro **default** (di solito `NULL`) → l'inserimento fallisce se una di esse è `NOT NULL` senza default.
- Allo stesso modo un `UPDATE` può "far uscire" una riga dalla vista (es. `SET stipendio = 2000`).

### 5. Altri casi limite

- **`SELECT *` viene "congelato"**: le colonne sono fissate al momento della `CREATE VIEW`. Una colonna aggiunta dopo alla tabella **non** compare nella vista.
- **Dipendenze**: non si può eliminare una tabella (o una sua colonna) usata da una vista senza `CASCADE`, che elimina anche la vista. Vedi [[Eliminazione di oggetti (DROP)]].
- **`CREATE OR REPLACE VIEW`** può solo **aggiungere colonne in fondo**: non può eliminarle, rinominarle o cambiarne il tipo (*"non è possibile eliminare colonne da una vista"*). In quel caso `DROP VIEW` e poi `CREATE VIEW`.
- **Schema**: come le tabelle, una vista senza prefisso finisce nello schema del `search_path` (`public`). Scrivere `CREATE VIEW segreteria.nome_vista ...`.
- **`ORDER BY` nella vista**: è inutile ordinare nella definizione, l'ordine va messo nella query che usa la vista.
