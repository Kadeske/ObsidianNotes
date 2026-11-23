---
corso: Interazione Uomo Macchina
parziale: 1
argomento: Requisiti e Personas
tags: [IUM, teoria, parziale1, requisiti, personas, UCD]
---

# 05 - Requisiti e Personas

## Cosa sono i requisiti

Nello **user-centered design (UCD)** i requisiti rappresentano le **aspettative, necessità e vincoli degli utenti finali** durante lo sviluppo di un prodotto o servizio.

**Perché sono importanti**
- **Orientamento all'utente**: assicurano che il design risponda davvero ai bisogni degli utenti.
- **Riduzione dei rischi**: evitano malintesi e sviluppi errati, migliorando la soddisfazione.
- **Base per la progettazione**: guidano le decisioni di design e sviluppo.

## Requisiti tradizionali vs User-Centered Design

|  | Requisiti tradizionali | Requisiti UCD |
| --- | --- | --- |
| **Definizione** | Specifiche tecniche e funzionali definite in fase di progettazione | Aspettative e bisogni degli utenti finali |
| **Focus** | Caratteristiche del sistema, performance, architettura | Esperienza utente, usabilità, soddisfazione |
| **Esempi** | Capacità di carico, tempi di risposta, compatibilità con altri sistemi | Facilità d'uso, accessibilità, necessità di interazione |

Come si relazionano:
- **Integrazione**: i requisiti UCD **arricchiscono** quelli tradizionali, assicurando che le funzionalità soddisfino necessità reali.
- **Priorità**: i tradizionali sono più orientati alla tecnologia, quelli UCD mettono **l'utente al centro**.
- **Iterazione**: i requisiti UCD possono **evolvere** durante lo sviluppo → serve flessibilità.
- **Obiettivo**: un prodotto che non solo funzioni bene, ma che sia anche **facile e piacevole** da usare.

## Importanza della definizione dei requisiti

È la fase in cui sono **più comuni i fallimenti** dei progetti.

![[ium_p05_s05.jpg]]

> [!info] La vignetta dell'altalena (*Tree Swing*)
> Celebre vignetta sui progetti software: come il cliente ha spiegato l'altalena, come l'ha capita il project leader, come l'ha progettata l'analista, come l'ha scritta il programmatore, come l'ha descritta il consulente commerciale, come è stato documentato il progetto, quali componenti sono stati installati, quanto è stato fatturato al cliente, come è stato supportato… e **cosa serviva davvero al cliente** (un semplice copertone appeso a un ramo). Ogni passaggio di comunicazione distorce i requisiti.

## I diversi tipi di requisiti

- **Funzionali** – *cosa* il sistema deve fare. Storicamente sono quelli su cui ci si concentra di più.
- **Non funzionali** – vincoli sul *come*: dimensioni della memoria, tempi di risposta…
- **Dati** – che tipi di dati vanno salvati? Come (es. database)?
- **Ambiente / contesto d'uso**
  - **Fisico**: polvere, rumore, vibrazioni, luce, caldo, umidità…
  - **Sociale**: condivisione di file, display, carta; lavoro a distanza; lavoro individuale; privacy dei clienti
  - **Organizzativo**: gerarchia, supporto agli utenti, struttura e infrastruttura dell'organizzazione, disponibilità alla formazione

> [!tip] Approfondimento – Altri tipi
> Preece et al. aggiungono anche i requisiti **utente** (caratteristiche del gruppo target: abilità, esperienza, nazionalità…) e di **usabilità** (obiettivi di usabilità misurabili, es. "un nuovo utente completa l'acquisto in < 3 minuti").

## Come si scrive un requisito

Un requisito deve essere **chiaro**, **non ambiguo** e **specifico** (quindi verificabile).
- ✅ **Buono**: «Il download di qualsiasi pagina del sito deve impiegare meno di 5 secondi»
- ❌ **Cattivo**: «Le bambine di età inferiore ai 10 anni devono ritenere che il sito sia attraente» (come lo misuro? cosa vuol dire "attraente"?)

Di solito si aggiungono altri campi descrittivi: **identificatore**, **tipo**, **chi lo ha generato**, **motivazione** per averlo incluso.

### Esempi per categoria

1. **Funzionali**
   - *Ricerca*: l'utente deve poter cercare prodotti con filtri per categoria, prezzo e valutazione.
   - *Registrazione*: gli utenti devono registrarsi via email o social, con un processo che richieda **meno di 2 minuti**.
2. **Usabilità**
   - *Navigazione*: massimo **3 clic** per raggiungere qualsiasi pagina.
   - *Feedback*: dopo ogni azione significativa (es. invio di un modulo) deve essere fornito un feedback immediato.
3. **Accessibilità**
   - *Screen reader*: il sito deve essere completamente navigabile con tecnologie assistive per utenti non vedenti.
   - *Contrasto visivo*: il testo deve avere contrasto sufficiente con lo sfondo per utenti con disabilità visive.
4. *(nelle slide la numerazione salta il 4)*
5. **Performance**
   - *Tempo di caricamento*: pagine in meno di **3 secondi** su una connessione media.
   - *Reattività*: la UI deve rispondere all'input in meno di **200 ms**.
6. **Contesto**
   - *Uso mobile*: layout responsive che si adatta a schermi di dimensioni diverse.
   - *Modalità offline*: accesso ai contenuti salvati anche senza connessione.

## User Centered Design → conoscere gli utenti

- È difficile prendere decisioni basandosi su un **vago concetto di "utente"**.
- Per questo dobbiamo renderlo **concreto**.

### Le caratteristiche dell'utente
- **Chi sono?**
- Caratteristiche: abilità, conoscenze, dimestichezza con il computer.
- Uso del sistema: **inesperto** o **esperto**, **casuale** o **frequente**.

| Tipo di utente | Di cosa ha bisogno |
| --- | --- |
| Inesperto | Istruzioni passo-passo, interazione vincolata, informazioni chiare |
| Esperto | Flessibilità, accesso diretto alle funzioni, "potere" |
| Frequente | Scorciatoie (*short-cuts*) |
| Casuale | Istruzioni chiare (es. menu) |

### Le persone sono varie sotto molti aspetti
- La **dimensione delle mani** può influire su posizione e dimensione dei bottoni.
- Le **abilità motorie** influiscono sulla possibilità di usare certe periferiche di input/output.
- L'**altezza** influisce sul design di un chiosco interattivo.
- La **forza**: un giocattolo per bambini deve richiedere poca forza per essere attivato, ma molta per cambiare le batterie (sicurezza).
- **Disabilità** (vista, udito, destrezza).

## Come descriviamo gli utenti

![[ium_p05_s13.png]]

Tre strumenti collegati:
1. **Persona** – *chi* è la storia: il personaggio principale, con atteggiamenti, motivazioni, obiettivi e *pain points*.
2. **Scenario** – *quando, dove e come* si svolge la storia: la narrazione che descrive come la persona si comporta come sequenza di eventi (vedi [[06 - Scenari]]).
3. **Goal** – *cosa* vuole ottenere la persona: la motivazione per cui agisce; quando l'obiettivo è raggiunto lo scenario finisce.

## Persona

> "A persona is a **fictional, yet realistic**, description of a **typical or target user** of the product. A persona is an **archetype** instead of an actual living human, but personas should be described **as if they were real people**."
> — Nielsen Norman Group

- Catturano le **caratteristiche dell'utente**.
- **Non sono persone reali**, ma hanno caratteristiche **sintetizzate** dagli utenti reali.
- **Non devono essere idealizzate.**
- Devono essere **"vive"**: un nome, caratteristiche, scopi, conoscenze, attività sociali.
- Hanno un **insieme unico di obiettivi**: non solo il ruolo lavorativo (persone con lo stesso ruolo possono avere obiettivi diversi).
- Di solito si sviluppano **diverse personas**.

### Esempio (NN/g)

![[ium_p05_s16.png]]

*Rosa Cho, 34 anni, Seattle – "Company Investigator", Content Strategist freelance.* Citazioni: "Sto cercando l'azienda giusta, che mi metta alla prova e mi permetta di crescere"; "Amo la varietà nei settori in cui lavoro, così non mi annoio". Seguono: *About*, considerazioni comportamentali, frustrazioni, obiettivi, task.

### Cosa contiene una persona
- Descrizione **dettagliata** → uscire dal concetto generico di utente: bisogni, preoccupazioni, obiettivi, età, genere…
- Ragioniamo meglio su **istanze concrete** che in astratto.
- Le caratteristiche scelte possono essere la **sintesi di risultati statistici**.

### Quando si creano
- **Il prima possibile** nel processo di design.
- Sono il risultato di **user research**.
- Devono basarsi su **dati disponibili**, **non su desideri** su come vorremmo che fossero gli utenti.

### Cosa inserire
- **Nome, età, genere e una foto**.
- Un'**"etichetta"** che descriva cosa fa nella vita reale.
- **Livello di esperienza** nel dominio del prodotto/servizio.
- **Contesto d'uso**: ha scelta o lo usa per lavoro? Quanto spesso? Desktop, mobile o altro?
- **Obiettivi e problemi** nello svolgere un compito rilevante: velocità, accuratezza, completezza o altri fattori.
- Una **frase** che riassuma l'atteggiamento della persona.

> [!warning] Evitare troppe battute: la persona diventa divertente ma poco utile.

## Come identificare le caratteristiche degli utenti

| Tecnica | Punti di forza | Limiti |
| --- | --- | --- |
| **Interviste** | Si possono usare supporti (prototipi, scenari); ottime per indagare problemi | Richiedono molto tempo; impossibile visitare molti utenti |
| **Focus group** (interviste di gruppo) | Buoni per trovare consenso o aree di conflitto | Possono essere dominati da pochi individui |
| **Questionari** | Dati qualitativi o quantitativi; domande specifiche a molte persone; spesso combinati con altre tecniche | Poca profondità |
| **Ricerca di prodotti simili** | Buona per individuare requisiti | — |
| **Osservazione diretta** | Informazioni sui task e sulle parti coinvolte; capire natura e contesto dei task | Richiede tempo e un membro del team; produce enormi quantità di dati |
| **Osservazione indiretta** (es. log, diari) | Buona per tenere traccia dei task correnti | Poco usata per i requisiti |
| **Studio della documentazione** (manuali, procedure, regolamenti) | Passi di un'attività, leggi e regole; contesto; **non richiede tempo agli utenti** | Non può essere usato da solo |

## Esercizi

> [!example] Esercizio 1 – Analizzare una persona
> Leggere il template https://public-media.interaction-design.org/pdf/Persona-Template.pdf, identificare le varie informazioni e discutere le implicazioni sull'ideazione dell'interfaccia.

> [!example] Esercizio 2 – Scrivere una persona
> App per la gestione dell'**orario delle lezioni universitarie**: scrivere una persona che caratterizzi una categoria ragionevole di utenti.
>
> **Possibile soluzione**
> - **Giulia Serra, 20 anni**, studentessa pendolare al 2° anno di Informatica, vive a Oristano e prende il treno ogni giorno.
> - *Etichetta*: "La pendolare organizzata".
> - *Esperienza*: usa lo smartphone tutto il giorno, ha dimestichezza con app di calendario; conosce poco il sito di ateneo.
> - *Contesto*: usa l'app soprattutto **in treno o camminando**, da smartphone, spesso con connessione instabile; la consulta più volte al giorno.
> - *Obiettivi*: sapere velocemente aula e orario della prossima lezione; essere avvisata di **lezioni annullate o spostate** prima di partire.
> - *Frustrazioni*: avvisi pubblicati solo su e-learning o via mail all'ultimo minuto; orari in PDF illeggibili da telefono.
> - *Frase*: "Se una lezione salta, voglio saperlo prima di salire sul treno delle 7."

> [!example] Esercizio 3 – Personas con ChatGPT
> Ripetere l'esercizio scrivendo un prompt che riassuma le informazioni necessarie per creare una persona. Confrontare il risultato: quanto è plausibile? *(Tipicamente le personas generate sono plausibili ma generiche e stereotipate, perché non derivano da dati reali sugli utenti: violano la regola "basate su dati, non su desideri".)*

## Altri esempi di costruzione di personas

> [!info] Questi esempi non sono nelle slide: li ho aggiunti per esercitarsi. Seguono la struttura vista sopra (*Cosa inserire*) e mostrano anche **come** si arriva alla persona partendo dai dati.

### Esempio A – Dai dati alla persona (app di una biblioteca comunale)

**1. Dati di user research** (ipotetici, raccolti con questionari, osservazione al banco prestiti e interviste):
- il 60% degli utenti che prenotano libri ha più di 55 anni;
- molti chiedono al bibliotecario di prolungare il prestito per telefono;
- nelle interviste emergono difficoltà a leggere testi piccoli sullo smartphone e la paura di "sbagliare qualcosa" online;
- quasi tutti usano WhatsApp ogni giorno, pochi usano altre app.

**2. Sintesi in una persona**

| Campo | Contenuto |
| --- | --- |
| **Nome, età, foto** | Franco Pinna, 67 anni (foto di un uomo sorridente con gli occhiali) |
| **Etichetta** | "Il lettore fedele" – insegnante in pensione |
| **Esperienza nel dominio** | Frequenta la biblioteca da 30 anni, conosce bene il catalogo cartaceo; usa lo smartphone solo per WhatsApp e le foto dei nipoti |
| **Contesto d'uso** | Uso **volontario**, da smartphone, a casa, circa una volta a settimana |
| **Obiettivi** | Prenotare i libri in arrivo prima che finiscano; rinnovare i prestiti senza andare in biblioteca |
| **Problemi / frustrazioni** | Testi piccoli, pulsanti ravvicinati, paura di prenotare per sbaglio; le password lunghe da ricordare |
| **Frase** | "Mi piace la tecnologia, basta che non mi faccia sentire stupido." |

**3. Implicazioni di design**: testo grande e ridimensionabile; pulsanti grandi e distanziati (legge di Fitts, [[11 - Udito, Tatto e Movimento]]); conferma prima di ogni prenotazione e possibilità di annullare; accesso con codice della tessera invece di una password complessa; promemoria di scadenza del prestito.

### Esempio B – Più personas per la stessa app (orario delle lezioni)

Di solito si sviluppano **diverse personas**, perché persone diverse hanno **obiettivi diversi** anche usando lo stesso prodotto. Oltre a Giulia (la pendolare, vedi *Esercizio 2*):

> [!example] Marco Deiana, 27 anni – "Lo studente lavoratore"
> - Iscritto part-time a Economia, lavora come commesso in un negozio con turni che cambiano ogni settimana.
> - **Esperienza**: buona con lo smartphone, usa Google Calendar per i turni di lavoro.
> - **Contesto**: consulta l'app la domenica sera per organizzare la settimana; non segue tutte le lezioni.
> - **Obiettivi**: capire quali lezioni sono compatibili con i turni; sapere dove trovare le registrazioni di quelle che perde.
> - **Frustrazioni**: dover copiare a mano gli orari nel suo calendario; non sapere se una lezione verrà registrata.
> - **Frase**: "Non posso esserci sempre, ma non voglio restare indietro."
> - → *Implicazioni*: **esportazione** dell'orario su Google Calendar; filtro "solo i miei corsi"; indicazione del link alla registrazione accanto a ogni lezione.

> [!example] Prof.ssa Anna Melis, 52 anni – "La docente con poco tempo"
> - Docente di Analisi, tiene tre corsi in due sedi diverse.
> - **Esperienza**: usa il computer ogni giorno per lavoro, ma considera le app "una perdita di tempo".
> - **Contesto**: usa il sistema **per obbligo** (non per scelta), da PC in ufficio, quando deve spostare una lezione.
> - **Obiettivi**: comunicare in fretta un cambio d'aula o un annullamento a tutti gli studenti del corso.
> - **Frustrazioni**: dover avvisare su tre canali diversi (e-learning, email, Teams); studenti che non leggono gli avvisi.
> - **Frase**: "Se devo spostare una lezione, voglio farlo in un minuto e sapere che tutti l'hanno saputo."
> - → *Implicazioni*: funzione "sposta/annulla lezione" con **notifica automatica** a tutti gli iscritti; template di messaggio già pronto.

Notare che Giulia, Marco e Anna portano a **funzionalità diverse**: notifiche push (Giulia), esportazione del calendario (Marco), pubblicazione rapida degli avvisi (Anna). Senza personas si rischierebbe di progettare per un "utente generico" che non esiste.

### Esempio C – Persona fatta male vs persona fatta bene

❌ **Persona da evitare**
> *Luca, 25 anni, ama la tecnologia e trova l'app intuitiva e bellissima. Vuole un'app veloce e moderna. È simpaticissimo, il suo motto è "pizza e codice" 🍕💻.*

Problemi:
- è **idealizzata**: descrive l'utente che vorremmo, non quello reale ("trova l'app intuitiva");
- usa aggettivi di "fuffa" (*veloce, moderna, intuitiva*, vedi [[03 - Definizioni e Terminologia]]);
- **non ci sono obiettivi né problemi** concreti → non aiuta a prendere decisioni di design;
- troppe battute: diventa divertente ma inutile;
- non dice **nulla sul contesto d'uso** (dove, quando, con quale dispositivo, per scelta o per obbligo).

✅ **Persona corretta (stesso target)**
> *Luca Sanna, 25 anni, sviluppatore junior in una startup di Cagliari. Usa ogni giorno strumenti come GitHub e Slack e si aspetta scorciatoie da tastiera e notifiche configurabili. Usa l'app di gestione progetti per lavoro (non per scelta), da desktop, molte volte al giorno. Obiettivo: vedere in un colpo d'occhio i task assegnati a lui e aggiornarne lo stato senza aprire mille finestre. Frustrazione: le notifiche per ogni minima modifica che gli fanno perdere la concentrazione. "Voglio sapere cosa devo fare oggi, non tutto quello che succede nel team."*

È un **utente esperto e frequente** → ha bisogno di flessibilità, accesso rapido alle funzioni e *short-cuts* (vedi *Le caratteristiche dell'utente*).

### Checklist per verificare una persona

- [ ] Ha **nome, età, genere, foto** e un'**etichetta**?
- [ ] È basata su **dati** di user research e non su desideri?
- [ ] Indica il **livello di esperienza** nel dominio?
- [ ] Descrive il **contesto d'uso** (dispositivo, frequenza, scelta/obbligo)?
- [ ] Ha **obiettivi** e **problemi** concreti legati al prodotto?
- [ ] Ha una **frase** che riassume l'atteggiamento?
- [ ] È **realistica** (non idealizzata) e senza troppe battute?
- [ ] Da essa si possono ricavare **implicazioni di design**?

## Riferimenti
- Dix, Finlay, Abowd, Beale – *Interazione Uomo Macchina*, cap. 5
- Rogers, Sharp, Preece – *Interaction Design*, cap. 9-10-11
- Approfondimento: Bill Buxton – *Sketching user experiences*
- NN/g – *Personas*: https://www.nngroup.com/articles/persona/
- Interaction Design Foundation – *Four different perspectives on user personas*
