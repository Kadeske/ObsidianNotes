# 07 - La componente umana e la percezione visiva

> Fonte: [[IUM/TEORIA/PDF/07- percezione visiva.pdf]]
> Libro di riferimento: Gamberini, Chittaro, Paternò — *Human-Computer Interaction*, cap. 3

---

## 1. L'essere umano come sistema

Per progettare interfacce si usa un **modello semplificato** di come "funziona" una persona, concentrandosi sugli aspetti utili:

| Aspetto | Componenti |
|---|---|
| **Canali di ingresso/uscita** | Visivo, uditivo, tattile, movimento |
| **Memoria** | Sensoriale, a breve termine, a lungo termine |
| **Elaborazione** | Ragionamento, problem solving, abilità acquisite, errori |

Inoltre:
- le capacità umane sono **influenzate dalle emozioni**;
- **ogni persona è diversa**: la stessa interfaccia può essere usata da utenti con esperienza molto diversa.

## 2. La percezione visiva

Il sistema visivo ha due componenti: la **retina** (il *sensore*, studiata dai **fisiologi**) e il **cervello** (l'*elaboratore* dell'informazione, studiato dagli **psicologi**).

### Struttura dell'occhio

![[IUM07-occhio.png|420]]

| Parte | Funzione |
|---|---|
| **Cornea** | Parte trasparente attraversata dalla luce |
| **Iride** | Parte "colorata" dell'occhio |
| **Pupilla** | Foro al centro dell'iride |
| **Cristallino** | Lente che mette a fuoco le immagini |
| **Retina** | Superficie su cui si focalizza la luce |
| **Fovea** | Parte centrale della retina, dove si concentrano i **coni** |
| **Punto cieco** | Punto d'"ingresso" del nervo ottico, privo di recettori |

### Coni e bastoncelli

L'occhio converte la luce in **segnali elettrici** per il cervello tramite due tipi di fotorecettori, chiamati così per la loro forma:

![[IUM07-coni-bastoncelli.png|320]]

| | **Bastoncelli** | **Coni** |
|---|---|---|
| Sensibili a | **Variazioni di luminosità** | **Lunghezza d'onda** (colore) |
| Posizione | Bordo esterno della retina | Fovea |
| Sensibilità | Altissima: visione al buio, si saturano facilmente | Minore |
| Risoluzione | **Bassa** | Alta |
| Specialità | **Movimento**, visione **periferica** | Percezione del colore e dei dettagli |

I coni contengono **fotopigmenti** che si attivano in base alla frequenza della luce. Ne esistono tre tipi, in proporzioni diverse:
- **blu** — onde corte (×1);
- **verde** — onde medie (×2);
- **rosso** — onde lunghe (×4).

> [!tip] Conseguenze per il design
> Il **rosso attira l'attenzione**; il **blu** è adatto agli **sfondi**.

### Come si forma l'immagine
- La luce riflessa dagli oggetti viene messa a fuoco, **capovolta**, sulla retina.
- È il **cervello** a riconoscere pattern e movimento.
- **Angolo visivo**: quanta parte del campo visivo occupa un oggetto (dipende da dimensione e distanza).
- **Acuità visiva**: capacità di percepire i dettagli.
- **Costanza di dimensione**: gli oggetti familiari sembrano sempre della stessa grandezza, a qualunque distanza.
- Indizi come la **sovrapposizione** aiutano a stimare dimensione e distanza.

La percezione di un oggetto dipende dalle **caratteristiche dell'oggetto**, dalla **luce** che lo illumina, dal **colore dello sfondo** e dalla **persona** che lo guarda. Esempi: un oggetto blu illuminato da luce rossa appare **nero**; uno sfondo scuro fa sembrare più chiaro l'oggetto in primo piano (e viceversa).

> [!example] Esperimento del punto cieco
> 1. Scrivere **C** e **L** su un foglio, a circa 10 cm di distanza.
> 2. Tenere il foglio alla normale distanza di lettura, con la C davanti all'occhio sinistro e la L davanti al destro.
> 3. Chiudere e coprire l'occhio sinistro; con il destro fissare la **C**.
> 4. Avvicinarsi o allontanarsi dal foglio finché la **L sparisce**: la sua immagine cade sul punto cieco.

## 3. Il colore

### Opponenza cromatica
La visione umana si basa su **tre coppie di colori opposti**: **rosso–verde**, **blu–giallo**, **nero–bianco**. Questi sei colori sono molto riconoscibili tra loro.

> [!example] La bandiera "sbagliata"
> Fissando a lungo una bandiera USA con colori invertiti (verde, nero e giallo) e poi guardando una superficie bianca, si vede la bandiera **con i colori giusti**: la sovrastimolazione di un colore viene **compensata** con il suo opposto quando lo stimolo cessa all'improvviso (*immagine residua*).
>
> ![[IUM07-bandiera-opponenza.png|250]]

### Effetto del contrasto
Lo **stesso colore** viene percepito diversamente a seconda dello **sfondo**: i quattro quadrati centrali sono identici.

![[IUM07-contrasto.png|300]]

> [!warning] Attenzione se il colore che usate **veicola informazione**!

### Scegliere i colori

![[IUM07-ruota-colori.png|380]]

- **Colori analoghi**: vicini sulla ruota dei colori → utili per creare **schemi di colore** armoniosi.
- **Colori complementari**: opposti sulla ruota → distinzione netta, ma **attenzione ad accostarli**.
- **Evitare di usarne troppi** (*color pollution*).
- Usare strumenti dedicati, es. **Figma Color Wheel**: <https://www.figma.com/color-wheel/>

### Cromostereopsi
Impressione di **profondità** dovuta alla diversa **rifrazione** della luce tra colori (rosso–blu, rosso–verde): accostati, sembrano stare su piani diversi e affaticano la vista.

![[IUM07-cromostereopsi.png|250]]

## 4. Reperimento visivo

- **Endogeno**: analisi **volontaria** degli oggetti per trovare ciò che cerchiamo.
- **Esogeno**: attrazione **involontaria** dell'attenzione (es. un elemento lampeggiante).
- L'attenzione visiva è **selettiva**: analizza solo una parte di ciò che vediamo.
- Canali per attirare l'attenzione: **forma**, **colore**, **spessore** e **orientamento** delle linee, **curvatura**, **ombreggiatura**.

![[IUM07-reperimento-visivo.png|500]]

Trovare l'elemento diverso è facile se differisce per **colore** (sinistra), più difficile se differisce per **forma** (centro), ancora più difficile se bisogna combinare **forma e colore** (destra).

## 5. Interpretazione del segnale: la profondità

| Indizi **monoculari** (bastano un occhio) | Indizi **binoculari** |
|---|---|
| Occlusione · altezza relativa · grandezza relativa · convergenza prospettica (prospettiva lineare) · familiarità della dimensione · prospettiva aerea · texture · ombre · parallasse di movimento | Disparità binoculare · convergenza |

### Indizi monoculari

| Indizio | Descrizione |
|---|---|
| **Occlusione** | Un oggetto che ne copre parzialmente un altro è più vicino |
| **Altezza relativa** | Gli oggetti più in alto nel campo visivo sono più lontani |
| **Grandezza relativa** | A parità di dimensione reale, l'oggetto più vicino occupa più campo visivo |
| **Convergenza prospettica** | Le linee parallele convergono verso un punto di fuga |
| **Familiarità della dimensione** | Conoscere la grandezza reale di un oggetto dà informazioni sulla sua distanza |
| **Prospettiva aerea** (atmosfera) | Gli oggetti lontani appaiono meno nitidi e bluastri |
| **Texture** | Elementi regolarmente spaziati appaiono più fitti con la distanza |
| **Ombre** | Indicano la posizione relativa degli oggetti rispetto alle superfici |
| **Parallasse di movimento** | Muovendosi, gli oggetti vicini escono subito dal campo visivo, quelli lontani sembrano muoversi più lentamente (es. lo scorrimento a più livelli dei videogiochi a piattaforme) |

![[IUM07-occlusione.jpg|260]] ![[IUM07-grandezza-relativa.jpg|260]]
![[IUM07-prospettiva-aerea.jpg|260]] ![[IUM07-ombre.png|260]]

### Indizi binoculari: visione stereoscopica
- I due occhi vedono la scena da punti di vista diversi: sulle due retine si formano **due immagini diverse** (**disparità binoculare**).
- Il cervello interpreta le differenze come **profondità**; la **fusione binoculare** fa percepire un'unica immagine.
- Circa il **10%** della popolazione è *stereo-blind* e si affida agli altri indizi.

![[IUM07-disparita-binoculare.jpg|320]]

## 6. Compensazione e illusioni ottiche

Il sistema visivo **compensa** i movimenti degli oggetti osservati e le variazioni di luminanza, e usa il **contesto** per risolvere le ambiguità della scena. Quando compensa troppo (**sovra-compensazione**) nascono le **illusioni ottiche**.

| Illusione | Cosa mostra |
|---|---|
| **Müller-Lyer** | Due segmenti uguali sembrano di lunghezza diversa a seconda delle "frecce" alle estremità (vale anche per gli spigoli di una stanza in prospettiva) |
| **Ponzo** | Due barre uguali sembrano diverse se poste tra linee convergenti (binari): quella "più lontana" appare più lunga |
| **Stanza di Ames** | Una stanza deformata, vista da un punto preciso, sembra regolare: due persone di uguale altezza appaiono una gigante e una minuscola |
| **Triangolo di Kanizsa** | Si "vede" un triangolo bianco che non esiste, ricostruito dagli indizi visivi |
| **Linee parallele / café wall** | Linee perfettamente parallele sembrano inclinate per effetto del pattern a scacchi |

![[IUM07-muller-lyer.png|260]] ![[IUM07-ponzo.png|260]]
![[IUM07-ames.png|260]] ![[IUM07-kanizsa.png|220]]
![[IUM07-cafe-wall.png|420]]

> **In sintesi**: oggetti della stessa dimensione sono percepiti diversamente a seconda del **contesto**, e oggetti inesistenti possono essere **immaginati** a partire da indizi visivi.

---

## Termini e concetti da ricordare

- **Modello dell'essere umano**: canali I/O (vista, udito, tatto, movimento) · memoria (sensoriale, breve, lungo termine) · elaborazione.
- **Occhio**: cornea, iride, pupilla, cristallino, retina, **fovea** (coni), **punto cieco** (nervo ottico).
- **Bastoncelli**: luminosità, buio, movimento, visione periferica, bassa risoluzione.
- **Coni**: colore; blu (×1), verde (×2), rosso (×4); **rosso attira l'attenzione, blu per gli sfondi**.
- **Angolo visivo**, **acuità visiva**, **costanza di dimensione**.
- **Opponenza cromatica**: rosso–verde, blu–giallo, nero–bianco; immagine residua.
- **Effetto del contrasto**: lo stesso colore cambia aspetto con lo sfondo.
- **Colori analoghi** (schemi armonici) vs **complementari** (contrasto netto); evitare la **color pollution**.
- **Cromostereopsi**: rosso–blu / rosso–verde creano falsa profondità.
- **Reperimento visivo** endogeno vs esogeno; attenzione **selettiva**; il colore è il canale più rapido.
- **Indizi monoculari** (occlusione, altezza/grandezza relativa, prospettiva lineare e aerea, texture, ombre, parallasse) e **binoculari** (disparità, convergenza); **fusione binoculare**; ~10% *stereo-blind*.
- **Illusioni ottiche** = sovra-compensazione: Müller-Lyer, Ponzo, stanza di Ames, Kanizsa, café wall.
