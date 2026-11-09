# 06 - Prototipi ed euristiche di Nielsen

> Fonte: [[IUM/TEORIA/PDF/06-prototipi.pdf]]

Il PDF contiene due lezioni: **creazione di prototipi** (parte 1) e **le 10 euristiche di usabilità di Nielsen** (parte 2).

---

# Parte 1 — Creazione di prototipi

## 1. Cos'è un prototipo

Un **prototipo** è un **modello del prodotto che tralascia alcuni aspetti** della sua realizzazione (colore, scala, funzionalità…). Esempi: modellini di auto, plastici di città o edifici, oggetti stampati in 3D.

Per un'interfaccia, un prototipo può essere:
- una serie di **schizzi** delle schermate;
- uno **storyboard** (scene come in un fumetto o in un cartone animato);
- una **presentazione** PowerPoint;
- un **video** che simula l'uso del sistema;
- un **mock-up** di cartone;
- un **software con funzionalità limitate**, in qualsiasi linguaggio (non necessariamente quello del prodotto finale).

## 2. Perché creare prototipi

- **Esplorare**: rendere concreto un concetto astratto e capire pregi e difetti di un'idea.
- **Raffinare**: studiare la fattibilità tecnica, valutare una soluzione, individuare **in anticipo** i problemi.
- **Risparmiare tempo e denaro**, usando materiali e tecniche diversi da quelli del prodotto finale.
- **Ottenere valutazioni e feedback**, fondamentali per buone interfacce: un prototipo si vede, si usa e si capisce più facilmente di un documento.
- **Comunicare** meglio nel team e testare le idee anche da soli.
- **Riflettere**, aspetto fondamentale nel design.
- **Rispondere a domande** e **scegliere tra alternative**.

**Esempio — telaio di una bici da corsa (Trek)**: dal prodotto finito si risale alle iterazioni: schizzi a mano, modelli 3D al computer, prototipo fisico, rendering finale.

![[IUM06-iterazioni-bici.jpg|600]]

## 3. Cosa prototipare

Ciò che può causare **problemi tecnici** · il **flusso di lavoro** · la **sequenza di azioni** per completare un task · il **layout** delle schermate · le **informazioni** da visualizzare · le aree **critiche** o difficili.

### Dimensioni che un prototipo può "filtrare"

| Dimensione | Variabili di esempio |
|---|---|
| **Aspetto** (*appearance*) | Dimensione, colore, forma, margini, peso, texture, proporzioni, durezza, trasparenza, sfumature, aptica, suono |
| **Dati** | Quantità, tipo (numero, stringa, media), uso, privacy, gerarchia, organizzazione |
| **Funzionalità** | Funzioni del sistema, funzionalità necessarie agli utenti |
| **Interattività** | Comportamento di input, output, feedback, informazioni |
| **Struttura spaziale** | Disposizione degli elementi dell'interfaccia o dell'informazione e loro relazioni (2D o 3D, tangibili, intangibili o miste) |

## 4. Prototipi a bassa fedeltà (*low-fidelity*)

- Usano un **medium diverso** da quello del prodotto finale.
- Sono **veloci, economici** e si modificano rapidamente.
- Esempi: schizzi delle schermate, sequenze di task, post-it, storyboard, «Mago di Oz».

### Schizzi delle schermate
- Si disegnano le schermate **su carta**, tralasciando gli aspetti grafici; si creano e si buttano via velocemente.
- Non serve saper disegnare (si possono usare moltissimi espedienti).
- **Meglio carta e matita del computer**: il programma limita ciò che si può inserire, mentre disegnare aiuta a riflettere.
- Per un sito web si possono usare cartoncini formato cartolina, uno per schermata o parte di schermata.

![[IUM06-schizzo-agenda.jpg|260]] ![[IUM06-schizzi-sito.png|260]]

### Schizzi di interazioni e configurazioni
Si rappresenta graficamente un'interazione, **visualizzando il contesto** e annotando.

![[IUM06-schizzo-interazione.jpg|450]]

### Storyboard
- Spesso usati **insieme agli scenari**, con più dettagli e la possibilità di immedesimarsi in un ruolo.
- Una serie di schizzi che mostrano come un utente **avanza in un task** usando un dispositivo.
- Usati nelle **fasi iniziali** del design, spesso annotati.
- Rappresentano solo gli **stati** dell'interazione, non le transizioni: difficile rappresentare situazioni dinamiche.

![[IUM06-storyboard.png|450]]

### Il «Mago di Oz» (*Wizard of Oz*)
- Si **finge che il sistema esista**: l'utente crede di interagire con un sistema reale, ma il risultato delle operazioni lo decide **uno sviluppatore nascosto** (o un altro espediente).
- Si usa nelle **fasi iniziali**, per capire cosa l'utente si aspetta dal sistema.
- Problema: i **tempi di risposta** (es. se è un umano a cercare l'informazione richiesta).

Esempi: **dettatura** del testo (un operatore nascosto trascrive ciò che l'utente dice) e **lavagna virtuale condivisa**.

![[IUM06-oz-dettatura.jpg|320]] ![[IUM06-oz-lavagna.png|320]]

## 5. Prototipi ad alta fedeltà (*high-fidelity*)

- Usano i **materiali previsti per il prodotto finale**: somigliano più al sistema finito che a una bozza.
- Di solito si realizzano con un vero linguaggio di programmazione.
- ⚠️ Rischio: gli utenti credono di avere tra le mani un **sistema completo**.

| | Bassa fedeltà | Alta fedeltà |
|---|---|---|
| Medium | Diverso dal prodotto finale (carta, post-it…) | Lo stesso del prodotto finale |
| Costo e tempo | Bassi | Alti |
| Modifiche | Rapide | Lente |
| Fase tipica | Iniziale, esplorazione | Avanzata, raffinamento |
| Rischio | Poco realistico | Scambiato per il sistema finito |

## 6. Compromessi: prototipi orizzontali e verticali

Ogni prototipo richiede **compromessi** (risposte lente? icone abbozzate? funzionalità limitate?):

- **Orizzontale**: un **ampio insieme di funzionalità** con **pochi dettagli**.
- **Verticale**: **molto dettaglio** per **poche funzionalità**.
- Un **scenario** corrisponde all'incrocio tra i due: le funzioni necessarie a quel percorso, al dettaglio necessario.

![[IUM06-orizzontale-verticale.png|380]]

I compromessi non si possono ignorare: il prodotto va studiato a fondo.

## 7. Costruzione del prodotto

- Si prendono i prototipi (o ciò che si è imparato) e si crea un **tutt'uno**.
- Attenzione alla qualità: usabilità, affidabilità, robustezza, manutenibilità, integrità, portabilità, efficienza.
- Il prodotto va **ingegnerizzato**: per **evoluzione dei prototipi** oppure partendo da zero dopo **prototipi usa e getta**.

---

# Parte 2 — Le 10 euristiche di usabilità di Jakob Nielsen

Dieci **regole pratiche**: non risolvono tutti i problemi di design, ma aiutano nei casi più frequenti. Gli esempi (per il web) sono raccolti da Theresa Neil.

### 1. Visibilità dello stato del sistema
Il sistema deve **sempre informare l'utente** su cosa sta succedendo, con un **feedback appropriato in tempi ragionevoli**.
*Esempi*: barra di avanzamento durante l'upload; messaggio di conferma dopo un'azione; indicatore "live" della robustezza della password.

![[IUM06-h1-upload.png|320]]

### 2. Corrispondenza tra sistema e mondo reale
Il sistema deve **parlare la lingua dell'utente** (parole, frasi e concetti familiari a lui, non al sistema), seguire le **convenzioni del mondo reale** e presentare le informazioni in un **ordine logico e naturale**.
*Esempi*: la metafora della **libreria** per i propri contenuti (iTunes); una **mappa mentale** invece di un elenco puntato.

![[IUM06-h2-libreria.png|180]]

### 3. Libertà e controllo per l'utente
Gli utenti scelgono spesso funzioni **per errore**: servono **"uscite di sicurezza"** chiare per abbandonare lo stato indesiderato senza un lungo percorso. Supportare **annulla e ripeti** (*undo/redo*).
*Esempi*: annullare facilmente una modifica; pannelli facili da aprire e da chiudere; pulsanti undo/redo; indicare chiaramente dove ci si trova (es. breadcrumb) e come spostarsi.

### 4. Consistenza e standard
Gli utenti non devono chiedersi se parole, situazioni o azioni diverse **significano la stessa cosa**: seguire le **convenzioni della piattaforma**.
*Esempi*: Gmail ha mantenuto la terminologia degli altri client email (Inbox, Sent, Drafts…) pur essendo nato dopo; i menu di Office usano gli stessi termini per funzioni equivalenti in tutte le applicazioni.

![[IUM06-h4-gmail.png|200]]

### 5. Prevenzione degli errori
Meglio di un buon messaggio d'errore è un design che **impedisce l'errore**: eliminare le condizioni che inducono in errore e chiedere **conferma** prima delle azioni definitive.
*Esempi*: disabilitare il pulsante di upload mentre l'operazione è in corso; rendere la funzione più frequente la più accessibile (**azione primaria** evidente, **secondaria** come semplice link); **autofocus** per non perdere l'input; suggerimenti per evitare errori di ortografia.

![[IUM06-h5-azione-primaria.png|320]]

### 6. Riconoscimento piuttosto che ricordo
Ridurre il **carico di memoria** rendendo visibili azioni, oggetti e opzioni. L'utente non deve ricordare informazioni da una parte all'altra del dialogo; le istruzioni devono essere visibili o facilmente reperibili.
*Esempi*: **autocompletamento** della sintassi in un editor di codice; menu dei font che mostra ogni font **con il suo aspetto** (riconoscimento dalla forma, non dal nome).

![[IUM06-h6-autocompletamento.png|260]] ![[IUM06-h6-font.png|260]]

### 7. Flessibilità ed efficienza d'uso
**Acceleratori** (es. combinazioni di tasti), invisibili ai principianti, velocizzano gli esperti: il sistema serve bene entrambi. Permettere di **personalizzare** le azioni frequenti.
*Esempi*: elenco delle scorciatoie da tastiera; azioni frequenti mostrate accanto all'elemento selezionato.

![[IUM06-h7-shortcut.png|260]]

### 8. Estetica e design minimale
Non mostrare informazioni **irrilevanti o usate raramente**: ogni elemento in più **compete** con quelli rilevanti e ne riduce la visibilità.
*Esempi*: **contrasto** (etichette in grassetto), **ripetizione** (colori diversi per i tipi di media), **vicinanza** (gruppi separati da una linea leggera); spaziatura adeguata invece di bordi superflui.

### 9. Aiutare a riconoscere, diagnosticare e rimediare agli errori
I messaggi d'errore devono essere in **linguaggio comune** (senza codici), **indicare con precisione il problema** e **suggerire una soluzione**.
*Esempi*: feedback immediato accanto al campo sbagliato di un modulo; pagina 404 umoristica che offre comunque link utili per continuare.

![[IUM06-h9-errori-form.png|320]]

### 10. Documentazione e aiuto
Sarebbe meglio un sistema utilizzabile senza documentazione, ma può servire un aiuto: deve essere **facile da cercare**, **centrato sui compiti dell'utente**, con **passi concreti** e **non troppo lungo**.
*Esempio*: collegamenti all'aiuto **contestuali**, proprio dove servono.

![[IUM06-h10-help.png|280]]

## Riferimenti
- Dix, Finlay, Abowd, Beale — *Interazione Uomo Macchina*, cap. 5
- Rogers, Sharp, Preece — *Interaction Design*, cap. 9–11
- Approfondimento: B. Buxton — *Sketching user experiences*
- Esempi delle euristiche: Theresa Neil, <http://designingwebinterfaces.com/6-tips-for-a-great-flex-ux-part-5>

---

## Termini e concetti da ricordare

**Prototipi**
- **Prototipo**: modello del prodotto che **tralascia alcuni aspetti**; serve a esplorare, raffinare, comunicare, valutare e scegliere tra alternative a basso costo.
- **Dimensioni filtrabili**: aspetto, dati, funzionalità, interattività, struttura spaziale.
- **Bassa fedeltà**: medium diverso, veloce ed economica (schizzi, post-it, storyboard, Mago di Oz).
- **Storyboard**: sequenza di schizzi degli **stati** di un task (non delle transizioni).
- **Mago di Oz**: un operatore nascosto simula il sistema; problema dei tempi di risposta.
- **Alta fedeltà**: materiali del prodotto finale; rischio che sembri un sistema completo.
- **Prototipo orizzontale** (tante funzioni, poco dettaglio) vs **verticale** (poche funzioni, molto dettaglio); lo **scenario** sta all'incrocio.
- **Evoluzione dei prototipi** vs **prototipi usa e getta**.

**Le 10 euristiche di Nielsen**
1. Visibilità dello stato del sistema (feedback).
2. Corrispondenza tra sistema e mondo reale (linguaggio dell'utente, metafore).
3. Libertà e controllo (uscite di emergenza, undo/redo).
4. Consistenza e standard (convenzioni della piattaforma).
5. Prevenzione degli errori (vincoli, conferme).
6. Riconoscimento piuttosto che ricordo (rendere visibili opzioni e azioni).
7. Flessibilità ed efficienza (acceleratori per esperti).
8. Estetica e design minimale (niente informazioni superflue).
9. Riconoscere e rimediare agli errori (messaggi chiari e costruttivi).
10. Documentazione e aiuto (cercabile, orientato ai task, contestuale).
