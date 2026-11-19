---
corso: Interazione Uomo Macchina
parziale: 1
argomento: La componente umana – La percezione visiva
tags: [IUM, teoria, parziale1, componente-umana, percezione, colore]
---

# 09 - La componente umana: la percezione visiva

## L'essere umano

Cercheremo di capire come **"funziona"** un essere umano:
- costruendo **modelli** per ragionare durante il design di interfacce;
- **semplificando**;
- concentrandoci sulle parti che ci interessano.

Il modello che useremo (Dix et al.) vede l'uomo come un **sistema di elaborazione dell'informazione**:
- l'informazione è **ricevuta e trasmessa** attraverso **canali di ingresso/uscita**: visivo, uditivo, tattile, movimento → qui e in [[11 - Udito, Tatto e Movimento]];
- l'informazione è **conservata in memoria**: sensoriale, a breve termine, a lungo termine → [[12 - Memoria]];
- l'informazione è **elaborata e utilizzata**: ragionamento, problem solving, abilità acquisite, errori → [[13 - Pensiero, ragionamento e metafore]].

Inoltre:
- le capacità umane sono **influenzate dalle emozioni**;
- **ogni persona è diversa** dalle altre;
- nel mondo dei computer lo stesso sistema (e quindi la stessa interfaccia) può essere usato da utenti con **livelli di esperienza molto diversi**.

---

## La percezione visiva

Componenti base del sistema visivo umano:
- la **retina** → il **sensore**;
- il **cervello** → l'**elaboratore** dell'informazione.

Il comportamento del cervello nell'analisi visiva è materia degli **psicologi**; il funzionamento dell'occhio è materia dei **fisiologi**.

### Struttura dell'occhio umano

![[ium_p09_s06.png]]

- **Cornea**: parte trasparente attraversata dalla luce.
- **Iride**: parte "colorata" dell'occhio (regola la quantità di luce).
- **Pupilla**: foro al centro dell'iride.
- **Cristallino**: la lente che permette di **mettere a fuoco**.
- **Retina**: dove si focalizza la luce (contiene i fotorecettori).
- **Fovea**: parte centrale della retina, dove si concentrano i **coni** (visione più nitida).
- **Punto cieco**: "ingresso" del **nervo ottico**, privo di fotorecettori.

### Percezione di un oggetto
Dipende:
- dalle caratteristiche dell'**oggetto** stesso;
- dalle caratteristiche della **luce** che lo illumina;
- dal **colore dello sfondo**;
- dalla **persona** che lo guarda.

*Esempi*: un oggetto blu illuminato con luce rossa appare **nero** (non riflette rosso); uno sfondo scuro fa apparire più chiaro l'oggetto in primo piano, e viceversa.

### Coni e bastoncelli

L'occhio converte la luce in **segnali elettrici** da inviare al cervello. Il sensore è costituito da due tipi di cellule, chiamate così per la loro forma: **coni** e **bastoncelli**.

![[ium_p09_s09.png]]

| | **Bastoncelli** | **Coni** |
| --- | --- | --- |
| Sensibili a | **variazioni di luminosità** | **lunghezza d'onda** → **colore** |
| Posizione | **bordo esterno** della retina (periferia) | concentrati nella **fovea** |
| Sensibilità | **altissima**, si saturano facilmente → ci fanno vedere **al buio** | servono più luce |
| Risoluzione | **bassa** | alta |
| Ruolo | percezione del **movimento**, **visione periferica** | dettagli e colori |

I coni contengono **fotopigmenti** la cui attivazione dipende dalla frequenza della luce. Ci sono tre tipi di coni:
- **Blu** – onde corte (proporzione *x*)
- **Verde** – onde medie (*2x*)
- **Rossi** – onde lunghe (*4x*)

Conseguenze di design:
- il **rosso attira l'attenzione** dell'utente;
- il **blu è buono per gli sfondi** (abbiamo pochi coni blu, e quasi nessuno nella fovea → è poco adatto per testi e dettagli fini).

### Dalla luce all'immagine
- La luce si riflette dagli oggetti e viene raccolta dall'occhio.
- Le immagini sono messe a fuoco, **invertite**, sulla retina.
- È il **cervello** che individua elementi caratteristici (**pattern**) e il **movimento**.
- **Dimensione e profondità**:
  - l'**angolo visuale** indica quanta parte del campo visivo occupa l'oggetto (dipende da dimensione e distanza);
  - l'**acutezza visiva** è la capacità di percepire i dettagli;
  - gli oggetti **familiari** sono percepiti sempre della stessa dimensione, indipendentemente dalla distanza (**costanza della dimensione**);
  - indizi come la **sovrapposizione** aiutano a capire dimensione e distanza.

> [!example] Esperimento del punto cieco
> 1. Scrivere su un foglio bianco una **C** e una **L** a circa 10 cm di distanza.
> 2. Tenere il foglio a distanza di lettura, la C davanti all'occhio sinistro e la L davanti al destro.
> 3. Chiudere l'occhio sinistro e coprirlo con la mano.
> 4. Con l'occhio destro fissare la **C**.
> 5. Avvicinare o allontanare il viso finché la **L sparisce**: la sua immagine cade sul punto cieco. Il cervello "riempie" il buco con lo sfondo bianco circostante, per questo normalmente non ce ne accorgiamo.

---

## Opponenza cromatica

![[ium_p09_s14.png]]

> [!example] L'esperimento della bandiera
> Si fissa per circa 30 secondi il puntino al centro della bandiera (giallo, nero e verde), poi si passa a una slide **bianca**: si vede la bandiera americana **con i colori giusti** (blu, bianco e rosso) per qualche istante (**immagine residua** o *after-image*).

- Il sistema visivo si basa sulla distinzione di **tre coppie di colori opposti**:
  - **Rosso – Verde**
  - **Blu – Giallo**
  - **Nero – Bianco**
- L'effetto della bandiera si spiega con una **sovrastimolazione** di uno degli opposti (giallo → i recettori si "stancano"): quando lo stimolo finisce all'improvviso viene **compensato** e si percepisce l'opposto (giallo → blu, verde → rosso, nero → bianco).
- Questi **sei colori** sono molto riconoscibili tra loro.

## Effetto del contrasto

Lo **stesso colore** viene percepito diversamente a seconda dello **sfondo** su cui è posizionato.

![[ium_p09_s17.png]]
![[ium_p09_s18.png]]

*I quattro quadrati grigi centrali sono identici, ma su sfondo nero sembrano più chiari e su sfondo chiaro più scuri.*

> [!warning] Fare attenzione se il colore che usi **veicola informazione**: la stessa tonalità può essere letta in modo diverso in punti diversi dell'interfaccia.

## Scegliere i colori

![[ium_p09_s19.jpg]]

- Colori **adiacenti** sulla ruota si dicono **analoghi** → utili per creare **schemi di colore** armoniosi.
- Colori **opposti** si dicono **complementari** → distinzione netta, ma attenzione al loro accostamento (vedi cromostereopsi).
- **Evitare di usarne troppi** (*color pollution*).

![[ium_p09_s20.jpg]]

*Esempio di color pollution: un sito con decine di colori e immagini in competizione, in cui nulla emerge.*

### Cromostereopsi

![[ium_p09_s21.png]]

Impressione di **profondità** (e di "vibrazione") dovuta alla diversa **rifrazione** della luce tra colori: il cristallino mette a fuoco lunghezze d'onda diverse a distanze leggermente diverse, così un colore sembra "davanti" all'altro. Coppie critiche: **rosso–blu**, **rosso–verde**. Da evitare per testo su sfondo.

### Usare strumenti per scegliere i colori di una UI

![[ium_p09_s22.jpg]]

Es. **Figma Color Wheel**: https://www.figma.com/color-wheel/ — genera palette complementari, analoghe, triadiche, monocromatiche…

> [!tip] Approfondimento – Altri accorgimenti sul colore
> - Circa l'**8% degli uomini** (0,5% delle donne) ha una forma di **daltonismo**, soprattutto rosso–verde: non affidare un'informazione **solo** al colore (aggiungi icone, testo, forme).
> - Le linee guida di accessibilità (WCAG) richiedono un **contrasto** minimo testo/sfondo di **4.5:1** per il testo normale.

---

## Reperimento visivo (*visual search*)

- **Endogeno**: analisi **volontaria** degli oggetti per trovare quello che cerchiamo.
- **Esogeno**: attrazione **involontaria** dell'attenzione (es. qualcosa che **lampeggia**).
- L'attenzione visiva è **selettiva**: analizza solo una parte di ciò che vediamo.
- Canali per **attirare l'attenzione**: forma geometrica, **colore**, spessore delle linee, orientamento delle linee, curvatura, ombreggiatura.

![[ium_p09_s24.png]]

- **Sinistra**: reperimento **semplice** (l'elemento diverso differisce per **colore** → salta subito all'occhio, *pop-out*).
- **Centro**: un po' più difficile (differisce per **forma**).
- **Destra**: ancora più difficile (**forma e colore** combinati → bisogna esaminare gli elementi uno per uno).

> [!tip] Approfondimento – Elaborazione pre-attentiva
> Le caratteristiche come colore, orientamento o dimensione vengono elaborate **in parallelo** e in meno di 200–250 ms (*pre-attentive processing*): il tempo di ricerca non dipende dal numero di distrattori. Quando invece il target è definito da una **congiunzione** di caratteristiche (forma **e** colore), la ricerca diventa **seriale** e il tempo cresce con il numero di elementi (*Feature Integration Theory*, Treisman). Per evidenziare un elemento in una UI conviene quindi farlo differire per **una sola** caratteristica forte.

---

## Interpretazione del segnale: indizi di profondità

![[ium_p09_s25.jpg]]
![[ium_p09_s25b.jpg]]

| Monoculari (bastano un occhio) | Binoculari (servono due occhi) |
| --- | --- |
| Prospettiva lineare, prospettiva aerea, occlusione, tessitura (texture), ombre, dimensione, parallasse (di movimento) | Disparità binoculare, convergenza |

### Indizi monoculari

**Occlusione**: un oggetto che copre parzialmente un altro è più vicino.
**Altezza relativa**: gli oggetti più in alto nel campo visivo sono più lontani.

![[ium_p09_s26.jpg]]

**Grandezza relativa**: a parità di dimensione reale, l'oggetto più vicino occupa più campo visivo.
**Convergenza prospettica**: le linee parallele convergono in un **punto di fuga**.
**Familiarità della dimensione**: conoscere la dimensione reale di un oggetto dà informazioni sulla distanza.

![[ium_p09_s27.jpg]]

**Prospettiva aerea / percezione dell'atmosfera**: gli oggetti distanti si vedono meno nitidi e assumono una **tinta bluastra**.

![[ium_p09_s28.jpg]]

**Texture (gradiente di tessitura)**: elementi spaziati regolarmente appaiono **più fitti** man mano che la distanza aumenta.

![[ium_p09_s29.jpg]]

**Ombre**: l'ombra fa sembrare un oggetto staccato dallo sfondo (usate nelle UI per indicare elevazione, es. Material Design).

![[ium_p09_s30.png]]

### Indizi prodotti dal movimento

**Parallasse di movimento**: muovendoci, gli oggetti **vicini** escono subito dal campo visivo, quelli **lontani** sembrano muoversi più lentamente (usata nei videogiochi a scorrimento e nei siti *parallax*).

![[ium_p09_s31.gif]]

### Indizi binoculari: disparità

![[ium_p09_s32.png]]

- Nella **visione stereoscopica** i due occhi ricevono **due immagini diverse**, per il diverso punto di vista.
- Le differenze sono interpretate come **indizi di profondità**.
- La **fusione binoculare** è responsabile della percezione di **una sola immagine** della scena.
- Circa il **10%** della popolazione è **stereo-blind**, ma sfrutta gli altri indizi di profondità.

![[ium_p09_s33.png]]

> [!tip] Approfondimento – Convergenza
> L'altro indizio binoculare: per fissare un oggetto vicino gli occhi ruotano verso l'interno ("convergono"); il cervello usa lo sforzo dei muscoli oculari come informazione sulla distanza. Funziona solo per oggetti entro pochi metri. È rilevante per la **realtà virtuale** e gli schermi 3D.

---

## Compensazione e illusioni ottiche

Il sistema visivo è in grado di **compensare**:
- i **movimenti** degli oggetti che guardiamo;
- le **modifiche di luminanza**.

Per risolvere le **ambiguità** di interpretazione si usano le **informazioni di contesto**.
Quando c'è una **sovra-compensazione** si ottengono le **illusioni ottiche**.

### Illusione di Müller-Lyer
Oggetti della stessa dimensione vengono percepiti in modo diverso **a seconda del contesto**.

![[ium_p09_s35.png]]
![[ium_p09_s36.png]]

*Le due linee orizzontali sono lunghe uguali (vedi le guide rosse), ma quella con le "pinne" verso l'esterno sembra più lunga.*

![[ium_p09_s37.png]]

*Una spiegazione: interpretiamo le figure come angoli 3D. Le pinne verso l'esterno ricordano l'angolo interno di una stanza (più lontano), quelle verso l'interno lo spigolo esterno di un edificio (più vicino). Il cervello "compensa" la distanza presunta e ingrandisce la linea che crede più lontana.*

### Illusione di Ponzo

![[ium_p09_s38.png]]
![[ium_p09_s39.png]]

*Le due barre rosse sono identiche, ma inserite tra due binari che convergono quella in alto sembra più lunga: la prospettiva ci fa credere che sia più lontana, quindi il cervello la "ingrandisce".*

### La stanza di Ames (*Ames room*)

![[ium_p09_s40.png]]

La stanza sembra rettangolare vista dallo spioncino, ma in realtà è **trapezoidale**: un angolo è molto più lontano dell'altro. Poiché assumiamo che la stanza sia normale, interpretiamo le persone come di **dimensioni diverse** invece che a distanze diverse.

### Contorni illusori
Oggetti che **non esistono** possono essere immaginati sulla base di indizi visivi.

![[ium_p09_s41.png]]

*Triangolo di Kanizsa: vediamo un triangolo bianco sopra tre cerchi neri, anche se nessun triangolo è disegnato.*

### Illusione del muro del caffè (*Café wall*)

![[ium_p09_s42.png]]
![[ium_p09_s43.png]]

*Le linee grigie orizzontali (prima immagine) sono perfettamente parallele; quando vi si sovrappongono le file di quadrati bianchi e neri sfalsati, sembrano inclinate a cuneo.*

> [!tip] Rilevanza per il design
> Le illusioni mostrano che **percezione ≠ realtà fisica**: allineamenti, dimensioni e colori nelle interfacce vanno spesso corretti "otticamente" (es. un cerchio va disegnato un po' più grande di un quadrato per sembrare della stessa dimensione; un'icona "play" va spostata leggermente a destra per sembrare centrata).

## Riferimenti
- Gamberini, Chittaro, Paternò – *Human-Computer Interaction*, Pearson 2012, cap. 3: *Percezione visiva e design di interfacce grafiche*
