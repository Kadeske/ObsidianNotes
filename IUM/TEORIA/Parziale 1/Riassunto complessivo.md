
## corso: Interazione Uomo Macchina parziale: 1 tipo: appunti unici per l'esame tags: [IUM, teoria, parziale1, ripasso]

# IUM – Primo Parziale: appunti completi

> [!abstract] Come usare questo file Tutti gli argomenti del primo parziale (fino alle **metafore incluse**) in un solo file, organizzati per rispondere al tipo di domande della simulazione:
> 
> - **Vero/Falso** → ogni sezione finisce con un riquadro **⚠️ Trappole V/F**: sono le affermazioni "quasi giuste" che l'esame usa per ingannare.
> - **Domande aperte** → la [[#Parte C – Modelli per le domande aperte]] contiene schemi pronti per: criteri di leggibilità, persona, scenario, modelli della LTM, analogia, problem space, Fitts, metafore, prototipi.
> - **Autoverifica** → la [[#Parte D – Tutte le domande della simulazione]] le riporta tutte con la spiegazione; la [[#Parte E – Altre domande V F simili (autoverifica)]] ne aggiunge altre con la risposta nascosta (clicca per aprirla).
> 
> Le immagini (`![[ium_pXX_sYY]]`) sono quelle della cartella `img/` del tuo vault: Obsidian le trova da sola se il file sta nello stesso vault.

## Indice

- **Parte A – HCI e processo di design**
    1. [[#1. Perché studiare HCI]]
    2. [[#2. Definizioni e terminologia]]
    3. [[#3. Interaction Design e processo]]
    4. [[#4. Requisiti]]
    5. [[#5. Conoscere gli utenti e Personas]]
    6. [[#6. Scenari]]
    7. [[#7. Prototipi]]
    8. [[#8. Architettura dell'informazione]]
- **Parte B – La componente umana** 9. [[#9. Percezione visiva]] 10. [[#10. Il processo di lettura]] 11. [[#11. Udito, tatto e movimento (legge di Fitts)]] 12. [[#12. Memoria]] 13. [[#13. Ragionamento]] 14. [[#14. Problem solving]] 15. [[#15. Errori (sviste, lapsus, errori)]] 16. [[#16. Metafore]]
- [[#Parte C – Modelli per le domande aperte]]
- [[#Parte D – Tutte le domande della simulazione]]
- [[#Parte E – Altre domande V F simili (autoverifica)]]
- [[#Parte F – Numeri e formule da sapere]]

---

# Parte A – HCI e processo di design

## 1. Perché studiare HCI

**L'HCI determina come usiamo i prodotti digitali**: cosa possiamo fare, quanto è facile lavorarci, quanto in fretta si impara, quanto è sicuro un prodotto.

**L'usabilità ha valore economico.** Un prodotto facile da usare:

- aumenta la **produttività**, **riduce i costi** (supporto), aumenta **vendite/profitti**, la **lealtà** dei clienti e ne **attira di nuovi**;
- è considerato **segno di qualità** e dà **vantaggio competitivo**.

**Perché puntare su utente e interazione?** Tradizionalmente un prodotto si differenzia per **funzionalità e prezzo**. Se i concorrenti offrono funzioni e prezzi simili (es. app di messaggistica gratuite), conta: è **più facile da usare**? Fa **finire il compito più in fretta**? È **più piacevole**?

- Trend: tecnologie poco distinguibili tra loro, molta potenza disponibile, nuovi input/output, **nessuno vuole imparare cose nuove**, **la maggior parte degli utenti non è interessata alla tecnologia in sé**, utenze enormi.
- Vecchia domanda: _cosa possono fare i computer?_ → Nuova: _**cosa posso fare io con i computer?**_ (Shneiderman, _Leonardo's Laptop_, 2002: la tecnologia deve potenziare le persone).

**Online shop**: chi non trova il prodotto, non capisce se è quello giusto o non riesce a pagare **non compra** → correlazione diretta vendite–usabilità. _"Bad usability is like a leaky pipe"_ (Brignull, 2006): ogni problema in un processo a più passi (funnel) è un buco da cui escono utenti; le perdite si moltiplicano.

**L'HCI è scienza, ingegneria e design**: si occupa di prestazione congiunta uomo-macchina, struttura della comunicazione, capacità umane (incl. **apprendibilità**), problemi ingegneristici, processo di specifica/progetto/implementazione, **trade-off**, algoritmi dell'interfaccia.

**HCI = innovazione visibile**: i problemi spesso **non sono percepiti finché non esiste una soluzione**; le buone soluzioni **sembrano ovvie** dopo (uovo di Colombo). Esempi: smartphone prima/dopo iPhone (tutti si sono allineati al touch capacitivo); menu dei font di Office che mostra ogni font **nel suo stile** (anteprima).

## 2. Definizioni e terminologia

**HCI (ACM SIGCHI, 1992)**: disciplina che riguarda **progettazione, valutazione e implementazione di sistemi interattivi** per uso umano e lo studio dei fenomeni che li circondano. Definizione pragmatica: abilitare l'interazione tra **uno o più umani**, **uno o più dispositivi** e i **dati**.

**Sistema interattivo**: consente di interagire **in tempo reale** con **feedback immediato** (tempo reale ≈ percepito come istantaneo, **< 100 ms**). Es.: GUI, smartphone, console, biglietterie automatiche, **anche la riga di comando**.

- Soglie di Nielsen: **0,1 s** istantaneo · **1 s** si nota il ritardo ma il flusso di pensiero regge · **10 s** limite dell'attenzione (serve progress bar).
- Le interazioni possono partire dall'utente o dal sistema (sistemi **proattivi**, es. notifiche).

**Interfaccia utente**: la parte del sistema con cui l'utente interagisce; può dare **input, output o entrambi**; **non è limitata al digitale** (cruscotto, cabina di un aereo, voce di Alexa).

**ISO 9241-210:2019**

|Termine|Definizione|
|---|---|
|**Utente**|persona che interagisce con il sistema, prodotto o servizio|
|**Contesto d'uso**|combinazione di **utenti, obiettivi e compiti, risorse e ambiente**|
|**Sistema interattivo**|combinazione di hardware e/o software e/o servizi e/o **persone** con cui gli utenti interagiscono per un obiettivo|
|**Interfaccia utente**|tutti i componenti (sw o hw) che **forniscono informazioni e permettono il controllo** per completare compiti|

**Usabilità (ISO 9241-11)**: grado in cui un prodotto può essere usato da **specifici utenti** per **specifici obiettivi** con **efficacia, efficienza e soddisfazione** in uno **specifico contesto d'uso**.

**UI design** = scelta delle **modalità** (grafica, vocale, tattile), degli **elementi** e del loro **posizionamento**. **Interaction Design (IxD)** = progettare prodotti interattivi per supportare le persone nella vita quotidiana e lavorativa (Preece, Rogers, Sharp); l'attenzione va oltre l'oggetto e **include come gli utenti interagiranno**. **UX Design** = più **olistico**: l'**esperienza complessiva** (aspettative, emozioni, prima e dopo l'uso). Inclusione: **UI ⊂ IxD ⊂ UX**.

**Utilità / Usabilità / Piacevolezza**

- **Utilità**: serve allo scopo. **Essenziale.**
- **Usabilità**: **qualità ed efficienza** con cui il prodotto supporta il compito.
- **Piacevolezza**: può essere slegata da utilità e usabilità (estetica, moda, status).

**Usabilità secondo Nielsen (2003)**: _attributo di qualità_ che misura quanto un'interfaccia è facile da usare; indica anche i _metodi_ per migliorarla. **5 componenti**:

|Componente|Domanda|
|---|---|
|**Apprendibilità**|quanto è facile la **prima volta**?|
|**Efficienza**|una volta imparato, **quanto sono veloce**?|
|**Memorabilità**|dopo un **periodo di inattività**, quanto recupero in fretta?|
|**Errori**|quanti, **quanto gravi**, quanto è facile **recuperare**?|
|**Soddisfazione**|quanto è **piacevole**?|

Esempio: software di montaggio video → bassa apprendibilità, alta efficienza; app di check-in usata 2 volte l'anno → servono apprendibilità e memorabilità.

**"Fuffa" da non usare** (nelle relazioni di progetto): _intuitive, user-friendly, clean, modern, simple, seamless, attractive…_ → vaghi e non misurabili. Sostituirli con criteri misurabili ("un nuovo utente si registra in < 2 minuti senza aiuto").

> [!warning] ⚠️ Trappole V/F
> 
> - La riga di comando **è** un sistema interattivo.
> - L'interfaccia **non** è solo digitale.
> - Secondo l'ISO il sistema interattivo può includere anche **persone** e servizi.
> - UX è **più ampio** di IxD, non il contrario.
> - Un prodotto può piacere senza essere usabile (piacevolezza ≠ usabilità).

## 3. Interaction Design e processo

È un **processo** guidato da un obiettivo, **creativo**, che richiede di **bilanciare trade-off**.

**Quattro approcci al design (Saffer)**

|Approccio|Cosa guida il design|
|---|---|
|**User-centered**|i **bisogni dell'utente**|
|**Activity-centered**|il **comportamento/le attività** necessarie al task|
|**System design**|progettazione **rigorosa** di un sistema per un problema preciso (componenti, obiettivi, feedback)|
|**Genius design**|**solo l'esperienza** del designer, senza utenti|

**Perché coinvolgere gli utenti**

- **Gestione delle aspettative**: aspettative **realistiche** (niente sorprese né delusioni), apprendimento tempestivo, comunicazione senza "vendere fumo".
- **Senso di proprietà (ownership)**: utenti parti attive → **capiscono e accettano i problemi** → il prodotto viene **accettato** più facilmente.
- **Grado di coinvolgimento**: basso (interviste/osservazione occasionali) · medio (workshop, test di prototipi) · alto (utenti nel team, **design partecipativo**). Più coinvolgimento = più informazioni e accettazione, ma più costi e rischio che diventino "troppo esperti".
- Gli utenti **non possono essere sostituiti sempre** da un esperto di interfacce: l'esperto non ha il contesto, i bisogni e le abitudini dell'utente reale.

**Approccio user-centered (Gould & Lewis)**

1. **Focus precoce su utenti e task**;
2. **Misurazione empirica** delle prestazioni;
3. **Design iterativo** (problema trovato → correggo → ritesto).

**Bisogni**: gli utenti **raramente sanno cosa è possibile** e **non sanno dirti cosa serve** ("cavalli più veloci", Ford). Si studiano i **task esistenti**: contesto, informazioni necessarie, chi collabora, **perché** si fa così; oppure si descrivono **scenari futuri**.

**Piramide di Maslow** (dal basso): fisiologici → sicurezza → amore/appartenenza → stima → autorealizzazione. (Instagram ≈ stima/appartenenza; Google Maps ≈ sicurezza; Tinder ≈ amore/appartenenza.)

**Ciclo Task–Artifact (Carroll, 1990)**: le attività esprimono bisogni → si creano artefatti → usandoli **nascono nuove possibilità e nuovi bisogni** → nuovi task… all'infinito. Es.: l'automobile ha cambiato la mobilità e la **struttura delle città**.

**Iterazioni**

- **Migliorare un approccio** (raffinare un'idea): rischio di fermarsi a un **massimo locale**.
- **Esplorare** più idee: trovare soluzioni globalmente migliori. Buxton: _getting the right design_ (esplorare) prima di _getting the design right_ (raffinare).
- **Elaborazione** (generare molte idee, pensiero divergente, _opportunity seeking_) e **riduzione** (selezionare, pensiero convergente, _decision making_) si alternano.

**Le 4 attività di base** (ciclo iterativo): **Requisiti → Progettare alternative → Prototipi → Valutazione** → (si torna indietro o prodotto finale). Il processo specifico conta poco: conta **cosa fai** dentro il processo. Design Thinking (Stanford): **Empathize – Define – Ideate – Prototype – Test**, non lineare.

![[ium_p04_s16.png]]

> [!warning] ⚠️ Trappole V/F
> 
> - Coinvolgere gli utenti **aiuta** a mantenere realistiche le aspettative → **Vero**.
> - Gli utenti **non** possono essere **sempre** sostituiti da un esperto → **Falso** se lo dice.
> - Gli utenti **non** sanno dire esattamente di cosa hanno bisogno.
> - Il genius design **non** coinvolge gli utenti.
> - Il design UCD è **iterativo**, non a cascata.

## 4. Requisiti

Nello UCD i requisiti sono **aspettative, necessità e vincoli degli utenti finali**. Servono a: orientare il design all'utente, **ridurre i rischi** (malintesi, sviluppi sbagliati), fare da **base** per le decisioni. È la fase in cui i progetti **falliscono più spesso** (vignetta dell'altalena: ogni passaggio di comunicazione distorce ciò che serviva davvero).

| Tradizionali | UCD                                      |                                      |
| ------------ | ---------------------------------------- | ------------------------------------ |
| Cosa         | specifiche tecniche/funzionali           | aspettative e bisogni degli utenti   |
| Focus        | sistema, performance, architettura       | esperienza, usabilità, soddisfazione |
| Esempi       | carico, tempi di risposta, compatibilità | facilità d'uso, accessibilità        |

I requisiti UCD **arricchiscono** quelli tradizionali e possono **evolvere** durante lo sviluppo.

**Tipi di requisiti**

- **Funzionali**: _cosa_ deve fare il sistema.
- **Non funzionali**: vincoli sul _come_ (memoria, tempi di risposta…).
- **Dati**: quali dati, come salvarli.
- **Ambiente / contesto d'uso**:
    - **fisico** (rumore, luce, polvere, caldo, vibrazioni);
    - **sociale** (condivisione, lavoro a distanza o individuale, privacy);
    - **organizzativo** (gerarchia, supporto, infrastruttura, formazione).
- (Preece) anche requisiti **utente** e di **usabilità** misurabili.

**Come si scrive**: **chiaro, non ambiguo, specifico → verificabile**.

- ✅ "Il download di qualsiasi pagina deve impiegare meno di 5 secondi."
- ❌ "Le bambine sotto i 10 anni devono trovare il sito attraente." (non misurabile)
- Campi aggiuntivi: **identificatore, tipo, chi l'ha generato, motivazione**.
- Esempi: ricerca con filtri (funzionale); registrazione < 2 min; max 3 clic per ogni pagina e feedback dopo ogni azione (usabilità); navigabile con screen reader, contrasto sufficiente (accessibilità); caricamento < 3 s, risposta < 200 ms (performance); layout responsive, modalità offline (contesto).

> [!warning] ⚠️ Trappole V/F
> 
> - I requisiti **forniscono** informazioni sul contesto d'uso (c'è un'intera categoria!) → "non forniscono alcuna informazione sul contesto" = **Falso**.
> - I requisiti **non** sono "approssimativi": devono essere **specifici e verificabili** → "informazione approssimativa" = **Falso**.
> - I requisiti UCD **possono cambiare** durante lo sviluppo.

## 5. Conoscere gli utenti e Personas

Un **vago concetto di "utente"** non aiuta a decidere: bisogna renderlo **concreto**.

**Caratteristiche**: abilità, conoscenze, dimestichezza col computer; uso **inesperto/esperto**, **casuale/frequente**.

|Utente|Ha bisogno di|
|---|---|
|Inesperto|istruzioni passo-passo, interazione vincolata, informazioni chiare|
|Esperto|flessibilità, accesso diretto, "potere"|
|Frequente|scorciatoie|
|Casuale|istruzioni chiare (menu)|

Differenze fisiche: dimensione delle mani (bottoni), abilità motorie, **altezza** (chioschi), **forza** (giocattolo facile da attivare, difficile da aprire per le batterie), disabilità.

**Tre strumenti collegati**: **Persona** (_chi_) · **Scenario** (_quando, dove, come_) · **Goal** (_cosa vuole ottenere_; raggiunto il goal lo scenario finisce).

### Persona

> "Descrizione **fittizia ma realistica** di un utente **tipico o target**. È un **archetipo**, non una persona reale, ma va descritta **come se fosse reale**." (NN/g)

- **Non sono persone reali**: caratteristiche **sintetizzate dai dati** degli utenti reali (anche da risultati statistici).
- **Non idealizzate**; devono essere **"vive"** e **concrete** (nome, foto, scopi, attività sociali, anche interessi **non legati all'app**: rendono il personaggio credibile).
- Hanno un **insieme unico di obiettivi** (non solo il ruolo lavorativo).
- Servono a **focalizzare le categorie di utenti** e a ragionare su istanze concrete invece che in astratto.
- Se ne creano **diverse**, **il prima possibile**, come risultato di **user research**, basandosi su **dati, non su desideri**.
- **Cosa inserire**: nome, età, genere, **foto**; **etichetta** (cosa fa nella vita); **livello di esperienza** nel dominio; **contesto d'uso** (lo sceglie o è obbligato? quanto spesso? su che dispositivo?); **obiettivi e problemi/frustrazioni** (velocità, accuratezza, completezza…); una **frase** che riassume il suo atteggiamento.
- ⚠️ Evitare **troppe battute**: diventa divertente ma inutile. Le personas generate da ChatGPT tendono a essere **plausibili ma generiche/stereotipate** (non basate su dati).

**Tecniche per raccogliere dati sugli utenti**

|Tecnica|Pro|Contro|
|---|---|---|
|Interviste|si usano prototipi/scenari, ottime per indagare problemi|lunghe, pochi utenti|
|Focus group|trovano consenso/conflitti|dominati da pochi|
|Questionari|dati qualitativi e quantitativi, molte persone|poca profondità|
|Prodotti simili|utile per i requisiti|—|
|Osservazione diretta|capire task e contesto|tempo, tantissimi dati|
|Osservazione indiretta (log, diari)|traccia dei task correnti|poco usata per i requisiti|
|Documentazione (manuali, regolamenti)|regole e passi, **non costa tempo agli utenti**|**non usabile da sola**|

> [!warning] ⚠️ Trappole V/F
> 
> - Le personas **non** sono persone reali → "sono persone reali" = **Falso**.
> - Le personas **non** sono una descrizione **astratta**: sono **concrete** (archetipo descritto come persona reale) → "descrizione astratta di una categoria" = **Falso**.
> - Le personas **possono** contenere interessi non correlati all'app → "non contengono alcuna informazione sugli interessi non correlati" = **Falso**.
> - Servono a **focalizzare le categorie di utenti** → **Vero**.
> - Si basano su **dati**, non su come vorremmo gli utenti; si creano **all'inizio**.

## 6. Scenari

- **Descrizione informale e narrativa** (una storia) di attività, per esplorare **contesto, necessità, requisiti**.
- **Non deve per forza** descrivere il ruolo del software, ma **può** farlo (spesso lo fa).
- La forma di storia **facilita la comunicazione** tra tutte le parti.
- Il **dettaglio varia** con la fase del progetto.
- Possono descrivere **interazioni future**, anche **non ancora realizzabili** con la tecnologia attuale.

**Anatomia** (un solo compito): **Attore** (di solito una persona, **specifica, non generica**) · **Motivatore** · **Obiettivo/Intenzione** · **Azione** · **Soluzione** (_resolution_).

> _Detailed Debbie_ (attore) _va in viaggio di lavoro_ (motivatore). _Deve prenotare un hotel economico e con buone recensioni_ (intenzione). _Consulta il sito e confronta gli hotel per prezzo e voti_ (azione). _Sceglie un hotel e prenota_ (soluzione).

**Linee guida**: rendono concreto l'astratto; **descrittivo ma non troppo**: alto livello, **niente dettagli di design** (tab, barre di ricerca, menu a sinistra → bloccano soluzioni alternative), **niente dettagli irrilevanti** (il cane Penny, il minibar, la carta American Express).

Esempio Thomson (vacanza in barca a vela): ne derivano requisiti come uso **collaborativo** dello stesso dispositivo, recensioni di utenti simili, **salvare** e riprendere, **stampare**.

> [!warning] ⚠️ Trappole V/F
> 
> - Scenari = focalizzare **un caso d'uso** → **Vero**.
> - **Possono** contenere il comportamento del software → **Vero**.
> - **Possono** descrivere interazioni oggi **non supportate** dalla tecnologia → **Vero**.
> - Le persone nello scenario **non** sono generiche (sono personas) → "sono generiche" = **Falso**.
> - Uno scenario descrive **un solo compito**.

## 7. Prototipi

**Prototipo** = modello del prodotto che **tralascia alcuni aspetti** (colore, scala, funzionalità). Per le UI: schizzi, **storyboard**, PowerPoint, video, mock-up di cartone, software con funzioni limitate (in **qualsiasi** linguaggio).

**Perché**: **esplorare** (rendere concreto, capire pregi/difetti), **raffinare** (fattibilità tecnica, trovare problemi **in anticipo**), con materiali diversi per risparmiare **tempo e denaro**; si **vede, usa e capisce** meglio di un documento, migliora la **comunicazione** nel team, stimola la **riflessione**, **risponde a domande** e aiuta a **scegliere tra alternative**.

**Il prototipo come filtro** (Lim et al.) — dimensioni: **aspetto**, **dati**, **funzionalità**, **interattività**, **struttura spaziale**. Il migliore è il più semplice che risponde alla domanda di design.

**Cosa prototipare**: parti a rischio tecnico, flusso di lavoro, sequenza di azioni del task, layout, informazioni da mostrare, punti critici.

**Design concettuale vs fisico**: concettuale = _cosa_ fa e come si comporta in astratto (**modello concettuale**, metafore); fisico = forma concreta (schermate, colori, icone, dispositivi).

### Bassa fedeltà

- **Medium diverso** dal prodotto finale (carta, cartone, post-it); **veloci, economici, modificabili**.
- **Sketch**: su carta, si buttano via, **meglio della matita che del computer** (il software limita, il disegno fa riflettere); non serve saper disegnare. Cartoncini per simulare la navigazione di un sito.
- **Storyboard**: serie di schizzi che mostra **come l'utente avanza in un task**; usato con gli **scenari** e nelle **fasi iniziali**; mostra **solo gli stati**, **non le transizioni** (difficile per situazioni dinamiche); spesso annotato.
- **Mago di Oz**: si **finge che il sistema esista**; l'utente crede di usare un sistema vero, ma le risposte le dà uno **sviluppatore nascosto**. Fasi iniziali, per capire cosa si aspetta l'utente. Problema: **tempi di risposta**. Es.: dettatura trascritta da una persona in un'altra stanza; lavagna condivisa simulata con telecamere.

### Alta fedeltà

- Materiali **attesi nel prodotto finale**, molto simile alla versione finale, linguaggio vero o Figma.
- ⚠️ Gli utenti (e i committenti) pensano che sia un **sistema completo/quasi finito**; feedback sui dettagli estetici; ci si affeziona.

|                | Bassa                  | Alta                   |
| -------------- | ---------------------- | ---------------------- |
| Costo/tempo    | basso                  | alto                   |
| Modificabilità | altissima              | bassa                  |
| Fase           | iniziale, esplorazione | avanzata, raffinamento |
| Feedback       | concetti e flusso      | dettagli, look & feel  |

### Compromessi: orizzontale vs verticale

- **Orizzontale**: **molte funzionalità, pochi dettagli**.
- **Verticale**: **poche funzionalità, molto dettaglio**.
- Intersezione = **scenario** (un percorso supportato quanto basta per un test).

![[ium_p07_s23.png]]

**Costruzione del prodotto**: attenzione alla qualità (usabilità, affidabilità, robustezza, manutenibilità…). **Prototipazione evolutiva** (il prototipo diventa il prodotto) vs **usa e getta** (_throw-away_: si impara e si riscrive da zero).

> [!warning] ⚠️ Trappole V/F
> 
> - Lo storyboard **non** rappresenta le transizioni.
> - Nel Mago di Oz l'utente **non sa** che c'è un umano dietro.
> - Un prototipo software **non** deve essere scritto nel linguaggio finale.
> - Orizzontale = ampiezza; verticale = profondità (non invertirli).

## 8. Architettura dell'informazione

**IA** = design strutturale di ambienti informativi condivisi; arte e scienza di supportare **usabilità e reperibilità (findability)**. In pratica: **strutturare l'informazione e aiutare a trovarla**. Tre ingredienti: **strutturare**, **etichettare**, **navigare**.

**Strutturare**: **granularità** (quanto grandi le unità di contenuto), **categorie**, **vocabolario**. Va **bilanciato** il bisogno degli utenti (trovare: navigando, cercando, chiedendo) con quello dell'organizzazione (**gestire**: creare, aggiornare, processi, politiche).

**Comportamenti di ricerca**

|Tipo|Cosa vuole|Esempio|
|---|---|---|
|**Contenuto noto** (_known-item_)|_the right thing_, risposta specifica|definizione, risultato di una partita|
|**Esplorativa**|_a few good things_, nessuna risposta giusta, impara cercando, non sa quando ha finito → **filtri e faccette**|un buon ristorante|
|**Esaustiva**|_everything_, non perdere nulla, ripete con parole diverse, paziente|tutto su una malattia|
|**Ritrovare** (_re-finding_)|_need it again_|segnalibri, "leggi dopo"|

**Schemi di organizzazione**: **alfabetico**, **cronologico**, **geografico** (schemi _esatti_, adatti al contenuto noto); **per argomento**, **per compito** (schemi _ambigui_, adatti all'esplorazione).

**Etichette**: devono far scattare l'**associazione giusta**, usare il **linguaggio dell'utente** (non il gergo aziendale), **spiegare i concetti nuovi** (es. easyJet: etichetta breve + descrizione). Errori: gruppi sbagliati, etichette non rappresentative o non distinguibili, gergo, **lingue mescolate**, errori di scrittura. **Etichette convenzionali** (Home, Search, Contact us, FAQ, About us, Site map, News) → si riconoscono senza sforzo. **Link contestuali**: portano a contenuti correlati fuori dalla gerarchia; il contesto deve far prevedere dove portano.

**Sistemi di navigazione**

|Tipo|Domanda|Dove|
|---|---|---|
|**Globale**|_Dove sono?_|barra in tutte le pagine|
|**Locale**|_Cosa c'è qui vicino?_|menu della sezione|
|**Contestuale**|_Cosa è collegato a ciò che guardo?_|link nel contenuto|
|(Supplementari)|mappa del sito, indice A–Z, ricerca, breadcrumb||

---

# Parte B – La componente umana

Modello (Dix): l'uomo come **sistema di elaborazione dell'informazione**: **input/output** (vista, udito, tatto, movimento) → **memoria** (sensoriale, breve, lungo termine) → **elaborazione** (ragionamento, problem solving, errori). Le capacità sono influenzate dalle **emozioni**; ogni persona è diversa; la stessa UI è usata da utenti con esperienza molto diversa.

## 9. Percezione visiva

**La percezione visiva è un processo combinato**: la **retina** è il **sensore** (acquisisce), il **cervello** è l'**elaboratore** (interpreta). Occhio → fisiologi; cervello → psicologi.

**Occhio**: **cornea** (trasparente) · **iride** (colore, regola la luce) · **pupilla** · **cristallino** (lente, **messa a fuoco**) · **retina** (fotorecettori) · **fovea** (centro, **coni**, massima nitidezza) · **punto cieco** (nervo ottico, **nessun fotorecettore**; il cervello "riempie" il buco).

**La percezione di un oggetto dipende da**: oggetto, **luce** che lo illumina, **sfondo**, **persona**. (Oggetto blu sotto luce rossa → appare **nero**.)

### Coni e bastoncelli

|             | Bastoncelli                                  | Coni                          |
| ----------- | -------------------------------------------- | ----------------------------- |
| Sensibili a | **luminosità**                               | **lunghezza d'onda = colore** |
| Dove        | **periferia** della retina                   | **fovea**                     |
| Sensibilità | altissima, si saturano → **visione al buio** | serve più luce                |
| Risoluzione | bassa                                        | alta                          |
| Ruolo       | **movimento**, visione periferica            | dettagli, colori              |

- **Tre tipi di coni**: **blu** (onde corte) : **verde** (medie) : **rosso** (lunghe) = **1 : 2 : 4**.
- ⇒ il **rosso attira l'attenzione**; il **blu va bene per gli sfondi** (pochi coni blu, quasi nessuno nella fovea → male per testo e dettagli).
- I bastoncelli **non distinguono i colori** e sono **di un solo tipo**: non ha senso dire che sono "in numero diverso per ogni colore".

### Dalla luce all'immagine

- Le immagini arrivano sulla retina **a fuoco e invertite**; è il **cervello** che riconosce **pattern** e **movimento**.
- **Angolo visuale**: quanta parte del campo visivo occupa l'oggetto (dipende da dimensione **e** distanza).
- **Acutezza visiva**: capacità di percepire i dettagli.
- **Costanza della dimensione**: gli **oggetti familiari** sono percepiti della **stessa dimensione** anche se l'angolo visuale cambia con la distanza.
- **Le immagini dei due occhi sono diverse** (punti di vista diversi → disparità binoculare); il cervello le fonde in una sola.

### Colore

- **Opponenza cromatica**: tre coppie **rosso–verde, blu–giallo, nero–bianco**. Sovrastimolando un colore (bandiera giallo-verde-nera fissata 30 s) e poi guardando il bianco, si vede l'**opposto** (bandiera USA) → **immagine residua**. I sei colori sono molto riconoscibili.
- **Contrasto simultaneo**: lo **stesso grigio** sembra più chiaro su fondo scuro e più scuro su fondo chiaro → attenzione se il colore **veicola informazione**.
- **Ruota dei colori**: **analoghi** (adiacenti → schemi armoniosi), **complementari** (opposti → contrasto netto). Evitare la **color pollution** (troppi colori).
- **Cromostereopsi**: rosso–blu o rosso–verde accostati sembrano su **piani diversi** e "vibrano" (rifrazione diversa nel cristallino) → evitare per testo/sfondo.
- Strumenti: Figma Color Wheel. **Daltonismo** (~8% uomini, rosso-verde): mai affidare l'informazione **solo** al colore. WCAG: contrasto ≥ **4.5:1**.

### Reperimento visivo

- **Endogeno** = ricerca **volontaria**; **esogeno** = attrazione **involontaria** (lampeggio).
- Attenzione visiva **selettiva**. Canali per attirarla: **forma, colore, spessore, orientamento, curvatura, ombreggiatura**.
- Target diverso per **un solo attributo** (colore) → _pop-out_, ricerca immediata e parallela; per **congiunzione** (forma **e** colore) → ricerca **seriale**, lenta.

![[ium_p09_s24.png]]

### Indizi di profondità

|**Monoculari** (un occhio basta)|**Binoculari**|
|---|---|
|prospettiva lineare (convergenza al punto di fuga), prospettiva aerea (lontano = sfocato e bluastro), occlusione, altezza relativa, grandezza relativa, familiarità della dimensione, gradiente di tessitura, ombre, **parallasse di movimento**|**disparità binoculare** (stereoscopia), **convergenza**|

- La **prospettiva lineare può modificare la percezione delle dimensioni** (illusione di Ponzo: la barra "più lontana" sembra più lunga).
- Parallasse: muovendoci, gli oggetti vicini scorrono veloci, i lontani lenti (siti _parallax_, videogiochi).
- ~**10%** della popolazione è **stereo-blind** ma usa gli altri indizi.

### Compensazione e illusioni

Il sistema visivo **compensa** movimento e variazioni di luminanza e usa il **contesto** per risolvere le ambiguità; la **sovra-compensazione** genera **illusioni**:

- **Müller-Lyer** (linee uguali con pinne diverse sembrano di lunghezza diversa);
- **Ponzo** (binari convergenti);
- **Stanza di Ames** (trapezoidale: persone che sembrano di dimensioni diverse);
- **Contorni illusori** (triangolo di Kanizsa);
- **Café wall** (linee parallele che sembrano inclinate). ⇒ **percezione ≠ realtà fisica**: nelle UI servono correzioni ottiche.

> [!warning] ⚠️ Trappole V/F
> 
> - I **bastoncelli** non riguardano il colore → "bastoncelli in numero diverso per colore" = **Falso** (sono i **coni** ad avere proporzioni 1:2:4).
> - Oggetti familiari = dimensione costante anche cambiando angolo visuale → **Vero**.
> - La prospettiva lineare può modificare la dimensione percepita → **Vero**.
> - Le immagini dei due occhi **non** sono uguali → **Falso**.
> - Percezione = **acquisizione (occhio) + elaborazione (cervello)** → **Vero**.
> - Il blu è buono **per gli sfondi**, non per il testo.
> - Il punto cieco **non ha** fotorecettori.

## 10. Il processo di lettura

**Stadi**: **percezione** del pattern visivo → **decodifica** (rappresentazione interna del linguaggio) → **interpretazione** (sintassi, semantica, pragmatica).

- La lettura alterna **saccadi** (salti rapidi) e **fissazioni**; ci sono anche regressioni.
- **La percezione/il riconoscimento avviene durante le fissazioni** (~**94%** del tempo).
- Parola **sconosciuta** → **più fissazioni** (si scandiscono le lettere); parole **familiari** riconosciute **dalla forma**.
- Velocità ~**250 parole/min**; le parole non sono lette carattere per carattere.
- Il **MAIUSCOLO** ha forma uniforme (niente ascendenti/discendenti) → costringe a leggere lettera per lettera → **rallenta**. La leggibilità dipende soprattutto dalla **metà superiore** delle parole. Maiuscole ok per **sigle e codici** (AZ7815).
- **Contesto**: lo stesso segno si legge **B** tra lettere e **13** tra numeri; le aspettative ingannano ("il il" a cavallo dell'a capo); **ricostruzione semantica** (lettere mescolate leggibili se prima e ultima sono al loro posto).
- **Massima velocità**: font **9–12 pt**, riga **58–132 mm** → **la lunghezza della riga influenza la velocità**.

**Font**: **serif** (con grazie: Times, Palatino) · **sans-serif** (senza: Helvetica, Arial) · **monospaziato** (Courier, larghezza fissa) vs **proporzionale**. 1 pt = **1/72 di pollice**.

- Su **carta**: serif leggermente più leggibili per testi lunghi; sans-serif per titoli/testi brevi.
- Su **schermo** (72–300 dpi contro 1200–2400 della stampa, **jagging**): consigliati i **sans-serif**. Lettura a video ~**25% più lenta** (+15% fissazioni per riga).
- Estetica ≠ leggibilità (Libro d'Ore gotico).
- La scelta di **font e dimensione dipende anche dagli utenti** (es. età media: anziani → caratteri più grandi, più contrasto).
- **Contrasto** foreground/background alto → **più leggibilità**. Scuro su chiaro migliora la lettura (sui vecchi CRT favoriva il flickering).

**Tipografia digitale**

- Blocchi uniformi, bilanciare **pieni e vuoti** (no _wall of text_), allineamento **a sinistra sul web**, giustificato su carta.
- Evitare **vedove** (parola sola a fine paragrafo), **orfani** (riga isolata in nuova colonna/pagina), **fiumi** (canali bianchi nel giustificato).
- Pochi font, coerenti; font distintivo per marchi/titoli; **niente testo come immagine**.
- **Grassetto** per sostantivi/aggettivi chiave, con parsimonia (**test della pellicola**: leggendo solo i grassetti si capisce il senso). **Corsivo** per citazioni/titoli/termini tecnici, non per testi lunghi. **Sottolineato** = link sul web, non per enfasi.

**Gestalt (raggruppamento)**: **prossimità** (vicini = gruppo), **similarità** (forma simile), **regione comune** (bordo attorno), **chiusura** (forme aperte viste chiuse), **continuità** (allineati/continui = gruppo).

**Lettura sul web – F-shape**: gli utenti **scansionano**: riga orizzontale in alto, una seconda più corta, poi verticale sul lato sinistro (anche su mobile). _Google golden triangle_: attenzione in alto a sinistra.  
**Contrastare la F-shape**: contenuto importante nei **primi due paragrafi**; **titoli** e sottotitoli con **parole chiave all'inizio**; raggruppare; **grassetto**; link con testo che **anticipa** il contenuto (no "clicca qui"); **elenchi** puntati/numerati; **tagliare** il superfluo.

> [!warning] ⚠️ Trappole V/F
> 
> - Lunghezza della riga → impatto sulla velocità → **Vero**.
> - Font e dimensione **dipendono** dall'età degli utenti → "non è influenzata" = **Falso**.
> - Più contrasto → più leggibilità → **Vero**.
> - Riconoscimento della parola **durante la fissazione** (non durante la saccade) → **Vero**.
> - Parola sconosciuta → **più fissazioni** → **Vero**.
> - Il maiuscolo **rallenta** la lettura dei testi lunghi.
> - Sul web gli utenti **non** leggono tutto.

## 11. Udito, tatto e movimento (legge di Fitts)

### Udito

- Suono = vibrazione in un mezzo elastico. Dà informazioni su distanze, direzioni, oggetti.
- Udito → **sorgenti primarie** (oggetti che **emettono**); vista → **sorgenti secondarie** (oggetti che **riflettono** la luce).
- **Non richiede attenzione pregressa** (non si chiudono le orecchie) → ottimo per **allarmi** e notifiche.
- **Orecchio**: esterno (protegge, **amplifica**) · medio (timpano + ossicini, **trasmette vibrazioni**) · interno (coclea → **trasmettitori chimici** → nervo uditivo; **apparato vestibolare** → accelerazioni/equilibrio). **Chinetosi**: vista e vestibolo in conflitto (leggere in auto, VR).
- **Altezza** (frequenza: grave/acuto) · **intensità** (volume) · **timbro** (strumento).
- Range **20 Hz – 15 kHz**; distinguiamo **peggio le alte frequenze**.
- Filtro: il rumore di fondo viene eliminato (**cocktail party effect**), ma i suoni **strani** non vengono filtrati.
- Earcons (suoni astratti) vs auditory icons (suoni realistici, carta accartocciata).

### Tatto

- Feedback sull'ambiente (temperatura, peso) → coordinare i movimenti. **Feedback aptico = tatto + cinestesia**.
- Senso **diffuso** (non localizzato), aree più sensibili (polpastrelli, labbra) in base all'**uso**.
- Recettori: **termocettori** (temperatura), **nocicettori/chemiocettori** (pressione, calore, dolore **intensi**), **meccanocettori** (pressione).
- Fondamentale per **disabilità visive** (Braille, vibrazione).
- **Cinestesia** = consapevolezza della **posizione** del corpo (recettori in **muscoli e tendini**). **Propriocezione**: toccarsi il naso a occhi chiusi, digitare senza guardare; **assente in alcuni ambienti di simulazione** (VR).

### Movimento

- **Tempo di risposta = tempo di reazione + tempo di movimento**.
- Tempo di movimento: età, forma fisica, allenamento. Tempo di reazione: **visivo ~200 ms, uditivo ~150 ms, dolore ~700 ms**.
- Chiedere più velocità **riduce l'accuratezza** dell'**inesperto**, non dell'**esperto** addestrato.

### Legge di Fitts

$$Mt = a + b \log_2\left(\frac{D}{W} + 1\right)$$

- $Mt$ tempo di movimento; $D$ **distanza** dal bersaglio; $W$ **dimensione** del bersaglio; $a, b$ **costanti empiriche** (dipendono da dispositivo e utente).
- **Predice** il tempo per puntare un oggetto dati **dimensione e distanza**. È un **modello predittivo/statistico**: **non** calcola "in modo preciso" il tempo per ogni pulsante.
- Per usarla **non** serve conoscere il rapporto tra spostamento del mouse sulla scrivania e del puntatore sullo schermo: tutto ciò è inglobato nelle costanti $a$ e $b$ (ricavate sperimentalmente).
- $ID = \log_2(D/W+1)$ = **indice di difficoltà** (bit).
- **Conseguenze di design**: **ingrandire** e **avvicinare** i controlli **usati più spesso** → privilegiare le funzioni frequenti.
- **È logaritmica**: +20% a un bersaglio **piccolo** → guadagno **sensibile**; +20% a uno **già grande** → guadagno trascurabile.
- **Due fasi**: movimento **balistico** veloce e impreciso → fase lenta di **precisione** (dipende da $W$).
- **Angoli attivi**: il puntatore **non esce dallo schermo**, quindi **bordi e angoli** sono bersagli di **dimensione infinita** (non a _distanza_ infinita!). Es. barra dei menu macOS, pulsante Start, hot corners. Menu contestuali/pie menu → $D \approx 0$.
- Esempio: chiudere app in iOS 4–6 (bottone "−" minuscolo) vs iOS 7 (anteprima grande da trascinare in alto).

> [!example] Esempio numerico $a = 50$ ms, $b = 150$ ms/bit, bottone largo 20 px a 300 px: $ID = \log_2(16) = 4$ bit → $Mt = 650$ ms. Con larghezza 40 px: $ID ≈ 3{,}09$ → $Mt ≈ 513$ ms.

> [!warning] ⚠️ Trappole V/F
> 
> - Fitts predice il tempo dati dimensione e distanza → **Vero**.
> - Fitts calcola **in modo preciso** il tempo per ogni pulsante → **Falso** (è un modello predittivo con costanti empiriche).
> - Angoli = punti a **distanza infinita** → **Falso** (sono bersagli di **dimensione infinita**).
> - Serve il rapporto mouse-scrivania/puntatore-schermo → **Falso**.
> - Usata per **privilegiare le funzioni frequenti** → **Vero**.
> - L'udito non richiede attenzione pregressa; reazione uditiva **più rapida** di quella visiva.

## 12. Memoria

**Memoria** = processi cognitivi che permettono di **apprendere dall'esperienza** e **pianificare comportamenti futuri**. Fasi: **acquisizione** (codifica) → **ritenzione** → **recupero**.

**Modello modale (Atkinson & Shiffrin)**: **Stimoli → Memoria sensoriale —(attenzione)→ STM —(ripasso/rehearsal)→ LTM** (e dalla LTM si recupera verso la STM).

| |Durata|Capacità|Accesso / decadimento|
|---|---|---|---|
|**Sensoriale**|200–500 ms|limitatissima|rapidissimo|
|**STM**|secondi → 1 minuto|**7 ± 2 chunk**|accesso ~70 ms, decadimento ~200 ms|
|**LTM**|tutta la vita|enorme|accesso ~0,1 s, decadimento lento/nullo|

⇒ la STM ha **rapidità intermedia** sia di accesso sia di decadimento rispetto alle altre.

### Memoria sensoriale

- **Buffer** per ogni senso: **iconica** (visiva, ~300 ms; scia dei fuochi d'artificio), **ecoica** (uditiva, ~2 s; direzione del suono), **aptica** (tattile).
- Sovrascritta continuamente; permette valutazioni **"differenziali"** (traiettorie, direzioni).
- Sperling: l'intera griglia di lettere è nella memoria iconica, ma svanisce prima di poterla riportare tutta.
- ⚠️ È la memoria **sensoriale** a essere suddivisa **per stimolo sensoriale**, non la STM.

### Attenzione

**Selezione degli stimoli** in base a **interesse o necessità** (_arousal_), per non essere sovraccaricati. È il ponte **SM → STM** (non STM → LTM).

### Memoria a breve termine (STM)

- **Memoria di lavoro** (una "cache"): tiene l'informazione dell'elaborazione in corso (es. ricordare l'inizio di una frase per capirla).
- **7 ± 2 chunk** (Miller, 1956). ⚠️ Contano solo gli elementi da **ricordare**: le voci di un menu sono **visibili**, quindi la regola "max 7 voci per menu" è un abuso di Miller.
- **Chunking**: la STM **organizza le informazioni in blocchi**, aumentando la capacità (telefono `070 675 85 40`; "A DOG LOST IN THE WOODS" = 1 frase invece di 18 lettere). Formazione di un chunk = **chiusura** (_closure_).
- **Il designer può favorire i chunk**: raggruppando cifre (IBAN, carte, codici), dividendo i moduli in sezioni, usando parole con significato.
- **Raggiunto l'obiettivo si svuota la STM** → evitare azioni **dopo** l'obiettivo (**post-completion error**: bancomat dimenticato dopo aver preso i soldi → gli ATM danno la carta **prima**).
- **Interferenze** disturbano la STM; **recency effect** (ultimi elementi ricordati meglio), che **sparisce** se si fa un altro compito nel mezzo; **primacy effect** (primi elementi, già passati in LTM). ⇒ compiti che impegnano la STM vanno completati **senza distrazioni**.

**Modello di Baddeley (memoria di lavoro)**:

- **Esecutivo centrale**: gestisce le risorse limitate e coordina;
- **Taccuino visuo-spaziale**: informazione visiva, spaziale, aptica;
- **Loop fonologico**: informazione verbale/acustica ("voce interiore");
- **Buffer episodico**: integra i sistemi tra loro e con la LTM.
- **Interferenza solo se si impegna ripetutamente lo stesso componente** (podcast + guardare un paesaggio sì; leggere mentre qualcuno parla no).
- ⚠️ I componenti sono funzionali (visuo-spaziale, verbale…), **non** "uno per ogni stimolo sensoriale" come nella memoria sensoriale.

**Implicazioni di progetto STM**: ridurre il carico sulla memoria di lavoro; **mai** chiedere di ricordare stati del sistema; mostrare **sempre lo stato** del processo ("passo 2 di 4"); far capire **quale input** il sistema si aspetta; riepiloghi visibili, codici OTP incollabili.

### Memoria a lungo termine (LTM)

- **Esplicita/dichiarativa (conscia)**:
    - **episodica**: eventi ed esperienze, in **ordine seriale**;
    - **semantica**: fatti, concetti, capacità **strutturati**; **deriva** dall'episodica.
- **Implicita/procedurale (inconscia)**: abilità automatiche — **andare in bicicletta**, usare la penna, digitare.
- La memoria umana è **distribuita** nel cervello (non è come quella del computer).
- La **memoria semantica** fornisce **accesso**, rappresenta le **relazioni** tra informazioni e supporta l'**inferenza**. Può essere rappresentata con **vari modelli complementari**:

|Modello|Cosa rappresenta|Relazioni / meccanismo|Esempio|
|---|---|---|---|
|**Reti semantiche**|concetti e relazioni, **grafo diretto**|**ereditarietà** (is-a, instance), relazioni esplicite, deduzione|Animale → Cane → Collie → Lassie: Lassie abbaia e respira|
|**Frames**|oggetti tipici come **struttura dati** (slot)|**tipo–sottotipo**; campi **fixed / default / variable**|DOG (legs: 4; sound: bark) → COLLIE|
|**Scripts**|situazioni **stereotipate**, sequenze di azioni nel tempo|**contesto**: situazione iniziale, risultati, proprietà/oggetti, ruoli, **scene**, tracce/varianti; inferire il non detto|visita dal veterinario|
|**Regole di produzione**|conoscenza **procedurale**|**SE condizione ALLORA azione**|SE il cane ringhia ALLORA scappa|

⚠️ Le **regole di produzione non esprimono relazioni tipo–sottotipo** (quelle sono dei **frame**; l'ereditarietà è delle reti semantiche).

**Script della visita veterinaria**: _situazione iniziale_ (cane malato, ambulatorio aperto, proprietario può pagare) · _risultati_ (cane meglio, proprietario con meno soldi, veterinario con più soldi) · _proprietà_ (tabella esami, medicine, strumenti) · _ruoli_ (veterinario visita/diagnostica/cura; proprietario porta il cane e paga) · _scene_ (reception → attesa → visita → pagamento) · _tracce_ (servono medicine / serve un intervento).

### Processi della LTM

**Memorizzare**

- STM → LTM tramite **rehearsal** (ripetizione), **non** tramite l'attenzione.
- **Ipotesi del tempo totale**: ricordo ∝ tempo di ripetizione.
- **Distribution of practice**: a parità di tempo, meglio **distribuire** lo studio.
- Si ricorda meglio ciò che ha **struttura, significato, familiarità**; i concetti **astratti** sono più difficili dei **concreti** (esperimento dei simboli del tris: conoscere la regola elimina il carico di memoria).
- ⇒ per far ricordare le funzioni di un'app servono **nomi significativi e familiari**, non "più corti possibile".
- **Doppia codifica (Paivio)**: sistema **verbale** (logogeni) e **visivo** (imageni); le immagini sono codificate in **entrambi** → **superiorità dell'immagine** (icona + etichetta > sola etichetta).

**Dimenticare**

- **Decadimento** graduale e lento.
- **Interferenza retroattiva**: il **nuovo** rimpiazza il vecchio (nuovo numero di telefono).
- **Inibizione proattiva**: il **vecchio** interferisce con il nuovo (tornare alla vecchia casa; cercare un comando nel vecchio posto dopo un redesign).
- Ricordiamo meglio parole legate a **emozioni**; tendiamo a **richiamare le esperienze positive** e inibire le negative ("bei tempi andati").
- Non è chiaro se si dimentichi davvero o se l'informazione sia solo **difficile da recuperare**.

**Recuperare**

- **Ricordo (recall)**: "ripescare" l'informazione, aiutati da **indizi** (categorie, visualizzazione).
- **Riconoscimento (recognition)**: capire che un'informazione è **già vista**; **più facile** del ricordo (l'informazione stessa fa da indizio) → GUI più facili della riga di comando; euristica di Nielsen _recognition rather than recall_.

> [!warning] ⚠️ Trappole V/F
> 
> - STM organizza in blocchi → **Vero**.
> - STM suddivisa in componenti **per stimolo sensoriale** → **Falso** (quella è la memoria sensoriale).
> - STM con rapidità **intermedia** di accesso e decadimento → **Vero**.
> - STM → LTM tramite **attenzione** → **Falso** (tramite **rehearsal**; l'attenzione è SM → STM).
> - Il designer **non può** influenzare i chunk → **Falso**.
> - LTM mantiene le relazioni tra informazioni → **Vero**.
> - LTM rappresentabile con vari modelli → **Vero**.
> - Regole di produzione esprimono tipo–sottotipo → **Falso**.
> - Riconoscimento **più difficile** del ricordo → **Falso**.
> - Si richiamano più facilmente le esperienze positive → **Vero**.
> - Andare in bici = memoria **conscia** → **Falso** (implicita/procedurale).
> - Il vecchio può interferire col nuovo → **Vero** (inibizione proattiva).
> - Nomi più corti possibile per ricordare le funzioni → **Falso** (servono nomi **significativi**).

## 13. Ragionamento

Pensiero = **ragionamento** + **problem solving**. Il ragionamento usa la conoscenza per **trarre conclusioni**.

|Tipo|Direzione|Affidabile?|Esempio|
|---|---|---|---|
|**Deduttivo**|regola + caso → conclusione|sì, **logicamente** valido|"Se è venerdì va al lavoro; è venerdì ⇒ va al lavoro"|
|**Induttivo**|casi osservati → regola generale|no: può solo **falsificare**|"Tutti gli elefanti visti hanno la proboscide ⇒ tutti"|
|**Abduttivo**|effetto + regola → causa probabile|no: **false spiegazioni**|"Marco guida veloce ⇒ avrà bevuto"|

- **Deduzione**: conclusioni logicamente corrette anche se **assurde** ("se piove il terreno è asciutto…") → le scartiamo con la **conoscenza del mondo**, che però può anche farci **vedere deduzioni inesistenti** ("alcune persone sono bambini, alcuni bambini piangono ⇒ alcune persone piangono": **non segue** logicamente).
- **Induzione**: utile per costruire modelli; usiamo meglio l'evidenza **positiva** che la **negativa**.
- **Carte di Wason**: regola "se pari ⇒ retro rosso". Si girano l'**8** e la carta **blu** (cercare il controesempio), non la rossa. Con contenuto **familiare** ("se bevi alcolici hai ≥ 18 anni" → girare **15** e **birra**) quasi tutti rispondono giusto.
- **Abduzione**: la usiamo di continuo per spiegare il comportamento dei sistemi ("non risponde ⇒ si è bloccato").

**XAI (spiegazioni dell'IA)**:

- **Induttivo** → esempi simili del training set (**k-NN**, Twin Systems);
- **Abduttivo** → la causa più probabile: **Grad-CAM** (mappa di salienza su immagini), **LIME** (parole più importanti per la classe predetta);
- **Deduttivo** → applica regole: **CNN-LSTM** che genera una giustificazione testuale ("è un 1 perché è una linea dritta"), LIME per ogni classe.

## 14. Problem solving

Ricerca della soluzione a problemi **non familiari** usando la conoscenza. Tre teorie:

### Gestalt

- **Riproduttivo**: riapplicare soluzioni già note.
- **Produttivo**: **ristrutturare** il problema con l'**insight** ("aha!").
- Limiti: non spiega **quando** avviene la ristrutturazione né **cos'è** l'intuito. Merito: supera il **comportamentismo**, apre all'elaborazione dell'informazione.

### Teoria del problem space (Newell & Simon, anni '70)

- Il **problem space** contiene **tutti gli stati** del problema: **stato iniziale**, **stati intermedi**, **stato obiettivo** (_goal state_).
- Si risolve **generando stati** tramite **operatori legali** (le mosse/azioni), che hanno **condizioni di applicabilità** (precondizioni): **non tutti gli operatori sono applicabili in ogni stato**.
- ⚠️ Gli **operatori non sono stati**: sono le **azioni** che trasformano uno stato in un altro.
- Euristica: **analisi mezzi-fini** → scegliere l'operatore che **riduce di più la differenza** tra stato corrente e goal; se non è applicabile, renderlo applicabile diventa un **sotto-obiettivo**.
- Adatta a problemi **ben definiti** (giochi, puzzle); lavora entro i limiti umani (**capacità della STM**, velocità di recupero).
- **Non** tiene conto di **emozioni** o condizioni affettive: è un modello di elaborazione dell'informazione.
- Es.: spostare la scrivania (operatori sollevare/trascinare; condizione: sollevo solo oggetti leggeri; sotto-obiettivo: svuotarla). Cancellare un paragrafo (sotto-obiettivi: selezionare, poi eliminare). **Lupo, pecora e cavolo**: 7 mosse, la 4ª (riportare indietro la pecora) "allontana" dal goal ed è per questo controintuitiva.

### Analogia

- Si risolve un problema nuovo trovando una **corrispondenza** con un **dominio noto** e **trasferendo** metodi e soluzioni.
- Può essere **difficile** se i domini sono **semanticamente lontani**; nella Gestalt è una **ristrutturazione produttiva**.
- **Problema delle radiazioni** (Duncker; Gick & Holyoak): tumore → **molti raggi deboli da direzioni diverse** che convergono, come il generale che divide l'esercito su più strade. Molti trovano la soluzione **solo se viene suggerito** il collegamento tra le storie.
- ⇒ la capacità di risolvere problemi **può essere stimolata con analogie** (ed è la base delle **metafore** nelle interfacce).

> [!warning] ⚠️ Trappole V/F
> 
> - L'analogia stimola il problem solving → **Vero**.
> - "Gli operatori sono stati intermedi" → **Falso** (sono le azioni/mosse).
> - Il problem space considera le emozioni → **Falso**.
> - Tutti gli operatori applicabili sempre → **Falso** (hanno precondizioni).
> - L'induzione **non** può dimostrare che una regola è vera.

## 15. Errori (sviste, lapsus, errori)

> [!note] Questo argomento non è nelle tue note di teoria, ma compare nella simulazione: lo integro dalla classificazione classica (Norman / Reason, ripresa da Dix). Le definizioni sono quelle standard; le fonti italiane variano nella traduzione, quindi tieni come riferimento la chiave della simulazione.

|Tipo|Intenzione|Cosa va storto|Esempio|
|---|---|---|---|
|**Svista** (_slip_)|**corretta**|l'**esecuzione** fallisce (disattenzione, automatismo, scarsa abilità)|voglio cliccare "Salva" e clicco "Chiudi" accanto|
|**Lapsus** (_lapse_)|corretta|**dimentico** un passo o l'intenzione (fallimento di **memoria**)|dimentico il bancomat dopo il prelievo (post-completion error)|
|**Errore** (_mistake_)|**sbagliata**|il **piano** è sbagliato per un modello mentale errato|penso che "chiudere la finestra" salvi il documento|

- Nella simulazione: "intenzione corretta ma fallisco nel metterla in atto ⇒ **lapsus**" è **Falso** → quel caso è una **svista (slip)**.
- Design: le sviste si prevengono con bersagli grandi e distanziati, conferme per azioni distruttive, **undo**; gli errori con un buon **modello concettuale** e feedback chiari.

## 16. Metafore

**Metafore = analogie suggerite dal designer**: il modello dell'interfaccia si basa su un **corrispettivo reale**, e l'utente risolve i problemi **pensando agli operatori reali** (so cosa fare con un cestino → so cosa fare col cestino del desktop).

**Limiti**: è **difficile mappare tutto il sistema in un'unica metafora**; la metafora può essere **fuorviante** se presa **alla lettera** → **modello concettuale sbagliato**.

**Esempi**: **desktop** (lo schermo è la scrivania, documenti, cartelle, anche il disordine; Xerox Star 1981 → Macintosh 1984 → Windows; paradigma **WIMP**, sfrutta il **riconoscimento**); **cestino**; **macchina da scrivere** (Word: foglio bianco, cursore); **Paint** (tela, pennelli, gomma, tavolozza).

**Scheumorfismo**: design **simile alla controparte reale** (iBooks: scaffale di legno, pagina che si gira). Aiuta i principianti ma aggiunge decorazioni inutili e i limiti dell'oggetto fisico → dal 2013 **flat design** (iOS 7, Material).

**Critiche alle metafore**

- **Forzature** sempre presenti (trascinare il disco nel cestino per espellerlo nel vecchio Mac).
- **Limitano le funzioni** possibili (una cartella vera non sta in due posti, un file sì).
- Possono **confliggere con principi di progetto**.
- Impediscono di **capire il sistema oltre la metafora**.
- Rischio di copiare **oggetti fisici progettati male** (calcolatrice di sistema senza cronologia).
- **Limitano l'immaginazione** del progettista.

**Hall of shame – Microsoft Bob (1995)**: desktop sostituito da una **casa** con stanze e oggetti e il cane-assistente **Rover**; metafora **all'estremo**, lenta e infantile per gli esperti, non insegnava nulla ai principianti, richiedeva hardware potente; ritirato nel 1996 (eredità: **Clippy**).

---

# Parte C – Modelli per le domande aperte

> [!tip] Regola generale Struttura sempre: **definizione breve → elementi/criteri con nome in grassetto → motivazione legata alla teoria (percezione, memoria, Fitts…) → esempio concreto e nuovo**. Un esempio "non visto a lezione" vale più di uno copiato dalle slide.

### C1. "Descrivi N criteri per supportare la lettura su schermo" (news, blog, documentazione…)

Scegli fra questi, **nominando il principio teorico** dietro ognuno:

1. **Supporto alla scansione (F-shape)**: piramide invertita (il succo nei primi due paragrafi), titoli e sottotitoli con parole chiave all'inizio, paragrafi brevi, elenchi, grassetto selettivo (test della pellicola), link descrittivi.
2. **Tipografia per lo schermo**: sans-serif, corpo ~12–14 px (9–12 pt), **riga 58–132 mm** (≈ 50–75 caratteri), interlinea ampia, allineamento a sinistra, niente testo lungo in maiuscolo o corsivo, niente sottolineato non-link.
3. **Contrasto e colore**: testo scuro su sfondo chiaro, contrasto ≥ 4.5:1, niente cromostereopsi (rosso/blu), colore mai unico portatore di significato.
4. **Spazio bianco e Gestalt**: bilanciare pieni/vuoti, evitare il _wall of text_, raggruppare per prossimità/regione comune, isolare l'articolo da banner e distrazioni (meno carico cognitivo, reperimento visivo endogeno non disturbato da stimoli esogeni lampeggianti).
5. **Adattamento agli utenti**: dimensione regolabile del font (utenti anziani), layout responsive per mobile.

### C2. "Scrivi una persona per l'app X"

Template (riempire **tutto**, in modo realistico e **coerente con l'app**):

- **Nome, età, foto (descritta), occupazione** + **etichetta** ("La pendolare organizzata").
- **Frase/citazione** che riassume l'atteggiamento.
- **Contesto**: dove vive, situazione, **dispositivi**, **quando/quanto spesso/dove** userebbe l'app, se è una scelta o un obbligo.
- **Esperienza** con la tecnologia e con il dominio (es. fitness: principiante? usa già uno smartwatch?).
- **Obiettivi** (2–4, legati all'app ma anche personali).
- **Frustrazioni / pain points** (con app simili o nella situazione attuale).
- **Tratti personali e interessi** anche non legati all'app (rendono la persona viva).
- **Bisogni → implicazioni di design** (opzionale ma utile: mostra che sai a cosa serve).

> [!example] Esempio – app di fitness **Marco Pinna, 41 anni**, impiegato in banca a Cagliari, sposato, due figli. _Etichetta_: "Il ripartente". _Frase_: "Voglio tornare in forma senza che diventi un secondo lavoro."  
> **Contesto**: sedentario durante la settimana, corre 2–3 volte a settimana la sera sul lungomare; usa uno smartwatch economico ricevuto in regalo e un Android; l'app la apre soprattutto prima e dopo la corsa e la domenica per vedere i progressi.  
> **Esperienza**: buona con lo smartphone, nessuna con app sportive; non conosce termini come "VO2max" o "zone cardio".  
> **Obiettivi**: perdere 8 kg entro l'estate; arrivare a correre 10 km; condividere i risultati nel gruppo WhatsApp con i colleghi che corrono.  
> **Frustrazioni**: app piene di grafici incomprensibili; abbonamenti a pagamento per funzioni base; dover avviare manualmente l'allenamento e dimenticarsene.  
> **Interessi**: calcio (allena la squadra del figlio), cucina la domenica.  
> **Implicazioni**: avvio automatico dell'allenamento, progressi spiegati in linguaggio semplice, obiettivi a piccoli passi, condivisione rapida.

### C3. "Scrivi uno scenario"

Cinque elementi, **un solo compito**, alto livello, **niente bottoni/menu**, niente dettagli irrilevanti: **Attore** (la persona) · **Motivatore** · **Intenzione/obiettivo** · **Azione** · **Soluzione**.

> _Esempio_: Marco (attore) ha deciso di iscriversi alla 10 km cittadina tra due mesi (motivatore). Vuole capire se il suo ritmo attuale gli permette di finirla e impostare un programma (intenzione). Dopo la corsa serale controlla i dati dell'allenamento, confronta il ritmo con le settimane precedenti e si fa proporre un piano (azione). Imposta l'obiettivo e riceve un calendario di uscite settimanali (soluzione).

### C4. "Descrivi il modello X della LTM e confrontalo con gli altri, con un esempio"

1. **Definizione** del modello e **struttura** (vedi tabella in [[#Memoria a lungo termine (LTM)]]).
2. **Cosa rappresenta bene** (statico/dinamico, dichiarativo/procedurale, relazioni).
3. **Confronto** con gli altri tre:
    - _Reti semantiche_: concetti e relazioni (is-a), **ereditarietà**, statiche, senza tempo.
    - _Frame_: struttura a slot (fixed/default/variable), **tipo–sottotipo**, descrivono oggetti/situazioni tipiche, statici.
    - _Script_: frame "dinamico" per situazioni stereotipate, **sequenza temporale di scene**, ruoli, oggetti, condizioni d'ingresso, risultati, tracce; permettono di **inferire il non detto**.
    - _Regole di produzione_: conoscenza **procedurale** atomica **SE–ALLORA**, flessibile, nessuna struttura gerarchica.
    - Sono **complementari**, non alternativi.
4. **Esempio nuovo** con tutti i campi. Es. script "ritiro di un pacco al locker": _situazione iniziale_ (pacco consegnato, ho il codice) · _ruoli_ (cliente, sistema del locker, corriere) · _oggetti_ (app, codice QR, sportello) · _scene_ (arrivo → apro l'app → scansiono il QR → si apre lo sportello → prendo il pacco → chiudo) · _risultati_ (ho il pacco, sportello libero) · _tracce_ (codice scaduto → richiedo nuovo codice). Collegamento UI: se l'interfaccia rispetta lo script atteso, l'utente non deve ragionare da zero. Frame equivalente: `SMARTPHONE — Fixed: ha schermo, batteria · Default: touch, 6" · Variable: marca, colore`. Regola: `SE la batteria < 20% ALLORA attiva il risparmio energetico`. Rete: `Dispositivo → is-a → Smartphone → instance → il mio Pixel` (eredita "ha una batteria").

### C5. "Spiega come l'analogia favorisce il problem solving, con un esempio non visto a lezione"

- **Definizione**: si mappa il problema nuovo su un **dominio noto** e si **trasferiscono** soluzioni/operatori. Difficile se i domini sono lontani; spesso serve un **suggerimento** (Gick & Holyoak). È la base delle **metafore**.
- **Esempi nuovi** (scegline uno, spiegando la **corrispondenza** elemento per elemento):
    - **Circuito elettrico come impianto idraulico**: tensione = pressione, corrente = flusso, resistenza = tubo stretto, interruttore = rubinetto → capisco perché un elettrodomestico non va.
    - **Firewall come buttafuori** di una discoteca: controlla chi entra in base a una lista.
    - **Git come salvataggi di un videogioco**: commit = checkpoint, branch = partita alternativa, revert = ricaricare il salvataggio.
    - **Gestire la memoria RAM come una scrivania**: più spazio sulla scrivania = più documenti aperti; il disco è l'armadio.
    - **Traffico di rete come traffico stradale**: congestione, corsie (banda), semafori (code).

### C6. "Modella il problema X con la teoria del problem space, con due soluzioni alternative"

Schema (sempre così):

1. **Stato iniziale (S0)** e **stato obiettivo (Sgoal)** descritti con precisione.
2. **Stati intermedi** (S1, S2…).
3. **Operatori** (azioni), ciascuno con **precondizioni** (in quali stati si applica) ed **effetto** (stato risultante).
4. **Soluzione 1** e **Soluzione 2** come sequenze `S0 –Op→ S1 –Op→ … → Sgoal`.
5. (Bonus) **analisi mezzi-fini**: quale differenza riduce ogni operatore, quali **sotto-obiettivi** nascono quando un operatore non è applicabile.

> [!example] Esempio – inviare una foto a un amico da smartphone
> 
> - **S0**: foto nella galleria, chat chiusa. **Sgoal**: foto inviata nella chat dell'amico.
> - **Op1 Apri galleria** (pre: S0) → S1 galleria aperta. **Op2 Seleziona foto** (pre: S1) → S2 foto aperta. **Op3 Tocca "Condividi" e scegli l'amico** (pre: S2, app di chat installata) → Sgoal.
> - **Op4 Apri la chat dell'amico** (pre: S0) → S3 chat aperta. **Op5 Tocca "allega" e scegli la foto** (pre: S3, permesso di accesso alle foto) → S4 foto in anteprima. **Op6 Invia** (pre: S4, connessione attiva) → Sgoal.
> - **Soluzione 1**: S0 –Op1→ S1 –Op2→ S2 –Op3→ Sgoal. **Soluzione 2**: S0 –Op4→ S3 –Op5→ S4 –Op6→ Sgoal.
> - Mezzi-fini: se manca la connessione, Op6 non è applicabile → sotto-obiettivo "attivare i dati/Wi-Fi".

### C7. Altre domande aperte probabili (schema rapido)

- **Legge di Fitts applicata a un'interfaccia**: formula + significato di D e W → due/tre interventi (ingrandire i comandi frequenti, avvicinarli, usare bordi/angoli, menu contestuali) → nota sulla logaritmicità → esempio (pulsante "scatta" grande in basso nell'app fotocamera, a portata di pollice).
- **Pro e contro delle metafore** + esempio: definizione → vantaggi (apprendibilità, riconoscimento, transfer degli operatori reali) → limiti/critiche (vedi §16) → esempio (carrello dell'e-commerce, cestino, desktop, Microsoft Bob come eccesso).
- **Scegliere un prototipo per una certa fase**: bassa fedeltà all'inizio (sketch, storyboard, Mago di Oz per funzioni non ancora realizzabili, es. un assistente vocale), alta fedeltà alla fine; orizzontale per valutare la navigazione complessiva, verticale per una funzione critica.
- **Requisiti per l'app X**: almeno uno per tipo (funzionale, non funzionale, dati, contesto fisico/sociale/organizzativo, usabilità), tutti **specifici e verificabili**.
- **Colori di un'interfaccia**: coni 1:2:4 (rosso per l'attenzione, blu per sfondi), opponenza, contrasto simultaneo, analoghi/complementari, cromostereopsi, daltonismo, color pollution.
- **Implicazioni della STM sul design**: chunking, mostrare lo stato, non far ricordare informazioni tra schermate, post-completion error, riconoscimento > ricordo.
- **Architettura dell'informazione di un sito**: tipo di ricerca degli utenti → schema di organizzazione → etichette (linguaggio dell'utente, convenzionali) → navigazione globale/locale/contestuale.
- **Tipi di ragionamento** con esempio in una UI: deduttivo ("se l'icona è grigia è disabilitata; è grigia ⇒ disabilitata"), induttivo ("tutti i link che ho visto sono blu ⇒ i link sono blu"), abduttivo ("la pagina non si carica ⇒ è caduta la rete").

---

# Parte D – Tutte le domande della simulazione

|#|Affermazione|Risposta|Perché|
|---|---|---|---|
|1|Il coinvolgimento degli utenti aiuta a mantenere realistiche le aspettative|**V**|gestione delle aspettative|
|2|Gli utenti coinvolti possono **sempre** essere sostituiti da un esperto di interfacce|**F**|l'esperto non ha bisogni e contesto dell'utente|
|3|I requisiti non forniscono alcuna informazione sul contesto d'uso|**F**|esistono requisiti di ambiente/contesto (fisico, sociale, organizzativo)|
|4|I requisiti forniscono un'informazione approssimativa|**F**|devono essere chiari, specifici, verificabili|
|5|I bastoncelli sono in numero diverso per ogni colore|**F**|i bastoncelli non vedono i colori; sono i coni (1:2:4)|
|6|Oggetti familiari percepiti con dimensione costante anche variando l'angolo visuale|**V**|costanza della dimensione|
|7|La prospettiva lineare può modificare la percezione della dimensione|**V**|illusione di Ponzo|
|8|Le immagini percepite dai due occhi sono uguali|**F**|disparità binoculare|
|9|Percezione visiva = acquisizione dagli occhi + elaborazione del cervello|**V**|retina sensore, cervello elaboratore|
|10|La lunghezza della riga influenza la velocità di lettura|**V**|ottimo 58–132 mm|
|11|Font e dimensione non dipendono dall'età media degli utenti|**F**|dipendono dagli utenti|
|12|Più contrasto foreground/background → più leggibilità|**V**||
|13|Il riconoscimento di una parola avviene durante la fissazione|**V**|percezione nelle fissazioni (94%)|
|14|Parola sconosciuta → più fissazioni|**V**|scansione delle lettere|
|15|Fitts predice il tempo di puntamento dati dimensione e distanza|**V**||
|16|Fitts calcola in modo preciso il tempo medio per ogni pulsante|**F**|modello predittivo con costanti empiriche|
|17|Gli angoli attivi si modellano come punti a distanza infinita|**F**|sono bersagli di **dimensione** infinita|
|18|Serve conoscere il rapporto mouse sulla scrivania / puntatore sullo schermo|**F**|inglobato in a e b|
|19|Fitts serve per privilegiare il puntamento delle funzioni più usate|**V**||
|20|La STM organizza l'informazione in blocchi|**V**|chunking|
|21|La STM è suddivisa in componenti per stimolo sensoriale|**F**|quella è la memoria sensoriale|
|22|La STM ha rapidità intermedia di accesso e decadimento|**V**|tra sensoriale e LTM|
|23|STM → LTM tramite l'attenzione|**F**|tramite rehearsal|
|24|Il designer non può influenzare la formazione dei chunk|**F**|raggruppamenti, struttura, significato|
|25|La LTM mantiene le relazioni tra informazioni|**V**|memoria semantica|
|26|La LTM può essere rappresentata con vari modelli|**V**|reti, frame, script, regole|
|27|Le regole di produzione esprimono relazioni tipo–sottotipo|**F**|è dei frame|
|28|Il riconoscimento è più difficile del ricordo|**F**|è più facile|
|29|Si tende a richiamare esperienze positive e inibire le negative|**V**||
|30|La LTM **conscia** gestisce abilità come andare in bicicletta|**F**|memoria implicita/procedurale|
|31|Nella LTM l'informazione vecchia può interferire con la nuova|**V**|inibizione proattiva|
|32|Per memorizzare le funzioni servono nomi più corti possibile|**F**|servono nomi significativi/familiari|
|33|Il problem solving può essere stimolato con analogie|**V**||
|34|Gli operatori sono stati intermedi del problem space|**F**|sono le azioni tra stati|
|35|Intenzione corretta ma esecuzione fallita = lapsus|**F**|è una svista (slip)|
|36|La teoria del problem space considera le emozioni|**F**||
|37|Tutti gli operatori sono applicabili in ogni stato|**F**|hanno precondizioni|
|38|Le personas sono persone reali|**F**|archetipi fittizi|
|39|Le personas focalizzano le categorie di utenti|**V**||
|40|Le personas non contengono interessi non legati all'app|**F**|li contengono, le rendono vive|
|41|Le personas sono una descrizione astratta di una categoria|**F**|sono concrete|
|42|Gli scenari focalizzano un caso di utilizzo|**V**||
|43|Gli scenari possono descrivere il comportamento del software|**V**||
|44|Gli scenari possono descrivere interazioni oggi non supportabili|**V**||
|45|Le persone negli scenari sono generiche|**F**|sono personas specifiche|

**Domande aperte della simulazione** → modelli in [[#C1. "Descrivi N criteri per supportare la lettura su schermo" (news, blog, documentazione…)|C1]] (lettura news), [[#C2. "Scrivi una persona per l'app X"|C2]] (persona fitness), [[#C4. "Descrivi il modello X della LTM e confrontalo con gli altri, con un esempio"|C4]] (script), [[#C5. "Spiega come l'analogia favorisce il problem solving, con un esempio non visto a lezione"|C5]] (analogia), [[#C6. "Modella il problema X con la teoria del problem space, con due soluzioni alternative"|C6]] (allegare un file: soluzione 1 graffetta → selezione → apri; soluzione 2 apri cartella → drag & drop).

---

# Parte E – Altre domande V F simili (autoverifica)

> [!question]- HCI, definizioni, processo
> 
> 1. Un'interfaccia a riga di comando è un sistema interattivo. → **V**
> 2. L'interfaccia utente riguarda solo i sistemi digitali. → **F**
> 3. Secondo l'ISO 9241-210 il contesto d'uso comprende utenti, obiettivi e compiti, risorse e ambiente. → **V**
> 4. La UX ha un ambito più ristretto dell'interaction design. → **F**
> 5. L'utilità è una componente dell'usabilità secondo Nielsen. → **F** (le 5 sono apprendibilità, efficienza, memorabilità, errori, soddisfazione)
> 6. La memorabilità misura quanto velocemente un utente esperto esegue i compiti. → **F** (è l'efficienza)
> 7. "Intuitiva" è un requisito di usabilità verificabile. → **F**
> 8. Il genius design si basa sull'esperienza del designer senza coinvolgere gli utenti. → **V**
> 9. Gli utenti sanno sempre indicare di cosa hanno bisogno. → **F**
> 10. Secondo il ciclo task-artifact, i nuovi artefatti generano nuovi bisogni. → **V**
> 11. Il senso di proprietà rende gli utenti più propensi ad accettare il prodotto. → **V**
> 12. Raffinare una sola idea garantisce di trovare la soluzione migliore. → **F** (massimo locale)
> 13. Il design user-centered prevede misurazioni empiriche. → **V**

> [!question]- Requisiti, personas, scenari
> 
> 14. I requisiti non funzionali descrivono cosa il sistema deve fare. → **F**
> 15. Il rumore dell'ambiente è un requisito di contesto fisico. → **V**
> 16. La privacy dei clienti rientra nel contesto sociale. → **V**
> 17. "Il sito deve essere bello" è un requisito ben scritto. → **F**
> 18. Lo studio della documentazione può essere usato da solo per raccogliere i requisiti. → **F**
> 19. I focus group possono essere dominati da pochi individui. → **V**
> 20. Le personas vanno create alla fine del progetto. → **F**
> 21. Le personas devono rappresentare l'utente ideale che vorremmo avere. → **F**
> 22. Di solito si sviluppa una sola persona. → **F**
> 23. Una persona contiene il livello di esperienza nel dominio. → **V**
> 24. Uno scenario dovrebbe specificare in quale tab si trova il pulsante di ricerca. → **F**
> 25. Uno scenario descrive più compiti contemporaneamente. → **F**
> 26. Gli scenari facilitano la comunicazione tra le parti. → **V**
> 27. Il livello di dettaglio di uno scenario è sempre lo stesso. → **F**

> [!question]- Prototipi e architettura dell'informazione
> 
> 28. Un prototipo a bassa fedeltà usa materiali diversi dal prodotto finale. → **V**
> 29. Lo storyboard rappresenta bene le transizioni tra stati. → **F**
> 30. Nel Mago di Oz l'utente sa che le risposte sono date da una persona. → **F**
> 31. Un prototipo verticale implementa molte funzionalità con pochi dettagli. → **F** (è l'orizzontale)
> 32. Con un prototipo ad alta fedeltà c'è il rischio che l'utente lo creda completo. → **V**
> 33. Un prototipo software deve usare lo stesso linguaggio del prodotto finale. → **F**
> 34. Nella ricerca esplorativa esiste una risposta giusta. → **F**
> 35. I segnalibri supportano il ritrovare informazione. → **V**
> 36. Le etichette dovrebbero usare il linguaggio interno dell'azienda. → **F**
> 37. La navigazione globale risponde alla domanda "dove sono?". → **V**
> 38. L'ordinamento cronologico è adatto alle notizie. → **V**

> [!question]- Percezione visiva
> 
> 39. I coni sono concentrati nella fovea. → **V**
> 40. I bastoncelli permettono la visione al buio. → **V**
> 41. I coni rossi sono più numerosi dei coni blu. → **V** (1:2:4)
> 42. Il blu è il colore migliore per testi piccoli. → **F**
> 43. Nel punto cieco ci sono solo bastoncelli. → **F** (nessun fotorecettore)
> 44. Lo stesso colore può sembrare diverso a seconda dello sfondo. → **V**
> 45. Le coppie opponenti sono rosso-verde, blu-giallo, nero-bianco. → **V**
> 46. La cromostereopsi crea un'impressione di profondità tra colori come rosso e blu. → **V**
> 47. Il reperimento visivo esogeno è volontario. → **F**
> 48. Un elemento diverso per colore e forma insieme si trova più in fretta di uno diverso solo per colore. → **F**
> 49. L'occlusione è un indizio di profondità binoculare. → **F** (monoculare)
> 50. La parallasse di movimento è un indizio monoculare. → **V**
> 51. Chi è stereo-blind non percepisce affatto la profondità. → **F**
> 52. Le illusioni ottiche nascono da una sovra-compensazione del sistema visivo. → **V**
> 53. Gli oggetti lontani appaiono più nitidi e bluastri. → **F** (meno nitidi)

> [!question]- Lettura
> 
> 54. Durante le saccadi percepiamo la maggior parte dell'informazione. → **F**
> 55. Il testo tutto in maiuscolo si legge più velocemente. → **F**
> 56. La metà superiore delle parole è la più importante per la leggibilità. → **V**
> 57. Sullo schermo si legge circa il 25% più lentamente che su carta. → **V**
> 58. Sul web conviene usare il sottolineato per l'enfasi. → **F**
> 59. Il contesto influenza il riconoscimento di un simbolo. → **V**
> 60. La legge di prossimità dice che oggetti vicini sono percepiti come gruppo. → **V**
> 61. Senza una guida, gli utenti leggono le pagine web seguendo una F. → **V**
> 62. Un font monospaziato ha caratteri di larghezza variabile. → **F**
> 63. Usare molti font diversi aiuta a veicolare informazione. → **F**
> 64. Le parole familiari si riconoscono dalla forma. → **V**

> [!question]- Udito, tatto, movimento, Fitts
> 
> 65. L'udito percepisce principalmente sorgenti secondarie. → **F** (primarie)
> 66. L'udito è adatto ai segnali di allarme perché non richiede attenzione pregressa. → **V**
> 67. Distinguiamo meglio le alte frequenze. → **F**
> 68. L'apparato vestibolare percepisce le accelerazioni. → **V**
> 69. Il timbro dipende dalla sorgente del suono. → **V**
> 70. La cinestesia è la consapevolezza della posizione del corpo. → **V**
> 71. Il tatto è un senso localizzato solo nelle mani. → **F**
> 72. Il tempo di reazione a uno stimolo uditivo è minore di quello a uno stimolo visivo. → **V**
> 73. Chiedere più velocità riduce l'accuratezza anche negli utenti esperti. → **F**
> 74. Raddoppiare un pulsante già molto grande riduce molto il tempo di puntamento. → **F** (logaritmica)
> 75. Nella legge di Fitts, aumentare la distanza aumenta il tempo. → **V**
> 76. Le costanti a e b si ricavano sperimentalmente. → **V**
> 77. La fase finale del movimento di puntamento è veloce e imprecisa. → **F** (è lenta e precisa)

> [!question]- Memoria
> 
> 78. La memoria iconica riguarda gli stimoli uditivi. → **F**
> 79. La memoria sensoriale permette di valutare traiettorie e direzioni. → **V**
> 80. La STM ha capacità di 7 ± 2 chunk. → **V**
> 81. Un menu non deve mai avere più di 7 voci per via della legge di Miller. → **F**
> 82. Ritirare la carta dopo i soldi favorisce il post-completion error. → **V**
> 83. L'effetto recenza scompare se il soggetto svolge un altro compito prima del ricordo. → **V**
> 84. Nel modello di Baddeley il loop fonologico gestisce l'informazione visuo-spaziale. → **F**
> 85. Secondo Baddeley l'interferenza si ha solo impegnando lo stesso componente. → **V**
> 86. La memoria episodica deriva da quella semantica. → **F** (il contrario)
> 87. Le reti semantiche supportano l'ereditarietà. → **V**
> 88. Gli script rappresentano situazioni stereotipate. → **V**
> 89. Le regole di produzione rappresentano conoscenza procedurale. → **V**
> 90. Distribuire lo studio nel tempo dà risultati migliori a parità di ore. → **V**
> 91. I concetti astratti si ricordano meglio di quelli concreti. → **F**
> 92. Le immagini sono codificate sia nel sistema verbale sia in quello visivo. → **V**
> 93. L'interferenza retroattiva è quando il nuovo sostituisce il vecchio. → **V**
> 94. Le GUI sfruttano il ricordo più del riconoscimento. → **F**
> 95. È certo che l'informazione dimenticata venga cancellata. → **F**

> [!question]- Ragionamento, problem solving, metafore
> 
> 96. Il ragionamento deduttivo produce sempre conclusioni vere. → **F** (valide logicamente; se le premesse sono false la conclusione può essere falsa)
> 97. Il ragionamento induttivo può dimostrare che una regola è vera. → **F**
> 98. L'abduzione risale dagli effetti alle cause. → **V**
> 99. Nelle carte di Wason bisogna girare la carta rossa. → **F**
> 100. Ragioniamo meglio con contenuti familiari che con simboli astratti. → **V**
> 101. Grad-CAM è un esempio di spiegazione abduttiva. → **V**
> 102. Il problem solving produttivo della Gestalt si basa sull'insight. → **V**
> 103. Nell'analisi mezzi-fini si sceglie l'operatore che riduce di più la differenza dal goal. → **V**
> 104. La teoria del problem space si applica bene a problemi ben definiti. → **V**
> 105. Le analogie tra domini molto diversi sono facili da stabilire. → **F**
> 106. Le metafore sono analogie suggerite dal designer. → **V**
> 107. È facile mappare un intero sistema in un'unica metafora. → **F**
> 108. Una metafora presa alla lettera può dare un modello concettuale sbagliato. → **V**
> 109. Lo scheumorfismo rende il design simile alla controparte reale. → **V**
> 110. Le metafore ampliano sempre le funzioni possibili dell'interfaccia. → **F**
> 111. Scegliere un'opzione sbagliata perché si ha un modello mentale errato è un mistake (errore). → **V**

---

# Parte F – Numeri e formule da sapere

|Cosa|Valore|
|---|---|
|Tempo reale percepito|< **100 ms**; Nielsen 0,1 s / 1 s / 10 s|
|Coni blu : verdi : rossi|**1 : 2 : 4**|
|Stereo-blind|~**10%**|
|Lettura|~**250 parole/min**, fissazioni ~**94%** del tempo|
|Font ottimale|**9–12 pt** (1 pt = 1/72 pollice)|
|Lunghezza riga ottimale|**58–132 mm**|
|Schermo vs carta|~**25% più lento** (+15% fissazioni per riga); 72–300 dpi vs 1200–2400 dpi|
|Udito|**20 Hz – 15 kHz**|
|Tempi di reazione|visivo **~200 ms**, uditivo **~150 ms**, dolore **~700 ms**|
|Fitts|$Mt = a + b\log_2(D/W + 1)$|
|Memoria sensoriale|200–500 ms (iconica ~300 ms, ecoica ~2 s)|
|STM|**7 ± 2** chunk, accesso ~70 ms, decadimento ~200 ms|
|LTM|accesso ~0,1 s, capacità enorme|
|Contrasto WCAG|≥ **4.5:1**|