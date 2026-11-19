---
corso: Interazione Uomo Macchina
parziale: 1
argomento: Udito, Tatto e Movimento
tags: [IUM, teoria, parziale1, componente-umana, udito, tatto, fitts]
---

# 11 - Udito, Tatto e Movimento

## Udito

### Caratteri generali
- Capacità di percepire **suoni**: vibrazioni di un corpo in oscillazione immerso in un mezzo elastico (l'aria).
- Fornisce informazioni sull'**ambiente**: distanze, direzioni, oggetti…
- Il soggetto della percezione **uditiva** sono principalmente le **sorgenti primarie** → gli oggetti che **emettono** il suono.
- Il soggetto della percezione **visiva** sono le **sorgenti secondarie** → gli oggetti che **riflettono** la luce.
- L'udito è un canale di input che **non richiede attenzione pregressa** (non possiamo "chiudere le orecchie", e sentiamo anche ciò che non guardiamo) → da sfruttare nelle interfacce, es. **segnali di allarme**, notifiche.

### L'orecchio umano

![[ium_p11_s03.png]]

- **Orecchio esterno** (padiglione e canale uditivo): protegge l'orecchio interno e **amplifica** il suono.
- **Orecchio medio** (timpano e catena degli ossicini): **trasmette** le onde sonore come **vibrazioni** verso l'interno.
- **Orecchio interno** (coclea): le vibrazioni provocano il rilascio di **trasmettitori chimici** che generano impulsi nel **nervo uditivo**.
- L'orecchio interno percepisce anche le **accelerazioni** tramite l'**apparato vestibolare** (canali semicircolari) → senso dell'equilibrio.
  - **Chinetosi** (*motion sickness*): malessere che nasce quando vista e apparato vestibolare danno informazioni contrastanti (es. leggere in auto, o la realtà virtuale: vedo movimento ma il corpo è fermo).

### Caratteristiche del suono

![[ium_p11_s04.png]]

- **Altezza** (*pitch*): la **frequenza** del suono. **Grave** = bassa frequenza; **acuto** = alta frequenza.
- **Intensità** (*loudness*): il livello del "volume" (ampiezza dell'onda). Si possono avere frequenze uguali con volumi diversi.
- **Timbro**: legato al tipo di strumento (o fonte) che produce il suono; è ciò che distingue la stessa nota suonata da un pianoforte o da un violino.

### Caratteristiche dell'udito umano
- Sentiamo frequenze da **20 Hz a 15 kHz** (fino a ~20 kHz nei giovani; il limite superiore cala con l'età).
- Distinguiamo **meno accuratamente le alte frequenze** rispetto alle basse.
- Il sistema uditivo **filtra** i suoni:
  - il **rumore di fondo** viene eliminato, permettendoci di udire suoni familiari senza prestare attenzione;
  - esempio: **cocktail party effect** — in una stanza affollata riusciamo a seguire una sola conversazione, e ci accorgiamo subito se qualcuno pronuncia il nostro nome;
  - ma quando sentiamo un suono **"strano"**, questo **non viene filtrato** (attira l'attenzione).

> [!tip] Approfondimento – Il suono nelle interfacce
> - **Earcons**: suoni astratti/musicali associati a eventi (es. il "ding" di una notifica).
> - **Auditory icons**: suoni che imitano il mondo reale (es. il rumore della carta accartocciata quando si svuota il cestino).
> - Il suono è utile quando gli occhi sono occupati (guida), per eventi urgenti o per utenti non vedenti, ma va usato con parsimonia: è **invasivo** e pubblico.

---

## Tatto

- Fornisce importanti **feedback sull'ambiente** (temperatura, peso…), fondamentali per **coordinare i movimenti**.
- **Feedback aptico = tatto + cinestesia**.
- Senso **non localizzato**, ma **diffuso** su tutto il corpo:
  - alcune **aree** sono più sensibili di altre (es. polpastrelli, labbra);
  - la sensibilità dipende dal loro **uso**.
- Gli stimoli tattili sono ricevuti da **recettori nervosi della pelle**:
  - **Termocettori**: sensibili alla **temperatura**;
  - **Nocicettori** (nelle slide "chemiocettori"): sensibili a **pressione, calore e dolore intensi**;
  - **Meccanocettori**: sensibili alla **pressione**.

- Può essere un senso **fondamentale per chi ha disabilità visive** (es. Braille, feedback vibrotattile).
- **Cinestesia**: consapevolezza della **posizione** (e del moto) del proprio corpo e degli arti.
  - Recettori nei **muscoli** e nei **tendini**.
  - Fornisce feedback al sistema motorio.
- **Sistema propriocettivo** (feedback):
  - utilissimo nella vita di tutti i giorni (es. toccarsi il naso a occhi chiusi, digitare senza guardare la tastiera);
  - **assente in alcuni ambienti di simulazione** (es. in VR "toccare" un oggetto virtuale non dà resistenza);
  - ci aiuta anche in assenza della vista.

> [!example] Esempi di feedback aptico nelle UI
> La vibrazione del telefono, il "click" simulato del trackpad Apple (Taptic Engine), i controller dei videogiochi con *force feedback*, i tasti fisici con corsa e scatto (il touch screen invece non dà feedback tattile → serve compensare con feedback visivo/sonoro).

---

## Movimento

- Tempo per rispondere a uno stimolo = **tempo di reazione + tempo di movimento**.
- Il **tempo di movimento** dipende da età, forma fisica, allenamento…
- Il **tempo di reazione** dipende dal **tipo di stimolo**:
  - visivo: **~200 ms**
  - uditivo: **~150 ms**
  - dolore: **~700 ms**
- Tempi di risposta crescenti (cioè chiedere di andare più veloci) **diminuiscono l'accuratezza** in un utente **inesperto**, ma **non** in un utente **addestrato** (*speed–accuracy trade-off*).
- Ci concentriamo sul movimento per **puntare un oggetto su schermo**.

### La legge di Fitts

Descrive il **tempo impiegato per raggiungere un obiettivo** su schermo, in funzione di **velocità e precisione**:

$$
Mt = a + b \log_2\left(\frac{D}{W} + 1\right)
$$

- $a$ e $b$: costanti **determinate empiricamente** (dipendono dal dispositivo e dall'utente);
- $Mt$: **tempo di movimento**;
- $D$: **distanza** da coprire;
- $W$: **dimensione** (larghezza) dell'obiettivo.

> [!tip] Approfondimento – Indice di difficoltà
> Il termine $ID = \log_2(D/W + 1)$ si chiama **indice di difficoltà** e si misura in **bit**. Raddoppiando la distanza o dimezzando la dimensione l'ID cresce di circa 1 bit. $1/b$ è il **throughput** (bit/s) del dispositivo: permette di confrontare mouse, touchpad, joystick…
>
> *Esempio numerico*: con $a = 50$ ms, $b = 150$ ms/bit, un bottone largo 20 px a 300 px di distanza: $ID = \log_2(300/20+1) = \log_2 16 = 4$ bit → $Mt = 50 + 150·4 = 650$ ms. Raddoppiando la larghezza a 40 px: $ID = \log_2(8.5) ≈ 3.09$ → $Mt ≈ 513$ ms.

#### Conseguenze
![[ium_p11_s12.png]]

Per **diminuire il tempo di movimento**:
- **aumentare la dimensione** dell'obiettivo;
- **diminuire la distanza**.

Quindi:
- **ingrandire i controlli usati più spesso**;
- **concentrarli** in una zona facilmente accessibile.

#### Attenzione: la legge è logaritmica!
![[ium_p11_s13.png]]

- Se aumenti del 20% un oggetto **piccolo**, il tempo di movimento diminuisce **sensibilmente**.
- Se ingrandisci del 20% un oggetto **già grande**, il tempo **non** diminuisce sensibilmente.

#### Fasi del movimento
![[ium_p11_s14.png]]

1. **Prima fase**: movimento **rapido** (balistico) e **impreciso** verso l'obiettivo.
2. **Seconda fase**: la velocità **cala** per la ricerca della **precisione** (piccole correzioni finali). È questa fase che dipende da $W$.

#### Gli angoli attivi
![[ium_p11_s15.png]]

- Il puntatore **non può uscire** dai bordi dello schermo.
- Quindi bordi e soprattutto **angoli** possono essere considerati obiettivi di **dimensione infinita**: basta "lanciare" il mouse in quella direzione.

#### Esempi

![[ium_p11_s16.png]]

- *Sinistra*: il multitasking delle vecchie versioni di iOS (4–6): per chiudere un'app bisognava toccare il minuscolo bottone rosso (−) nell'angolo dell'icona → obiettivo **piccolo**, lento e soggetto a errori.
- *Destra*: il multitasking di iOS 7: ogni app è un'**anteprima grande** che si chiude trascinandola verso l'alto → obiettivo molto più grande, tempo di movimento minore.

> [!tip] Altri esempi classici
> - La **barra dei menu di macOS** è sempre attaccata al bordo superiore dello schermo (altezza "infinita"), mentre in Windows i menu sono dentro la finestra → su Mac si raggiungono più velocemente.
> - Il pulsante **Start** di Windows e gli **hot corners** sono negli angoli.
> - I **menu contestuali** (tasto destro) e i **pie menu** compaiono dove si trova già il puntatore → $D$ minima.

## Riferimenti
- Dix, Finlay, Abowd, Beale – *Interazione uomo-macchina*, cap. 1 (sezioni Udito, Tatto e Movimento)
- http://particletree.com/features/visualizing-fittss-law/
- http://sixrevisions.com/usabilityaccessibility/improving-usability-with-fitts-law/
