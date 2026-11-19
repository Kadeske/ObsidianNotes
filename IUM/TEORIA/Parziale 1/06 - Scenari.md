---
corso: Interazione Uomo Macchina
parziale: 1
argomento: Scenari
tags: [IUM, teoria, parziale1, scenari, UCD]
---

# 06 - Scenari

## Cos'è uno scenario

- Una **descrizione informale e narrativa** (una storia).
- Descrive attività che permettono di esplorare **contesto**, **necessità** e **requisiti**.
- **Non deve per forza descrivere il ruolo del software** (ma spesso lo fa).
- La forma di storia facilita la **comunicazione tra le parti** (utenti, designer, sviluppatori, committenti).
- Il **livello di dettaglio varia** a seconda della fase e delle iterazioni di sviluppo.
- Si possono usare anche per descrivere **interazioni future**, ad esempio con supporti previsti ma non ancora tecnicamente realizzabili.

## Anatomia di uno scenario

Uno scenario descrive **un solo compito** ed è composto da:
- un **attore** (di solito una persona, vedi [[05 - Requisiti e Personas]]);
- un **elemento motivatore**;
- un **obiettivo** o intenzione;
- un'**azione**;
- una **soluzione** (*resolution*).

### Esempio e breakdown (NN/g)

> *Detailed Debbie is going on a business trip. She needs to book a hotel room that's affordable and has good reviews. Debbie browses the site to find a hotel for her upcoming trip. She looks closely at the various hotels to find one that meets her needs. She considers price and user ratings heavily as she shops. Debbie selects a hotel and books a room.*

| Elemento | Testo |
| --- | --- |
| **Actor** | Detailed Debbie |
| **Motivator** | is going on a business trip. |
| **Intention** | She needs to book a hotel room that's affordable and has good reviews. |
| **Action** | Debbie browses the site to find a hotel for her upcoming trip. She looks closely at the various hotels to find one that meets her needs. |
| **Resolution** | Debbie selects a hotel and books a room. |

## Linee guida

- Come le personas, gli scenari **rendono concreto qualcosa di astratto**.
- Lo scenario è **descrittivo… ma non troppo**. Evitare troppi dettagli:
  - mantenere lo scenario **ad alto livello**;
  - **evitare informazioni specifiche sul design** (bottoni, menu, tab…);
  - includere solo **riferimenti generali** alla persona e ai suoi bisogni.

### Esempio negativo 1 – troppi dettagli di design

> *Debbie goes to the **Book a Hotel tab** and uses the **search bar** to look for a hotel. She uses the **left-side filter menu** to filter the results. She considers price and user ratings heavily as she shops and adds those she likes best to her **Favorites list**. She revisits her Favorites list in her **user profile**.*

❌ Lo scenario sta già decidendo l'interfaccia (tab, barra di ricerca, menu a sinistra): blocca l'esplorazione di soluzioni alternative.

### Esempio negativo 2 – troppi dettagli irrilevanti

> *Detailed Debbie is going on a business trip to **Honolulu Hawaii**, and she is traveling with her **small dog, Penny**. She needs to book a hotel room that's affordable, has good reviews, and **a minibar**. It must also be **on the 2nd floor near the elevator**. […] spending **45 minutes** looking at hotel photos trying to determine if the bed will be comfortable and if there is enough closet room for her clothes. As she shops, she considers price, user ratings, **WIFI speed**, and the availability of **snacks** […] books a room using her **American Express card**. She hopes that **taxi cabs** are easy to reserve […]*

❌ Dettagli che non aiutano a capire il compito principale e confondono le priorità.

## Esempio: organizzazione di viaggi (Preece, Rogers, Sharp – cap. 10)

> Alla famiglia Thomson piace svolgere attività all'aperto e quest'anno ha deciso di provare la vela. La famiglia è composta da quattro persone: Marco (10 anni), Paola (15), Chiara (35) e Gianni (40).
>
> Una sera, dopo cena, si riuniscono per esplorare le diverse opzioni. Si raccolgono attorno all'applicazione per organizzare i viaggi, inserendo l'insieme iniziale di opzioni: un viaggio in barca a vela per quattro principianti. L'applicazione consente a **tutti i membri della famiglia** di visualizzare e/o selezionare le diverse opzioni.
>
> Il primo suggerimento è di partecipare a una **flottiglia**, dove si viaggia in piccoli gruppi con diversi livelli di esperienza su barche separate. Marco e Paola non sono felici di andare in vacanza con altre persone ma, dopo aver letto i **commenti di altri ragazzi** che hanno fatto quell'esperienza, sono d'accordo.
>
> Gianni conferma la scelta e chiede maggiori informazioni al sistema. Visto che si fa tardi, decide di continuare domani: **salva la scelta e stampa** la lista di informazioni.

Da questo scenario emergono requisiti: uso **collaborativo** (più persone davanti allo stesso dispositivo), recensioni di **utenti simili** (ragazzi), possibilità di **salvare** e riprendere, **stampare**.

## Esercizi

> [!example] Esercizio 1
> Descrivere uno scenario per la persona definita per l'app degli orari universitari, applicando le linee guida per valutare quali informazioni siano necessarie e quali no.
>
> **Possibile soluzione** (persona: *Giulia, la pendolare organizzata*, vedi [[05 - Requisiti e Personas]])
> - *Attore*: Giulia
> - *Motivatore*: domani mattina ha lezione di Basi di Dati alle 9 e il treno parte alle 7.
> - *Obiettivo*: vuole essere sicura che la lezione si tenga e sapere in che aula, per non fare il viaggio a vuoto.
> - *Azione*: la sera prima consulta l'app degli orari, controlla le lezioni del giorno dopo e verifica eventuali avvisi del docente.
> - *Soluzione*: scopre che la lezione è stata spostata in un'altra aula, lo annota e la mattina dopo va direttamente lì.
>
> Notare che lo scenario **non** dice come è fatta l'interfaccia (niente "tocca il tab Avvisi").

> [!example] Esercizio 2 – *With a little help from my (AI) friends*
> Ripetere lo stesso esercizio con ChatGPT e confrontare lo stile con il proprio. *(Gli LLM tendono a produrre scenari troppo lunghi e pieni di dettagli di design o irrilevanti, cioè gli "esempi negativi" visti sopra.)*

## Riferimenti
- NN/g – *Scenario mapping & personas*: https://www.nngroup.com/articles/scenario-mapping-personas/
- Preece, Rogers, Sharp – *Interaction Design*, cap. 10
