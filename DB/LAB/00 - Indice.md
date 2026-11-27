---
corso: Basi di Dati 1
parte: Laboratorio
tags: [DB, laboratorio, SQL, PostgreSQL, indice]
---

# Basi di Dati 1 – Laboratorio SQL (PostgreSQL)

Appunti ricavati dalle slide del laboratorio (cartella `pdf/`), organizzati **per argomento** e non per lezione. Gli esempi e i casi limite sono stati verificati su PostgreSQL.

> [!tip] Come usarle per gli esercizi
> - Parti da [[Pattern di query]]: la tabella "dalla traccia alla soluzione" ti porta alla tecnica giusta.
> - Quando un risultato non torna, controlla [[NULL e logica a tre valori]]: la maggior parte degli errori nasce lì.
> - Le differenze tra slide e comportamento reale di PostgreSQL sono segnalate con ⚠️.

## 1. Definizione dei dati (DDL)

| Nota | Argomenti principali | Slide |
| --- | --- | --- |
| [[Creazione tabelle]] | Tipi di dato, `CREATE TABLE`, `DEFAULT`, vincoli `NOT NULL`/`UNIQUE`/`PRIMARY KEY`/`CHECK`, `CONSTRAINT` | [[Introduzione_BDlab.pdf]], [[SQL-Lez1.pdf]] |
| [[Domini e schemi]] | `CREATE SCHEMA`, `search_path`, `CREATE DOMAIN`, casi limite dei tipi (`CHAR`, `NUMERIC`, divisione intera) | [[SQL-Lez1.pdf]] |
| [[Vincoli di integrità referenziale]] | `FOREIGN KEY`/`REFERENCES`, FK composte, politiche `CASCADE`/`SET NULL`/`SET DEFAULT`/`RESTRICT`/`NO ACTION` | [[SQL-Lez2.pdf]] |
| [[Alter tabelle]] | `ALTER TABLE`: aggiunta di colonne e vincoli, `SET/DROP DEFAULT`, `SET/DROP NOT NULL` | [[SQL-Lez2.pdf]] |
| [[Eliminazione di oggetti (DROP)]] | `DROP` di schemi/domini/tabelle/viste, `RESTRICT` vs `CASCADE`, nomi automatici dei vincoli | [[SQL-Lez2.pdf]] |

## 2. Interrogazioni (DML)

| Nota | Argomenti principali | Slide |
| --- | --- | --- |
| [[NULL e logica a tre valori]] | `UNKNOWN`, tabelle di verità, `IS NULL`, `NULL` in vincoli, `NOT IN`, ordinamento e gruppi | [[SQL-Lez1.pdf]] |
| [[Selezione]] | `SELECT`/`FROM`/`WHERE`, `BETWEEN`, `IN`, `ANY`/`ALL`, `LIKE`, `DISTINCT`, `ORDER BY`, alias | [[SQL-Lez3.pdf]] |
| [[Operazioni di combinazione]] | Prodotto cartesiano, join, self join, `INNER`/`LEFT`/`RIGHT`/`FULL`, join multipli, `NATURAL JOIN`, `USING` | [[SQL-Lez4-5.pdf]] |
| [[Query nidificate]] | `ANY`/`ALL`/`IN`/`EXISTS` con sub-query, costruttore di tupla, sub-query correlate, riducibilità a join | [[SQL-Lez6.pdf]] |
| [[Funzioni aggregate]] | `COUNT`/`SUM`/`AVG`/`MAX`/`MIN`, gestione dei `NULL`, insieme vuoto, aggregati non annidabili | [[SQL-Lez7-8.pdf]] |
| [[Raggruppamento (GROUP BY e HAVING)]] | `GROUP BY`, `HAVING`, `WHERE` vs `HAVING`, massimo tra gruppi, conteggi con zero | [[SQL-Lez7-8.pdf]] |
| [[Operatori insiemistici]] | `UNION`/`INTERSECT`/`EXCEPT`, `ALL`, compatibilità per posizione, equivalenze con sub-query | [[SQL-Lez9.pdf]] |
| [[Viste]] | `CREATE VIEW`, composizione di aggregati, viste aggiornabili | [[SQL-Lez10.pdf]] |

## 3. Preparazione agli esercizi

| Nota | Argomenti principali |
| --- | --- |
| [[Pattern di query]] | Dalla traccia alla soluzione: "almeno uno", "mai", "solo", "tutti", "almeno N", massimo, coppie, outer join, trappole frequenti, checklist finale |
