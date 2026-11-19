---
corso: Interazione Uomo Macchina
parziale: 1
argomento: Architettura dell'Informazione
tags: [IUM, teoria, parziale1, information-architecture]
---

# 08 - Architettura dell'Informazione

## Di cosa si tratta

![[ium_p08_s02.png]]

Come riempire le diverse parti di una UI con informazioni utili per l'utente e come scegliere il **layout** in base alle informazioni da mostrare. Ci si concentra soprattutto sul **web**, dove il problema dell'organizzazione dell'informazione è più comune, ma molti concetti valgono anche per altri tipi di UI.

## Architettura dell'informazione (IA)

1. Il **design strutturale** di ambienti di informazione condivisi.
2. L'arte e la scienza di creare prodotti che usano l'informazione per supportare **usabilità** e **reperibilità** (*findability*).
3. Una disciplina emergente e un insieme di pratiche comuni per l'organizzazione.

**In pratica: come strutturare l'informazione e aiutare a trovarla.**

### Tre ingredienti
1. **Strutturare** l'informazione
2. Creare **etichette**
3. Supportare la **navigazione**

## Strutturare l'informazione

- **Livello di granularità** – quanto "grandi" sono le unità di contenuto (un intero articolo, un paragrafo, una singola notizia?)
- **Categorie** – come raggruppare i contenuti
- **Vocabolario** – quali termini usare per nominarli

![[ium_p08_s05.png]]

*Esempio: il menu di CNN, con decine di categorie e sottocategorie (World, US Politics, Business, Health, Entertainment, Tech, Style, Travel, Sports…).*

## Trovare e gestire

**Gli utenti devono trovare l'informazione**: navigando, cercando, chiedendo.

**Gestione**: creare contenuto, aggiornare l'informazione, processi, politiche.

> [!note] Trovare l'informazione è critico: se l'utente non la trova, è come se non esistesse. Ma anche chi **gestisce** il contenuto deve poterlo creare e aggiornare: se l'architettura è ingestibile per loro, il sito diventa inutile. Bisogna **bilanciare i bisogni degli utenti e quelli dell'organizzazione**.

## Comportamenti nella ricerca

![[ium_p08_s07.png]]

| Comportamento | Cosa cerca l'utente | Esempi |
| --- | --- | --- |
| **Ricerca di un contenuto noto** (*known-item seeking*) | *"The right thing"*: una risposta specifica | Cosa vuol dire "prototipo"? Il risultato di una partita |
| **Ricerca esplorativa** (*exploratory seeking*) | *"A few good things"*: qualcosa di buono, senza risposta "giusta" | Un buon ristorante in zona, un letto nuovo |
| **Ricerca esaustiva** (*exhaustive research*) | *"Everything"*: tutto su un argomento | Tutto su una malattia di un amico |
| **Ritrovare informazione** (*re-finding*) | *"Need it again"*: rivedere qualcosa già visto | Segnalibri, liste "leggi dopo" |

### Cercare un contenuto conosciuto
Si cerca una **risposta specifica**: di solito sappiamo cosa cerchiamo, come si chiama e dove trovarlo (es. Google per una definizione, l'app sportiva per un risultato).

![[ium_p08_s08.png]]

### Ricerca esplorativa
**Ricerca aperta**: si impara qualcosa su un argomento cercando ed esplorando.
- L'utente non sa esattamente cosa cerca: sta imparando dal processo stesso di ricerca.
- Non esiste una risposta giusta; all'inizio non sa nemmeno formulare bene la domanda e parte da risultati preliminari.
- Alcuni risultati vengono saltati euristicamente.
- Non c'è un criterio standard per stabilire quando la ricerca è finita.
- Utili: **filtri e faccette** (vedi screenshot: ricerca ristoranti con filtri per quartiere, distanza, prezzo, caratteristiche).

![[ium_p08_s09.png]]

### Ricerca esaustiva
**Conoscere tutto** su un argomento: es. ci si informa su una malattia cercando più volte con termini diversi ("morbillo", "measles", sintomi…). L'utente vuole essere sicuro di **non perdere nulla**, quindi ripete la ricerca con parole chiave diverse ed è più **paziente** nell'analisi dei risultati.

![[ium_p08_s10.png]]

### Ritrovare informazione
Metodo per **rivedere un'informazione interessante in un altro momento**: es. si vede una notizia interessante ma non è il momento di leggerla → la si salva. È ciò che supportano **segnalibri** e liste **"da leggere"**.

## Strutturare il contenuto

Dare una struttura a una categoria:
- **Alfabetica** – prodotti, elementi di una lista…
- **Cronologica** – notizie, eventi, aggiornamenti
- **Spaziale / geografica** – fatti o dinamiche legati a luoghi
- **Per argomento** – didattica, arte…
- **Per compito** (*task*) – amministrazione, lavoro…

> [!tip] Approfondimento – Schemi esatti e ambigui
> Morville & Rosenfeld distinguono schemi **esatti** (alfabetico, cronologico, geografico: dividono l'informazione in sezioni ben definite e mutuamente esclusive, ottimi per la ricerca di contenuti noti) e schemi **ambigui** (per argomento, per compito, per pubblico: richiedono interpretazione, ma sono più adatti alla ricerca esplorativa).

## Etichette

![[ium_p08_s13.png]]

- Devono far scattare l'**associazione corretta** (es. "Contact us" → so cosa troverò).
- Usare il **linguaggio dell'utente**, non quello interno dell'azienda.
- I **concetti nuovi vanno spiegati**.

Le etichette riassumono i contenuti ed evitano di sommergere l'utente con informazioni non principali: sceglierle bene è cruciale per la UX.

### Esempio (easyJet)

![[ium_p08_s14.jpg]]

Ogni voce di menu ha un'**etichetta breve** + una **descrizione** che chiarisce cosa contiene (es. *Flight tracker – Check your flight status*).

### Errori frequenti
- **Gruppi errati**.
- Etichette **non rappresentative** e che **non si distinguono** tra loro.
- Etichette con **gergo** non noto all'utente.
- **Mescolare lingue**.
- **Errori di scrittura** o grammaticali.

### Link contestuali
- Collegamenti a **contenuti correlati**, fuori dalla gerarchia di navigazione.
- **Importanza del contesto**: il contesto aiuta a prevedere dove porta un link; se l'etichetta non è chiara, può essere una sorpresa.

![[ium_p08_s16.png]]

*In Wikipedia è chiaro che i link portano ad altre voci dell'enciclopedia; in un blog (sotto) dove porterà il link blu? A un altro post o a una risorsa esterna?*

### Etichette comuni (convenzioni)
- Main, Main Page, **Home**
- **Search**, Find, Browse, Search/Browse
- **Site Map**, Contents, Table of Contents, Index
- Contact, **Contact Us**
- Help, **FAQ**, Frequently Asked Questions
- **News**, News & Events, News & Announcements, Announcements
- About, **About Us**, About <company name>, Who We Are

> Usare le etichette convenzionali riduce il carico cognitivo: l'utente le riconosce senza doverle interpretare.

## Sistemi di navigazione

![[ium_p08_s18.png]]

| Tipo | Domanda a cui risponde | Dove si trova |
| --- | --- | --- |
| **Navigazione globale** | *Dove sono?* | Barra presente in tutte le pagine (in alto, su desktop e mobile) |
| **Navigazione locale** | *Cosa c'è qui vicino?* | Menu della sezione corrente (es. colonna laterale) |
| **Navigazione contestuale** | *Cosa è collegato a ciò che sto guardando?* | Link nel contenuto stesso |

> [!tip] Approfondimento – Altri sistemi di navigazione
> Oltre a questi sistemi "incorporati" nelle pagine, esistono sistemi **supplementari**: **mappa del sito**, **indice A–Z**, **guide/tour** e la **ricerca** interna. Utili soprattutto nei siti grandi o quando la navigazione principale fallisce. Elementi come le **breadcrumb** ("Home > Corsi > IUM") aiutano a rispondere a "dove sono?".

## Letture
- Peter Morville, Louis Rosenfeld – *Information Architecture*, 4ª ed. (abbastanza lungo…)
  - Cap. 2 *Defining Information Architecture*
  - Cap. 3 *Designing for Finding*
  - Cap. 7 *Labelling Systems*
  - Cap. 8 *Navigation Systems*
