# 01 - Introduzione al corso

> **Basi di Dati** — Prof. Daniele Riboni (riboni@unica.it)
> Università degli Studi di Cagliari, Dipartimento di Matematica e Informatica
> Fonte: [[DB/TEORIA/PDF/01 - Introduzione al corso.pdf]]

---

## 1. Organizzazione delle lezioni

| | Teoria | Laboratorio |
|---|---|---|
| **Giorni** | Martedì e mercoledì | Martedì |
| **Orario** | 9:00 | 11:00 |
| **Aula** | Aula Magna di Matematica | Laboratorio T e Laboratorio Pes (ex lab M) |

- **Teoria**: lezioni teoriche seguite da esercitazioni.
- **Laboratorio**: linguaggio SQL ed esercizi al computer con il DBMS **PostgreSQL**.

---

## 2. Obiettivi del corso

Fondamenti, progettazione e utilizzo di **sistemi di gestione di basi di dati** (DBMS) basati sul **modello relazionale**.

### Perché le basi di dati contano
Ogni applicazione moderna si appoggia su un database: banche e pagamenti, sanità, social network e media, pubblica amministrazione, mappe e trasporti. Nessun sistema digitale funziona senza un modo solido per **organizzare, proteggere e interrogare** i propri dati.

### Programma

**Teoria — Fondamenti e progettazione**
- DBMS, modello relazionale, algebra relazionale, architettura software dei DBMS
- Caratteristiche avanzate, *cloud-based data management* e nuovi sistemi di gestione dei dati
- Modello ER, progettazione e normalizzazione

**Laboratorio — Utilizzo di un DBMS relazionale**
- Linguaggio SQL
- Creazione e popolamento di database
- Modifiche e interrogazioni sui dati

### Cosa si impara
1. **Competenze pratiche spendibili**: progettare, interrogare e gestire basi di dati reali (SQL, algebra relazionale).
2. **Un modo di pensare rigoroso**: scomporre un problema complesso in una struttura logica e formale.
3. **Le fondamenta per ciò che viene dopo**: data science, intelligenza artificiale e big data richiedono di sapere come i dati sono organizzati.

---

## 3. Materiale didattico

- **Moodle**: <https://elearning.unica.it/> → Facoltà di Scienze › L.T. Informatica › Basi di Dati 1
  - Chiave di iscrizione: `bd1moodle`
  - Slide, esercizi e soluzioni
  - Video lezioni A.A. 2021/22 (richiedono l'iscrizione al Team di BD)
- **Libro di testo (opzionale)**: Atzeni, Ceri, Fraternali, Paraboschi, Torlone — *Basi di dati*, 6ª ed., McGraw-Hill, 2023.
  - Versione inglese scaricabile gratuitamente su `dbbook.inf.uniroma3.it`.

---

## 4. Esame

L'esame è composto da **due parti**, sostenibili nello stesso appello o in appelli diversi:

| Parte | Contenuti | Modalità |
|---|---|---|
| **Teoria** | Fondamenti, algebra, progettazione, normalizzazione | Esercizi e quiz su tutto il programma |
| **Laboratorio** | SQL: creazione e interrogazione di una base di dati | PostgreSQL + PgAdmin sulle macchine del laboratorio |

### Prove intermedie di teoria
- **Prima prova**: martedì 17 novembre 2026
- **Seconda prova**: a fine corso

Sono **facoltative** e sostituiscono la parte di teoria. Possono partecipare anche studenti che hanno frequentato in anni passati.

> [!warning] Attenzione
> - Chi **non passa la prima** prova non può sostenere la seconda e **perde il bonus**.
> - Chi passa la prima ma **non la seconda** (insufficiente, ritirato o assente) deve rifare **tutta la teoria** in un appello normale e perde il bonus.

### Voto finale

$$\text{Voto} = \left(\text{Teoria} \cdot \tfrac{2}{3} + \text{Laboratorio} \cdot \tfrac{1}{3}\right) + \text{bonus}$$

- I voti sono espressi in **31esimi**.
- **Lode**: voto finale $\geq 30.5$ → 30 e lode.
- Non si può **rifiutare** il voto (né quello finale né quello delle singole prove).

### Bonus (+2 punti)
Si ottiene superando **entrambe le parti al primo tentativo entro febbraio** (cioè senza insufficienze né nell'A.A. corrente né in quelli precedenti).

Si perde il bonus se:
- ci si **ritira** da una prova;
- ci si **iscrive** a un appello e **non ci si presenta**.

### Iscrizione agli appelli
1. È obbligatorio iscriversi al relativo appello su **ESSE3**.
2. Nelle note specificare: `TEORIA`, `LAB` o `ENTRAMBI`.
3. Se si decide all'ultimo di non partecipare, segnalarlo al docente via email.

### Validità delle prove
Il voto delle singole prove resta valido per **12 mesi**; trascorso questo periodo la prova viene annullata e va ripetuta.

---

## 5. Ricevimento

- Su **appuntamento via email** (riboni@unica.it).
- Ufficio: primo piano, di fronte alle scale.
- Spiegazioni sul programma e soluzioni di esercizi **solo a ricevimento** (in presenza o su Teams), non via email.

---

## Tabella riassuntiva per gli esercizi

| Voce | Regola |
|---|---|
| **Voto finale** | $\text{Teoria} \cdot \tfrac{2}{3} + \text{Laboratorio} \cdot \tfrac{1}{3} + \text{bonus}$ (voti in 31esimi) |
| **Lode** | Voto finale $\geq 30.5$ |
| **Bonus +2** | Entrambe le parti superate al primo tentativo entro febbraio, senza insufficienze pregresse |
| **Perdita del bonus** | Ritiro da una prova · iscrizione a un appello senza presentarsi · prima prova intermedia non superata · seconda prova non superata |
| **Prove intermedie** | 1ª: 17/11/2026 · 2ª: fine corso · facoltative, sostituiscono la teoria · senza la 1ª non si accede alla 2ª |
| **Validità dei voti** | 12 mesi per singola prova |
| **Iscrizione** | ESSE3, specificando nelle note `TEORIA`, `LAB` o `ENTRAMBI` |
| **Rifiuto del voto** | Non ammesso |

**Esempio di calcolo** — Teoria 28, Laboratorio 30, con bonus:
$$28 \cdot \tfrac{2}{3} + 30 \cdot \tfrac{1}{3} + 2 = 18.\overline{6} + 10 + 2 = 30.\overline{6} \;\geq\; 30.5 \;\Rightarrow\; \textbf{30 e lode}$$

## Esercizi della lezione

Questa lezione introduttiva non contiene esercizi.
