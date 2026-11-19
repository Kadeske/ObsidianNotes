---
corso: Interazione Uomo Macchina
parziale: 1
argomento: Creazione di prototipi
tags: [IUM, teoria, parziale1, prototipi]
---

# 07 - Creazione di prototipi

## Di cosa parleremo
- Costruzione e creazione di prototipi
- Design concettuale
- Design fisico
- Supportare il design con i prototipi

> [!tip] Approfondimento – Design concettuale e design fisico
> Sono nominati nell'indice ma le relative slide sono nascoste; ecco una definizione sintetica (Preece, Rogers, Sharp):
> - **Design concettuale**: definire *cosa* farà il prodotto e *come* si comporterà a livello astratto → il **modello concettuale** (concetti, oggetti e relazioni che l'utente deve capire, tipo di interazione, metafore — vedi [[13 - Pensiero, ragionamento e metafore]]).
> - **Design fisico**: dare forma concreta al modello concettuale → schermate, layout, colori, icone, suoni, dispositivi di input/output, dimensioni dei controlli.

## Panoramica
- **Cos'è** un prototipo?
- **Perché** creare un prototipo?
- Diversi tipi di prototipi: **bassa fedeltà** (*low fidelity*) e **alta fedeltà** (*high fidelity*)
- **Compromessi**: prototipi **verticali** e **orizzontali**
- **Costruzione** del prodotto

## Cos'è un prototipo

In generale un prototipo è un **modello del prodotto che tralascia alcuni aspetti** della sua realizzazione (colore, scala, funzionalità…).
Esempi: modellini di auto, plastici di città o palazzi, a volte oggetti creati con stampanti 3D.

Per un'interfaccia, un prototipo può essere:
- una serie di **schizzi** delle schermate;
- uno **storyboard** (serie di scene simili a un cartone animato o fumetto);
- una presentazione **PowerPoint**;
- un **video** che simula l'uso del sistema;
- un **mock-up** di cartone;
- un **software con funzionalità limitate**, scritto in qualsiasi linguaggio (non serve che sia quello del prodotto finale).

## Perché creare un prototipo

- **Esplorare**
  - rendere concreto un concetto astratto;
  - capire pregi e difetti di un'idea.
- **Raffinare l'idea**
  - studiare la **fattibilità tecnica**;
  - studiare pregi e difetti di una soluzione;
  - **identificare in anticipo** eventuali problemi.
- Si usano materiali e tecniche **diversi** dal prodotto finale per risparmiare **tempo** e **denaro**.

Inoltre:
- **Valutazione e feedback** sono fondamentali per ottenere buone interfacce.
- Chi partecipa al design può **vedere, usare e capire** un prototipo molto più facilmente di un documento.
- I membri del team **comunicano** meglio.
- Le idee si testano meglio anche da soli.
- Incoraggia la **riflessione**, aspetto molto importante nel design.
- I prototipi **rispondono a domande** e supportano la **scelta tra alternative**.

## Esempio: un telaio da bicicletta

![[ium_p07_s08.jpg]]

Il prodotto finale: la bici da cronometro Trek.

![[ium_p07_s09.jpg]]

Le iterazioni: **schizzi a mano** → modelli CAD → **prototipi fisici** → modello 3D definitivo. Ogni prototipo esplora aspetti diversi (forma, aerodinamica, struttura).

## Possibili dimensioni da filtrare

Un prototipo è un "filtro": sceglie quali aspetti rappresentare e quali tralasciare.

| Dimensione | Variabili di esempio |
| --- | --- |
| **Aspetto** (*appearance*) | dimensione, colore, forma, margini, peso, texture, proporzioni, durezza, trasparenza, gradazione, feedback aptico, suono |
| **Dati** | dimensione dei dati, tipo (numero, stringa, media), uso dei dati, tipo di privacy, gerarchia, organizzazione |
| **Funzionalità** | funzioni del sistema, funzionalità di cui l'utente ha bisogno |
| **Interattività** | comportamento di input, di output, di feedback, informativo |
| **Struttura spaziale** | disposizione degli elementi dell'interfaccia o dell'informazione e relazioni tra di essi (2D o 3D, tangibile o intangibile, o misto) |

> [!tip] Approfondimento
> La tabella viene da Lim, Stolterman e Tenenberg (2008), *The anatomy of prototypes*: un prototipo va pensato come un **filtro** (quali dimensioni del design esplora) e come una **manifestazione** (con quali materiali e a quale livello di dettaglio è realizzato). Il miglior prototipo è il più semplice che permette di rispondere alla domanda di design che ci interessa.

## Cosa prototipare?
- Ciò che può causare **problemi tecnici**.
- Il **flusso di lavoro**.
- La **sequenza di azioni** per completare un task.
- Il **layout** delle schermate.
- Le **informazioni** da visualizzare.
- Le aree dove si riscontrano **difficoltà o criticità**.

## Prototipi a bassa fedeltà (*low-fidelity*)

- Usano un **medium diverso** da quello del prodotto finale (carta, cartone, post-it…).
- Sono **veloci**, **economici** e **modificabili** rapidamente.
- Esempi: schizzi delle schermate, sequenze dei task, note su post-it, storyboard, **"Mago di Oz"**.

### Schizzi delle schermate (*sketch*)
- Si disegnano le schermate **su carta**, tralasciando gli aspetti grafici.
- Si creano e si buttano via velocemente.
- Non serve grande abilità manuale.
- **Meglio matita e foglio del computer**: il programma limita ciò che puoi inserire, mentre il disegno aiuta a riflettere.

![[ium_p07_s13.jpg]]
![[ium_p07_s13b.jpg]]

**Esempio per un sito web**: si usano cartoncini (dimensione di una cartolina) e in ognuno si disegna una schermata o una parte di essa; così si possono riordinare e simulare la navigazione.

![[ium_p07_s14.jpg]]

### Schizzi di interazioni/configurazioni
Si rappresenta graficamente un'interazione: si **visualizza il contesto** e si **annota/descrive** ciò che accade.

![[ium_p07_s15.jpg]]
![[ium_p07_s15b.jpg]]

### "Ma io non so disegnare!"

![[ium_p07_s16.png]]

Si possono usare moltissimi espedienti: ricalcare una foto, partire da una foto e semplificarla, usare forme geometriche e omini stilizzati. Lo scopo dello schizzo è comunicare un'idea, non fare arte.

### Storyboard
- Spesso usati **insieme agli scenari**, aggiungendo dettagli e la possibilità di interpretare un ruolo preciso.
- Serie di schizzi che mostrano come un utente **progredisce in un task** usando un dispositivo.
- Usati nelle **fasi iniziali** del design.
- Rappresentano solo gli **"stati"** dell'interazione:
  - **non** le transizioni da uno stato all'altro;
  - difficile rappresentare situazioni dinamiche.
- Spesso sono **annotati**.

![[ium_p07_s18.png]]

### Il "Mago di Oz" (*Wizard of Oz*)
- Si **finge che il sistema esista** e si usa qualche trucco per permettere l'interazione prima dell'implementazione.
- L'utente pensa di interagire con un **sistema reale**.
- In realtà il risultato delle operazioni è deciso da uno **sviluppatore nascosto** ("il mago") o da un altro espediente.
- Di solito si fa nelle **fasi iniziali** per capire cosa l'utente si aspetta dal sistema.
- Può avere problemi di **tempi di risposta** (es. un umano che cerca l'informazione al posto del sistema è più lento).

> [!info] Il nome viene dal film *Il mago di Oz*: il potente mago si rivela un uomo comune che manovra una macchina dietro una tenda.

**Esempio: funzionalità di dettatura del testo** — l'utente parla al computer; in un'altra stanza una persona trascrive ciò che sente e il testo appare sullo schermo dell'utente, come se fosse un riconoscimento vocale.

![[ium_p07_s20.jpg]]

**Esempio: lavagna virtuale condivisa** — due persone scrivono su due "lavagne" in luoghi diversi; il collegamento è simulato con telecamere e proiettori, senza alcun software di sincronizzazione.

![[ium_p07_s21.jpg]]

## Prototipi ad alta fedeltà (*high-fidelity*)
- Usano **materiali che ci si aspetta nel prodotto finale**.
- Somigliano **più alla versione finale** che a una bozza.
- Di solito si realizzano con un **vero linguaggio di programmazione** (o tool come Figma con prototipazione interattiva).
- ⚠️ Pericolo: gli utenti pensano di avere tra le mani un **sistema completo** (e i committenti che sia "quasi finito").

> [!tip] Approfondimento – Bassa vs alta fedeltà
>
> | | Bassa fedeltà | Alta fedeltà |
> | --- | --- | --- |
> | Costo / tempo | Basso | Alto |
> | Modificabilità | Altissima | Bassa |
> | Fase | Iniziale, esplorazione | Avanzata, raffinamento |
> | Feedback degli utenti | Su concetti e flusso (si sentono liberi di criticare) | Su dettagli, look & feel, tempi |
> | Rischi | Poco utile per test di usabilità dettagliati | Ci si affeziona alla soluzione; gli utenti si fermano ai dettagli estetici |

## Compromessi nella creazione

Tutti i prototipi richiedono **compromessi**. Per i prototipi software: risposta del sistema lenta? Icone abbozzate? Funzionalità limitate?

Due tipi di compromesso:
- **Orizzontale**: un **ampio insieme di funzionalità** ma con **pochi dettagli** (tutta l'interfaccia, ma "superficiale").
- **Verticale**: un **alto livello di dettaglio** per **poche funzionalità** (una funzione implementata "in profondità").

![[ium_p07_s23.png]]

Nella figura: il rettangolo rappresenta l'**interfaccia completa** (*full interface*); la fascia orizzontale in alto è il **prototipo orizzontale**, la colonna verticale è il **prototipo verticale**; la loro intersezione è uno **scenario** (un singolo percorso supportato sia in ampiezza che in profondità, quanto basta per un test).

I compromessi nei prototipi **non possono essere ignorati**: i prodotti devono essere studiati a fondo.

## Costruzione del prodotto

- Prendere i prototipi (o imparare da essi) e creare un **tutt'uno**.
- Attenzione alla **qualità**: usabilità, affidabilità, robustezza, manutenibilità, integrità, portabilità, efficienza…
- Il prodotto deve essere **ingegnerizzato**:
  - **evoluzione dei prototipi** (*evolutionary prototyping*): il prototipo viene via via raffinato fino a diventare il prodotto;
  - **prototipi usa e getta** (*throw-away*): servono solo per imparare, poi si riscrive il prodotto da zero con criteri ingegneristici.

## Riferimenti
- Dix, Finlay, Abowd, Beale – *Interazione Uomo Macchina*, cap. 5
- Rogers, Sharp, Preece – *Interaction Design*, cap. 9-10-11
- Approfondimento: Bill Buxton – *Sketching user experiences*
