# 05 - Scenari e modelli dei task

> Fonte: [[IUM/TEORIA/PDF/05-scenari.pdf]]

---

## 1. Scenari

Uno **scenario** è una **descrizione informale e narrativa** di un'attività, che permette di esplorare **contesto**, **necessità** e **requisiti**.

- Non deve per forza descrivere il ruolo del software (ma spesso lo fa).
- La forma di **storia** facilita la **comunicazione** tra le parti coinvolte.
- Il livello di dettaglio varia con la fase e le iterazioni di sviluppo.
- Può descrivere anche interazioni **future**, con tecnologie previste ma non ancora realizzabili.

### Anatomia di uno scenario

Uno scenario descrive **un solo compito** e contiene:

| Elemento | Ruolo |
|---|---|
| **Attore** | Chi agisce (di solito una **persona**, vedi [[04 - Requisiti e Personas]]) |
| **Motivatore** | Cosa fa scattare l'attività |
| **Intenzione / obiettivo** | Cosa vuole ottenere |
| **Azione** | Cosa fa |
| **Risoluzione** | Come si conclude |

**Esempio** (Nielsen Norman Group): *Detailed Debbie is going on a business trip. She needs to book a hotel room that's affordable and has good reviews. Debbie browses the site to find a hotel for her upcoming trip. She looks closely at the various hotels to find one that meets her needs. She considers price and user ratings heavily as she shops. Debbie selects a hotel and books a room.*

| Elemento | Testo |
|---|---|
| Attore | Detailed Debbie |
| Motivatore | is going on a business trip. |
| Intenzione | She needs to book a hotel room that's affordable and has good reviews. |
| Azione | Debbie browses the site to find a hotel… She looks closely at the various hotels to find one that meets her needs. |
| Risoluzione | Debbie selects a hotel and books a room. |

### Linee guida
Come le personas, gli scenari rendono **concreto** qualcosa di astratto. Lo scenario è descrittivo… **ma non troppo**:
1. mantenerlo ad **alto livello**;
2. evitare informazioni specifiche sul **design** dell'interfaccia;
3. includere solo **riferimenti generali** alla persona e ai suoi bisogni.

> [!failure] Esempio negativo 1 — troppi dettagli di design
> *Debbie goes to the **Book a Hotel tab** and uses the **search bar** to look for a hotel. She uses the **left-side filter menu** to filter the results. She considers price and user ratings heavily as she shops and adds those she likes best to her **Favorites list**. She revisits her Favorites list in her user profile.*
>
> Descrive già l'interfaccia (tab, barra di ricerca, menu a sinistra): lo scenario deve restare indipendente dal design.

> [!failure] Esempio negativo 2 — troppi dettagli sulla persona
> *…going on a business trip to **Honolulu Hawaii**, traveling with her small **dog, Penny**… a **minibar**… on the **2nd floor near the elevator**… spending **45 minutes** looking at hotel photos… **WIFI speed**… free **snacks**… using her **American Express card**… hopes that **taxi cabs** are easy to reserve…*
>
> I dettagli irrilevanti nascondono il compito e i bisogni essenziali.

> [!example] Esempio positivo — organizzazione di un viaggio (Preece, Rogers, Sharp, cap. 10)
> Alla famiglia Thomson piacciono le attività all'aria aperta e quest'anno vuole provare la vela. La famiglia è composta da Marco (10 anni), Paola (15), Chiara (35) e Gianni (40). Una sera, dopo cena, si riuniscono attorno all'applicazione per organizzare i viaggi e inseriscono le opzioni iniziali: un viaggio in barca a vela per quattro principianti. L'applicazione permette a tutti di vedere e selezionare le opzioni.
> Il primo suggerimento è una **flottiglia** (piccoli gruppi con diversi livelli di esperienza, su barche separate). Marco e Paola non vogliono andare in vacanza con altre persone ma, dopo aver letto i commenti di altri ragazzi che l'hanno provata, accettano. Gianni conferma la scelta e chiede più informazioni. Visto che è tardi, decide di continuare il giorno dopo: salva la scelta e stampa le informazioni.

> [!question] Esercizi
> 1. Descrivere uno scenario per la persona definita per l'app degli orari universitari, applicando le linee guida per decidere quali informazioni servono e quali no.
> 2. Ripetere l'esercizio con ChatGPT e confrontare lo stile con il proprio.

## 2. Analisi e modellazione dei task

- **Task (compito)**: attività da eseguire per raggiungere un obiettivo.
- **Obiettivo**: una **modifica desiderata dello stato** del sistema, oppure il tentativo di **ottenere informazioni** sullo stato.
- Ogni task è associato a un obiettivo; ogni obiettivo a **uno o più** task.
- I task si descrivono a **più livelli di astrazione**, fino ai task di base.
- Si distinguono **task analysis** (analisi dell'esistente) e **task model** (modello delle attività).

### Cosa si stabilisce con l'analisi
- Le **attività** da supportare.
- Gli **oggetti** necessari per eseguirle.
- Le **conoscenze** richieste.
- L'**allocazione dei compiti** tra sistema e utente.

### A cosa servono i modelli di task
- Comprendere un dominio applicativo.
- Memorizzare il risultato di una discussione interdisciplinare.
- Progettare un'applicazione coerente con il **modello concettuale dell'utente**.
- Analizzare e valutare l'usabilità di un'applicazione esistente.
- Assistere l'utente durante l'uso; documentare un'applicazione interattiva.

| **Scenario** | **Modello di task** |
|---|---|
| Descrizione **informale ma dettagliata** di **uno specifico uso** in uno specifico contesto | Descrive **tutte le possibili attività** e le loro **relazioni** |

Gli scenari possono aiutare a costruire modelli di task e possono essere **derivati** da essi.

## 3. Hierarchical Task Analysis (HTA)

Il task viene scomposto **gerarchicamente** in sottotask numerati; un **piano** (*plan*) indica in che ordine eseguirli.

![[IUM05-hta.png|550]]

Nell'esempio "Buy DVD", il *plan 0* dice: se l'utente è abituale esegue 1-2-5 (cerca, aggiungi al carrello, conferma), se è nuovo esegue 1-2-3-4-5 (inserendo anche pagamento e indirizzo).

## 4. ConcurTaskTrees (CTT)

Notazione grafica per modelli di task che specifica **struttura gerarchica**, **relazioni temporali** tra i task e **allocazione** dei task (chi li esegue).

![[IUM05-ctt.jpg|600]]

### Categorie di task

![[IUM05-categorie-task.png|220]]

| Categoria | Chi esegue il task |
|---|---|
| **Interazione** | L'utente interagisce con il sistema |
| **Applicazione** | Il sistema, da solo |
| **Utente** | L'utente, da solo (attività cognitiva, es. decidere) |
| **Astratta** | Task composto da sottotask di categorie diverse |

### Gerarchia
- Indica la **scomposizione** di un task padre in task figli, collegati tra loro da **operatori temporali**.
- Se tutti i figli sono della **stessa categoria**, anche il padre lo è.
- Se i figli sono di **categorie diverse**, il padre è **astratto**.

![[IUM05-gerarchia.png|500]]

A sinistra il caso corretto: i figli sono di categorie diverse (applicazione, interazione, applicazione), quindi `Task_0` è **astratto**. A destra il caso **sbagliato** (barrato nelle slide): `Task_1` ha figli di categorie diverse ma è marcato come task di applicazione.

### Operatori temporali

| Operatore | Simbolo | Arietà | Significato |
|---|---|---|---|
| **Iterazione** | `T*` | 1 | Il task si ripete |
| **Sequenza** (*enabling*) | `T1 >> T2` | 2 (n) | T2 può iniziare solo dopo la fine di T1 |
| **Parallelo** (*concurrency*) | `T1 \|\|\| T2` | 2 (n) | I task si eseguono in qualunque intreccio |
| **Scelta** (*choice*) | `T1 [] T2` | 2 (n) | Se ne esegue solo uno |
| **Disabilita** (*disabling*) | `T1 [> T2` | 2 (n) | L'inizio di T2 interrompe definitivamente T1 |
| **Qualsiasi ordine** (*order independence*) | `T1 \|=\| T2` | n | Tutti vanno eseguiti, in qualunque ordine ma uno alla volta |

(I significati sono quelli standard della notazione CTT; le slide riportano solo nome, simbolo e arietà.)

---

## Termini e concetti da ricordare

- **Scenario**: narrazione informale di **un solo compito**; esplora contesto, necessità, requisiti.
- **Anatomia**: attore · motivatore · intenzione · azione · risoluzione.
- **Linee guida**: alto livello, niente dettagli di design, solo riferimenti generali alla persona.
- **Task**: attività per raggiungere un **obiettivo** (cambiare lo stato o ottenere informazioni).
- **Task analysis vs task model**; **scenario** (un uso specifico) vs **modello di task** (tutte le attività e le relazioni).
- **HTA**: scomposizione gerarchica numerata + **piani** che fissano l'ordine.
- **CTT (ConcurTaskTrees)**: gerarchia + relazioni temporali + allocazione.
- **Categorie CTT**: interazione, applicazione, utente, astratta (figli di categorie diverse).
- **Operatori temporali**: iterazione `*`, sequenza `>>`, parallelo `|||`, scelta `[]`, disabilitazione `[>`, qualsiasi ordine `|=|`.
