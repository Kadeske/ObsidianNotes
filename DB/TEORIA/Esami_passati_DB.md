# Esami passati — Basi di Dati (teoria)

> Domande ed esercizi dei vecchi esami in `DB/TEORIA/vecchi_esami`, divisi per appello.
> - Le domande **a risposta aperta** sono state trasformate in **domande a risposta multipla** (una sola risposta corretta, salvo dove indicato). Sopra ogni domanda trasformata c'è il testo originale.
> - Le domande che erano già a risposta multipla sono riportate così com'erano.
> - Gli **esercizi** (algebra relazionale, normalizzazione, progettazione) sono mantenuti come esercizi.
> - Soluzioni e spiegazioni sono nei riquadri richiudibili sotto ogni domanda.

| Appello | Contenuto | File originali |
|---|---|---|
| [[#Appello del 21 gennaio 2021\|21/01/2021]] | Domande di teoria · algebra · normalizzazione | `TemiBD1Teoria.txt`, `BD1-Esercizi-21gen2021-1.pdf` |
| [[#Prima prova parziale dell'8 novembre 2022 — Compito C\|08/11/2022 — Compito C]] | Domande · algebra · livello fisico | `primo_parziale_8_11_2022 (1).pdf` (scansione) |
| [[#Prima prova parziale dell'8 novembre 2022 — Compito D\|08/11/2022 — Compito D]] | Domande · algebra · livello fisico | `Parziale_1_BD_-_08.11.2022 (2).pdf` |
| [[#Esame totale del 22 giugno 2023\|22/06/2023]] | Domande · algebra · normalizzazione · progettazione | `Esame_Totale_BD_22_06_23 (1).pdf` (scansione) |

> [!warning] Argomenti non ancora trattati
> Alcune domande ed esercizi riguardano la **normalizzazione** (BCNF, 3NF) e la **progettazione concettuale** (modello ER), che fanno parte del programma ma non sono ancora state trattate a lezione. Sono segnalati con 🔜.

---

## Appello del 21 gennaio 2021

> File: [[DB/TEORIA/vecchi_esami/TemiBD1Teoria.txt|TemiBD1Teoria.txt]] (domande) · [[DB/TEORIA/vecchi_esami/BD1-Esercizi-21gen2021-1.pdf|BD1-Esercizi-21gen2021-1.pdf]] (esercizi)

### Domande

#### 1.
*Testo originale: "Possono esistere attributi di una tabella che non appartengono a nessuna superchiave? Motivare la risposta."*

Possono esistere attributi di una tabella che non appartengono a nessuna superchiave?
- **A)** Sì: gli attributi che non fanno parte di alcuna chiave della relazione
- **B)** Sì: gli attributi che possono assumere il valore NULL in qualche tupla
- **C)** No: l'insieme di tutti gli attributi è sempre una superchiave
- **D)** Dipende dall'istanza: succede se la relazione ha tuple quasi uguali

> [!success]- Risposta
> **C)** Una relazione non contiene tuple uguali, quindi l'insieme di **tutti** i suoi attributi distingue sempre le tuple: è una superchiave, e contiene ogni attributo. La A confonde superchiavi e chiavi: un attributo può non appartenere a nessuna **chiave**, ma appartiene sempre ad almeno una superchiave.

#### 2.
*Testo originale: "Una superchiave non minimale può essere sottoinsieme di una chiave primaria? Motivare la risposta."*

Una superchiave non minimale può essere sottoinsieme di una chiave primaria?
- **A)** Sì, se la superchiave ha meno attributi della chiave primaria
- **B)** No, perché la chiave primaria non sarebbe più minimale
- **C)** Sì, sempre: ogni superchiave è contenuta in una chiave primaria
- **D)** Solo se la chiave primaria è una chiave artificiale (surrogata)

> [!success]- Risposta
> **B)** Se una superchiave $S$ fosse contenuta nella chiave primaria $K$, allora $K$ conterrebbe una superchiave diversa da sé, contro la definizione di chiave (superchiave **minimale**). Il ragionamento vale per qualunque chiave, primaria o no.

#### 3.
*Testo originale: "Cosa si intende per livello fisico, logico ed esterno di un DBMS?"*

Quale descrizione dei tre livelli dell'architettura di un DBMS è corretta?
- **A)** Esterno: strutture di memorizzazione su disco · Logico: le viste dei singoli utenti · Fisico: l'intera base di dati
- **B)** Esterno: viste per utenti e applicazioni · Logico: l'intera base di dati · Fisico: strutture di memorizzazione
- **C)** Esterno: l'interfaccia grafica del DBMS · Logico: lo schema ER del progetto · Fisico: l'hardware del server che ospita i dati
- **D)** Esterno: l'intera base di dati · Logico: le strutture di memorizzazione · Fisico: le viste degli utenti sui file

> [!success]- Risposta
> **B)** È l'architettura ANSI/SPARC a tre livelli: lo schema esterno è una vista parziale o derivata di una parte della base di dati, quello logico descrive l'intera base di dati nel modello logico del DBMS, quello fisico rappresenta lo schema logico tramite strutture di memorizzazione. L'accesso avviene solo dal livello esterno (che può coincidere con il logico), da cui derivano l'indipendenza **fisica** e **logica**.

#### 4.
*Testo originale: "Quante chiavi diverse può avere una relazione con 2 attributi? Motivare la risposta."*

Quante chiavi diverse può avere al massimo una relazione R(A, B)?
- **A)** Esattamente una
- **B)** Al massimo 2
- **C)** 3: {A}, {B} e {A, B}
- **D)** 4

> [!success]- Risposta
> **B)** I casi possibili sono: solo {A}; solo {B}; sia {A} sia {B}; solo {A, B}. {A, B} è una chiave **solo se** né A né B da soli sono superchiavi, quindi non può coesistere con {A} o {B}: se A è chiave, {A, B} è una superchiave **non minimale**. Il massimo è quindi 2. La risposta C è un errore frequente: confonde chiavi e superchiavi.

#### 5.
*Testo originale: "Quali sono i vantaggi e gli svantaggi di un DBMS che supporta le proprietà ACID rispetto ad uno che non le supporta?"*

Quale affermazione descrive correttamente vantaggi e svantaggi di un DBMS che supporta le proprietà ACID?
- **A)** Transazioni affidabili con concorrenza e guasti, al prezzo di una minore efficienza
- **B)** Maggiore velocità rispetto ai DBMS senza ACID, al prezzo di una minore affidabilità
- **C)** Vincoli di integrità superflui, perché le transazioni garantiscono già la correttezza
- **D)** Nessuno svantaggio: anche tutti i NoSQL garantiscono ACID completo senza alcun costo

> [!success]- Risposta
> **A)** Atomicità, consistenza, isolamento e durability rendono le transazioni affidabili, ma la loro realizzazione (log, controllo della concorrenza…) pesa sulle prestazioni. Per questo molti NoSQL adottano proprietà più deboli (BASE), mentre i NewSQL cercano di conciliare ACID ed efficienza.

#### 6. 🔜
*Testo originale: "Per quale motivo ogni relazione che soddisfa la BCNF (Boyce-Codd Normal Form) soddisfa anche la 3NF (Third Normal Form)?"*

Perché ogni relazione in BCNF è anche in 3NF?
- **A)** Perché la 3NF impone condizioni più restrittive della BCNF su ogni dipendenza
- **B)** Perché la condizione della BCNF è uno dei due casi ammessi dalla 3NF
- **C)** Non è vero: esistono relazioni in BCNF che non soddisfano la 3NF
- **D)** Perché entrambe vietano qualsiasi dipendenza funzionale non banale

> [!success]- Risposta
> **B)** La 3NF richiede che, per ogni dipendenza funzionale non banale $X \to Y$, $X$ sia superchiave **oppure** ogni attributo di $Y$ appartenga a una chiave. La BCNF richiede sempre la prima condizione: la 3NF è quindi **più permissiva**, e ogni relazione in BCNF è automaticamente in 3NF (non vale il contrario).

#### 7.
*Testo originale: "Cosa si intende per formato dei record fisso e auto-descrittivo per la scrittura dei record su disco? Quali sono i vantaggi e gli svantaggi di ognuno dei due?"*

Quale affermazione sui formati dei record è corretta?
- **A)** Fisso: schema nel direttorio e solo dati nel record; auto-descrittivo: anche i metadati nel record
- **B)** Fisso: metadati in ogni record; auto-descrittivo: schema nel direttorio, solo dati nei record
- **C)** Fisso e auto-descrittivo differiscono solo per l'ordine in cui i campi sono scritti nel record
- **D)** Auto-descrittivo occupa meno spazio, perché non ripete in ogni record le informazioni sullo schema

> [!success]- Risposta
> **A)** Il formato fisso è **compatto e veloce** (gli offset dei campi sono noti) ma **rigido**; quello auto-descrittivo è **flessibile** (record con campi diversi o opzionali) ma occupa **più spazio** e richiede più elaborazione. Nell'esempio delle slide il record auto-descrittivo `2 | 5 | I | 46 | 4 | S | 4 | FORD` contiene numero di campi, codici, tipi e lunghezze. La B inverte i due formati; la D è falsa perché è il formato auto-descrittivo a ripetere lo schema in ogni record.

### Esercizi di algebra relazionale

Schema per la gestione dei provini di attori cinematografici:

- REGISTA(<u>CFregista</u>, Nome, Cognome, CasaDiProduzione)
- ATTORE(<u>CFattore</u>, Nome, Cognome, Età)
- PROVINO(<u>Regista, Attore, Data</u>) — FK: `Regista → REGISTA(CFregista)`, `Attore → ATTORE(CFattore)`

#### Esercizio 1 (3 punti)
Elencare nome e cognome dei registi che hanno fatto un provino il giorno 1/1/2021 ad attori di più di 70 anni. Disegnare anche l'albero dell'espressione.

> [!success]- Soluzione
> $$\pi_{\text{Nome},\text{Cognome}}\Big(\text{REGISTA} \bowtie_{\text{CFregista}=\text{Regista}} \big(\sigma_{\text{Data}=\text{'1/1/2021'}}(\text{PROVINO}) \bowtie_{\text{Attore}=\text{CFattore}} \pi_{\text{CFattore}}(\sigma_{\text{Età}>70}(\text{ATTORE}))\big)\Big)$$
>
> ```
>                  π Nome,Cognome
>                        │
>               ⋈ CFregista=Regista
>             ┌──────────┴──────────┐
>         REGISTA          ⋈ Attore=CFattore
>                       ┌─────────┴─────────┐
>              σ Data='1/1/2021'        π CFattore
>                       │                   │
>                   PROVINO            σ Età>70
>                                           │
>                                        ATTORE
> ```
>
> **Attenzione**: REGISTA e ATTORE hanno entrambe `Nome` e `Cognome`. Proiettando ATTORE sul solo `CFattore` prima del join si evita che nel risultato compaiano due coppie di attributi omonimi (in alternativa si può usare $\rho$).

#### Esercizio 2 (3 punti)
Elencare il CF di tutti gli attori che hanno fatto un provino per un regista della casa di produzione MGM ma per nessuno della Warner Bros. Disegnare anche l'albero dell'espressione.

> [!success]- Soluzione
> $$\pi_{\text{Attore}}\big(\text{PROVINO} \bowtie_{\text{Regista}=\text{CFregista}} \sigma_{\text{CasaDiProduzione}=\text{'MGM'}}(\text{REGISTA})\big) \;-\; \pi_{\text{Attore}}\big(\text{PROVINO} \bowtie_{\text{Regista}=\text{CFregista}} \sigma_{\text{CasaDiProduzione}=\text{'Warner Bros'}}(\text{REGISTA})\big)$$
>
> ```
>                               −
>              ┌────────────────┴────────────────┐
>          π Attore                          π Attore
>              │                                 │
>     ⋈ Regista=CFregista               ⋈ Regista=CFregista
>      ┌───────┴───────┐                 ┌───────┴───────┐
>   PROVINO   σ Casa='MGM'            PROVINO   σ Casa='Warner Bros'
>                    │                                 │
>                 REGISTA                           REGISTA
> ```
>
> Il pattern è "**ma nessuno**" → differenza. Una sola selezione con `Casa='MGM' AND Casa<>'Warner Bros'` sarebbe sbagliata: troverebbe gli attori con almeno un provino MGM, anche se ne hanno fatto un altro con la Warner.

### Esercizio di normalizzazione 🔜

#### Esercizio 3 (3 punti)
Si consideri la relazione R(A, B, C, D, E, F, G, H) con dipendenze funzionali:
$A \to CF$ · $FH \to CD$ · $BEH \to AG$ · $AH \to B$ · $ACF \to H$.
Quali di questi insiemi di attributi sono chiavi di R: {A}, {A, E}, {A, E, G}? Dimostrarlo.

> [!success]- Soluzione
> Si calcola la **chiusura** di ogni insieme, cioè tutti gli attributi che determina.
>
> **{A}⁺**: $A \to CF$ dà $\{A,C,F\}$; $ACF \to H$ dà $H$; $AH \to B$ dà $B$; $FH \to CD$ dà $D$. Si ottiene $\{A,B,C,D,F,H\}$. $BEH \to AG$ non si applica (manca E).
> → Mancano **E** e **G**: {A} **non è superchiave**, quindi non è chiave.
>
> **{A, E}⁺**: come sopra si ottiene $\{A,B,C,D,E,F,H\}$; ora $BEH \to AG$ si applica e dà $G$. Si ottengono **tutti** gli attributi: {A, E} è superchiave. È anche **minimale**: {A}⁺ non contiene E e G, e {E}⁺ = {E} (nessuna dipendenza ha solo E a sinistra).
> → {A, E} **è una chiave**.
>
> **{A, E, G}**: contiene {A, E}, quindi è superchiave ma **non è minimale**.
> → **Non è una chiave**.

---

## Prima prova parziale dell'8 novembre 2022 — Compito C

> File: [[DB/TEORIA/vecchi_esami/primo_parziale_8_11_2022 (1).pdf|primo_parziale_8_11_2022 (1).pdf]] (scansione di un compito svolto; le risposte dello studente non sono riportate). Nella scansione le parti di algebra e livello fisico sono intestate "Compito D", ma le domande sono diverse da quelle del Compito D riportato più sotto.

### Parte 1 — Domande

#### 1. Nell'ambito del modello relazionale, data una relazione, esiste sempre una superchiave?
- **(a)** In generale no
- **(b)** Esiste una superchiave solo se non esistono chiavi
- **(c)** Sì, sempre
- **(d)** Sì, purché almeno un attributo ammetta valori nulli

> [!success]- Risposta
> **(c)** L'insieme di tutti gli attributi è sempre una superchiave, perché una relazione non contiene tuple uguali. La (b) è assurda: ogni chiave è anche una superchiave.

#### 2. Quale di queste sequenze di attività descrive meglio la realizzazione di un database?
- **(a)** Si definisce prima lo schema logico, poi quello concettuale, poi quello esterno
- **(b)** Si definisce prima lo schema concettuale, poi quello logico, poi quello esterno
- **(c)** Si definisce prima lo schema logico, poi quello esterno, poi quello concettuale
- **(d)** Si definisce prima lo schema esterno, poi quello logico, poi quello concettuale

> [!success]- Risposta
> **(b)** Il progettista parte dallo schema **concettuale** (es. ER, indipendente dal DBMS), lo traduce nello schema **logico** (tabelle) e su quest'ultimo definisce gli schemi **esterni** (viste).

#### 3. A cosa serve un Data Definition Language (DDL)?
- **(a)** Per l'interrogazione e l'aggiornamento di istanze nella base di dati
- **(b)** Per la definizione e la modifica di schemi (logici, esterni, fisici)
- **(c)** Per la definizione ma non per la modifica di schemi (logici, esterni, fisici)
- **(d)** Per l'interrogazione ma non per l'aggiornamento di istanze nella base di dati

> [!success]- Risposta
> **(b)** Il DDL (`CREATE`, `ALTER`, `DROP`) definisce **e modifica** gli schemi. La (a) descrive il DML.

#### 4. Nel livello fisico del DBMS, qual è il vantaggio dell'organizzazione disordinata dei record su disco rispetto a quella ordinata?
- **(a)** L'organizzazione disordinata è più efficiente per ricerche su range di valori
- **(b)** L'organizzazione disordinata è più efficiente in caso di ricerche sul valore della chiave
- **(c)** L'organizzazione disordinata non presenta vantaggi rispetto a quella ordinata
- **(d)** L'organizzazione disordinata è più efficiente in caso di inserimenti, modifiche e cancellazioni

> [!success]- Risposta
> **(d)** In un file disordinato si inserisce in coda o al posto di un record cancellato, senza dover mantenere l'ordine. Per le ricerche (a, b) è invece più efficiente l'organizzazione ordinata.

#### 5.
*Testo originale: "Spiegare cosa si intende per indipendenza logica e fisica dei dati."*

Quale coppia di definizioni di indipendenza fisica e logica dei dati è corretta?
- **A)** Fisica: l'esterno non dipende dal logico · Logica: logico ed esterno non dipendono dal fisico
- **B)** Fisica: i programmi non dipendono dall'hardware · Logica: i programmi non dipendono da SQL
- **C)** Fisica: logico ed esterno non dipendono dal fisico · Logica: l'esterno non dipende dal logico
- **D)** Fisica: le viste non dipendono dalle tabelle · Logica: le tabelle non dipendono dagli indici

> [!success]- Risposta
> **C)** Con l'indipendenza fisica la realizzazione fisica (file, indici…) può cambiare senza modificare i programmi; con l'indipendenza logica le modifiche allo schema logico che lasciano invariato lo schema esterno sono trasparenti. La A inverte le due definizioni. Entrambe derivano dall'architettura a tre livelli: l'utente accede ai dati solo tramite il livello esterno.

#### 6.
*Testo originale: "Spiegare le analogie e le differenze tra la relazione matematica e la relazione del modello relazionale di una base di dati."*

Quale affermazione descrive correttamente analogie e differenze tra la relazione matematica e la relazione del modello relazionale?
- **A)** Entrambe sono insiemi di tuple; la matematica è posizionale, l'altra usa nomi di attributo
- **B)** La matematica ammette tuple duplicate, mentre quella del modello relazionale non le ammette
- **C)** Nel modello relazionale conta l'ordine delle colonne, nella relazione matematica invece no
- **D)** Nessuna differenza: sono due nomi diversi per lo stesso concetto, con le stesse proprietà

> [!success]- Risposta
> **A)** Entrambe sono insiemi di tuple distinte, senza ordine tra le tuple. Nella relazione matematica il ruolo di un valore dipende dalla posizione (in `Partite ⊆ String × String × Int × Int` la prima stringa è la squadra di casa); nel modello relazionale lo dice il nome dell'attributo (`Casa`, `Fuori`, …), quindi anche l'ordine delle colonne è irrilevante.

#### 7.
*Testo originale: "Qual è la semantica del valore NULL? Cosa succede se in una selezione si applica una condizione (ad esempio `SELECT … WHERE Stipendio > 50000`) su un attributo (ad es. `Stipendio`) che ha valore NULL?"*

**7a.** Che cosa rappresenta il valore `NULL`?
- **A)** Il valore 0 per i numeri e la stringa vuota per i testi
- **B)** L'assenza di un valore: inesistente oppure sconosciuto
- **C)** Un valore speciale del dominio, uguale a ogni altro NULL
- **D)** Un errore nei dati, che il DBMS segnala all'utente

> [!success]- Risposta
> **B)** `NULL` non è un valore del dominio: indica che il valore **manca**. Usare valori come 0 o la stringa vuota (A) è sconsigliato, perché potrebbero diventare significativi e andrebbero interpretati in ogni programma.

**7b.** Che cosa succede alle righe con `Stipendio = NULL` in una query con `WHERE Stipendio > 50000`?
- **A)** Vengono restituite, perché NULL soddisfa ogni condizione
- **B)** La query si interrompe con un errore di tipo sul confronto
- **C)** Vengono trattate come se il loro stipendio valesse zero
- **D)** Non vengono restituite: il confronto con NULL non è vero

> [!success]- Risposta
> **D)** Una condizione atomica è vera solo per valori non nulli. Per includere quelle righe bisogna aggiungere esplicitamente `OR Stipendio IS NULL`.

### Parte 2 — Algebra relazionale

#### Quiz (2 punti) — Considerare la relazione R(A, B, C). Da quali tuple è composta la relazione $\text{SEL}_{A>B}(R)$?
- **(a)** Le tuple di R in cui il valore di A è maggiore di quello di B, più quelle per cui A è NULL ma B è diverso da NULL
- **(b)** Le tuple di R in cui il valore di A è maggiore di quello di B, più quelle per cui B è NULL ma A è diverso da NULL
- **(c)** Le tuple di R in cui il valore di A è maggiore di quello di B, più quelle per cui A è NULL oppure B è NULL (o entrambi sono NULL)
- **(d)** Le tuple di R in cui il valore di A e di B è diverso da NULL e il valore di A è maggiore di quello di B

> [!success]- Risposta
> **(d)** Se A o B è `NULL` il confronto `A > B` non è vero e la tupla viene scartata.

#### Esercizi
Schema per la gestione di case costruttrici, automobili e proprietari (nessun campo può essere `NULL`):

- PERSONA(<u>CodiceFiscale</u>, Nome, Cognome, Età)
- CASACOSTRUTTRICE(<u>Codice</u>, NomeCasa, Nazione)
- MODELLOAUTO(<u>CodiceCasa, NomeModello</u>, Categoria) — FK: `CodiceCasa → CASACOSTRUTTRICE`
- AUTOMOBILE(<u>Targa</u>, Casa, Modello, Anno, Proprietario) — FK: `Casa → CASACOSTRUTTRICE`, `<Casa, Modello> → MODELLOAUTO`, `Proprietario → PERSONA`

##### Esercizio 2 (3 punti)
Elencare la targa delle auto di categoria 'SUV'.

> [!success]- Soluzione
> $$\pi_{\text{Targa}}\big(\text{AUTOMOBILE} \bowtie_{\text{Casa}=\text{CodiceCasa} \,\wedge\, \text{Modello}=\text{NomeModello}} \sigma_{\text{Categoria}=\text{'SUV'}}(\text{MODELLOAUTO})\big)$$
>
> **Attenzione**: la chiave di MODELLOAUTO è **composta** (`CodiceCasa`, `NomeModello`), quindi il join va fatto su **entrambi** gli attributi. Confrontando solo `Modello = NomeModello`, due case con un modello omonimo verrebbero confuse.

##### Esercizio 3 (4 punti)
Elencare nome e cognome dei diciottenni che posseggono un'auto di categoria 'SUV'. Disegnare anche l'albero dell'espressione.

> [!success]- Soluzione
> $$\pi_{\text{Nome},\text{Cognome}}\Big(\sigma_{\text{Età}=18}(\text{PERSONA}) \bowtie_{\text{CodiceFiscale}=\text{Proprietario}} \big(\text{AUTOMOBILE} \bowtie_{\text{Casa}=\text{CodiceCasa} \,\wedge\, \text{Modello}=\text{NomeModello}} \sigma_{\text{Categoria}=\text{'SUV'}}(\text{MODELLOAUTO})\big)\Big)$$
>
> ```
>                         π Nome,Cognome
>                               │
>                 ⋈ CodiceFiscale=Proprietario
>               ┌───────────────┴───────────────┐
>          σ Età=18          ⋈ Casa=CodiceCasa ∧ Modello=NomeModello
>               │                ┌──────────────┴──────────────┐
>           PERSONA         AUTOMOBILE               σ Categoria='SUV'
>                                                            │
>                                                       MODELLOAUTO
> ```
>
> "Diciottenni" significa `Età = 18`, non `Età ≥ 18` (maggiorenni).

##### Esercizio 4 (3 punti)
Elencare il codice fiscale delle persone che posseggono sia una 'BMW' sia una 'KIA' (devono possederle entrambe, non solo una delle due). 'BMW' e 'KIA' sono valori dell'attributo `NomeCasa` di CASACOSTRUTTRICE. Disegnare anche l'albero dell'espressione.

> [!success]- Soluzione
> $$\pi_{\text{Proprietario}}\big(\text{AUTOMOBILE} \bowtie_{\text{Casa}=\text{Codice}} \sigma_{\text{NomeCasa}=\text{'BMW'}}(\text{CASACOSTRUTTRICE})\big) \;\cap\; \pi_{\text{Proprietario}}\big(\text{AUTOMOBILE} \bowtie_{\text{Casa}=\text{Codice}} \sigma_{\text{NomeCasa}=\text{'KIA'}}(\text{CASACOSTRUTTRICE})\big)$$
>
> ```
>                                 ∩
>               ┌─────────────────┴─────────────────┐
>        π Proprietario                      π Proprietario
>               │                                   │
>        ⋈ Casa=Codice                       ⋈ Casa=Codice
>        ┌──────┴──────┐                     ┌──────┴──────┐
>   AUTOMOBILE  σ NomeCasa='BMW'        AUTOMOBILE  σ NomeCasa='KIA'
>                      │                                   │
>              CASACOSTRUTTRICE                    CASACOSTRUTTRICE
> ```
>
> **Trappole**: $\sigma_{\text{NomeCasa}=\text{'BMW'} \,\wedge\, \text{NomeCasa}=\text{'KIA'}}$ dà un risultato **vuoto** (una tupla non può avere due valori), mentre con $\vee$ si troverebbero le persone che hanno **almeno una** delle due. Serve l'intersezione di due insiemi di proprietari (in alternativa, un self-join su AUTOMOBILE).

### Parte 3 — Livello fisico del DBMS

#### 1.
*Testo originale: "Spiegare brevemente il funzionamento di un indice Hash. Quali vantaggi ha un indice Hash rispetto ad un B Tree? In cosa consiste il problema delle collisioni e come si può risolvere?"*

**1a.** Come funziona un indice hash e qual è il suo vantaggio rispetto a un B-tree?
- **A)** Ordina le chiavi in un albero bilanciato; è ottimo per le ricerche su intervalli
- **B)** Calcola dalla chiave l'indirizzo del blocco; ~1 accesso per ricerche di uguaglianza
- **C)** Usa una sequenza di bit per ogni valore; è adatto a colonne con pochi valori distinti
- **D)** Tiene i record ordinati su disco; la ricerca binaria non richiede strutture aggiuntive

> [!success]- Risposta
> **B)** Una funzione hash calcola dal valore della chiave l'indirizzo del blocco: per le ricerche di **uguaglianza** costa circa 1 accesso, contro profondità + 1 di un B-tree. Lo svantaggio è che l'hash **non** serve per le ricerche su intervalli, dove il B-tree (A) è migliore. La C descrive l'indice bitmap.

**1b.** In che cosa consiste il problema delle collisioni e come si affronta?
- **A)** Due transazioni modificano lo stesso record; si risolvono con i lock sulle righe
- **B)** La funzione hash è iniettiva; si risolvono sostituendola con una non iniettiva
- **C)** Chiavi diverse finiscono nello stesso indirizzo; si usano overflow o posizioni libere
- **D)** Chiavi uguali finiscono in indirizzi diversi; si risolve riordinando il file sulla chiave hash

> [!success]- Risposta
> **C)** La funzione hash $h: C \to I$ con $|I| \ll |C|$ non può essere iniettiva. Le collisioni si **riducono** con una buona funzione hash e con spazio ridondante, e si **gestiscono** con posizioni successive o aree di overflow. Raggruppare le posizioni in blocchi (fattore di blocco) riduce ulteriormente gli overflow.

#### 2.
*Testo originale: "Nella scrittura dei record su file system, un DBMS può usare una tecnica spanned oppure unspanned. Spiegare cosa si intende per tecnica spanned e unspanned, e discutere vantaggi e svantaggi di entrambe."*

Quale affermazione su record spanned e unspanned è corretta?
- **A)** Unspanned: record in un solo blocco, semplice ma spreca spazio; spanned: record su più blocchi
- **B)** Spanned: record in un solo blocco, semplice ma spreca spazio; unspanned: record su più blocchi
- **C)** Unspanned è necessario per i record più grandi di un blocco, spanned per quelli più piccoli
- **D)** Spanned e unspanned sono sinonimi di formato dei record fisso e formato auto-descrittivo

> [!success]- Risposta
> **A)** Con spanned un record può essere diviso su più blocchi: sfrutta meglio lo spazio ed è **necessario** per i record più grandi di un blocco, ma è più complesso. La B inverte le definizioni; la C è falsa perché un record più grande di un blocco non può stare in un solo blocco.

#### 3.
*Testo originale: "In base a cosa si calcola il costo di una operazione su un DBMS? Nell'architettura di un DBMS, quale processo di ottimizzazione attua l'ottimizzatore delle query? Spiegare cosa si intende per euristica 'Push selections down' e 'Push projections down', e perché queste euristiche possono diminuire il costo di esecuzione di una query."*

**3a.** In base a cosa si calcola il costo di un'operazione su un DBMS?
- **A)** Dal numero di istruzioni macchina eseguite dalla CPU
- **B)** Dal numero di blocchi letti o scritti su disco
- **C)** Dal numero di tuple restituite dalla query
- **D)** Dalla lunghezza del testo della query SQL

> [!success]- Risposta
> **B)** Un accesso alla memoria secondaria costa quattro o più ordini di grandezza più di un'operazione in memoria centrale, quindi si contano solo i blocchi letti o scritti.

**3b.** Che cosa sono le euristiche *push selections down* e *push projections down*?
- **A)** Eseguire selezioni e proiezioni per ultime, vicino alla radice dell'albero
- **B)** Eliminare dalla query le selezioni e le proiezioni che risultano superflue
- **C)** Eseguire selezioni e proiezioni il prima possibile, vicino alle foglie
- **D)** Trasformare selezioni e proiezioni in join, che il DBMS esegue più velocemente

> [!success]- Risposta
> **C)** Fanno parte dell'**ottimizzazione algebrica**: la query viene sostituita da un'espressione **equivalente** che produce risultati intermedi più piccoli. Ad esempio $\sigma_{A=10}(R_1 \bowtie R_2)$ diventa $R_1 \bowtie \sigma_{A=10}(R_2)$: il join lavora su meno tuple (e, con la proiezione anticipata, su tuple più piccole), quindi legge e scrive meno blocchi.

---

## Prima prova parziale dell'8 novembre 2022 — Compito D

> File: [[DB/TEORIA/vecchi_esami/Parziale_1_BD_-_08.11.2022 (2).pdf|Parziale_1_BD_-_08.11.2022 (2).pdf]]

### Parte 1 — Domande

#### 1. Nell'ambito del modello relazionale, quale dei seguenti tipi di vincoli necessita tipicamente, per la sua verifica, di un controllo su più relazioni?
- **(a)** Integrità referenziale
- **(b)** Chiave
- **(c)** Superchiave
- **(d)** Nessuno dei precedenti

> [!success]- Risposta
> **(a)** È un vincolo **inter-relazionale**: bisogna controllare che i valori della chiave esterna compaiano nella chiave primaria di un'altra relazione. I vincoli di chiave e superchiave si verificano su una sola relazione.

#### 2. A cosa serve un Data Manipulation Language (DML)?
- **(a)** Per l'aggiornamento ma non per l'interrogazione di istanze nella base di dati
- **(b)** Per la definizione e la modifica di schemi (logici, esterni, fisici)
- **(c)** Per la definizione ma non per la modifica di schemi (logici, esterni, fisici)
- **(d)** Per l'interrogazione e l'aggiornamento di istanze nella base di dati

> [!success]- Risposta
> **(d)** `SELECT`, `INSERT`, `UPDATE`, `DELETE`. La (b) descrive il DDL.

#### 3. Cosa si intende dicendo che "il modello relazionale è basato sui valori"?
- **(a)** Che il modello relazionale non ammette valori nulli per le superchiavi
- **(b)** Che il modello relazionale è orientato a memorizzare valori complessi e non dati binari
- **(c)** Che il livello fisico si basa sull'uso di puntatori fisici per indicizzare i valori delle relazioni
- **(d)** Che i riferimenti tra tuple contenenti informazioni che si riferiscono allo stesso individuo sono realizzati in base ai valori di uno o più attributi

> [!success]- Risposta
> **(d)** A differenza dei modelli gerarchico e reticolare, che usano puntatori espliciti, il relazionale collega i dati tramite valori comuni (es. `ESAMI.Studente` = `STUDENTI.Matricola`).

#### 4. In quali casi la modalità di memorizzazione dei record "Column Store" è preferibile a "Row Store"?
- **(a)** La seconda è sempre preferibile alla prima perché determina minor consumo di spazio
- **(b)** La prima è sempre preferibile alla seconda perché più semplice da implementare
- **(c)** Quando si devono fare frequentemente query che coinvolgono un'unica colonna delle tuple della tabella
- **(d)** Quando si devono fare frequentemente query che coinvolgono tutte le colonne delle tuple

> [!success]- Risposta
> **(c)** Il column store legge solo le colonne necessarie (tipico di statistiche e data mining). Per query su tutte le colonne di un record (d) è preferibile il row store.

#### 5.
*Testo originale: "Cosa si intende per 'schema esterno' di un DBMS? Per quali motivi è utile in alcuni casi creare delle viste? Che ruolo hanno le viste riguardo alle garanzie di privatezza che un DBMS deve garantire?"*

**5a.** Che cos'è lo schema esterno e perché è utile creare viste?
- **A)** La rappresentazione dei dati su disco; le viste velocizzano le scritture
- **B)** Una vista parziale o derivata dei dati; le viste semplificano le query
- **C)** Lo schema concettuale ER; le viste servono a definire le chiavi primarie
- **D)** Una copia di backup del database; le viste servono al ripristino dai guasti

> [!success]- Risposta
> **B)** Lo schema esterno è una vista parziale o derivata di una parte della base di dati. Le viste mostrano a ogni utente solo ciò che gli interessa, semplificano la scrittura delle interrogazioni e permettono di riusare programmi esistenti su schemi ristrutturati. Sono relazioni **derivate**: le query sulle viste si eseguono sostituendo la loro definizione.

**5b.** Che ruolo hanno le viste nella privatezza dei dati?
- **A)** Cifrano i dati della tabella di base, rendendoli leggibili solo agli autorizzati
- **B)** Nessuno: la privatezza si gestisce solo con i permessi sui file a livello fisico
- **C)** Impediscono a chiunque, amministratore compreso, l'accesso alla tabella di base
- **D)** Si concedono i privilegi sulla vista, che mostra solo le righe autorizzate

> [!success]- Risposta
> **D)** È il modo standard per limitare la **visibilità** a un sottoinsieme delle tuple (e delle colonne): l'utente non ha privilegi sulla tabella di base, solo sulla vista.

#### 6.
*Testo originale: "Nel modello relazionale delle basi di dati, data una qualunque relazione R, è sempre vero che R ha almeno una chiave S? Se sì, dimostrarlo. Se no, mostrare l'esempio di un caso in cui R non abbia chiavi."*

Ogni relazione R ha almeno una chiave?
- **A)** No: una relazione che contiene tuple duplicate non ha alcuna chiave
- **B)** Sì: l'insieme di tutti gli attributi è superchiave, e contiene una chiave
- **C)** No: se tutti gli attributi ammettono NULL non può esistere una chiave
- **D)** Sì: il DBMS aggiunge automaticamente una chiave artificiale a ogni tabella

> [!success]- Risposta
> **B)** È il teorema di esistenza delle chiavi: R è un insieme, quindi non ha tuple uguali; perciò l'insieme di tutti i suoi attributi è una superchiave, e togliendo attributi finché resta superchiave si ottiene una chiave. La A parte da un'ipotesi impossibile: una relazione non può contenere tuple duplicate.

#### 7.
*Testo originale: "Quali sono i principali ruoli dei professionisti che lavorano nell'ambito delle basi di dati, e quali compiti svolgono?"*

Quale associazione tra ruolo e compiti è corretta?
- **A)** Il DBA scrive e mantiene il codice sorgente del software DBMS
- **B)** Gli utenti finali gestiscono autorizzazioni e prestazioni del sistema
- **C)** Il DBA gestisce risorse, prestazioni, affidabilità e autorizzazioni
- **D)** Gli sviluppatori di applicazioni progettano e realizzano il DBMS

> [!success]- Risposta
> **C)** Il DBA (*Database Administrator*) è responsabile del controllo centralizzato del sistema, e spesso anche della progettazione della base di dati. I ruoli visti a lezione sono: progettisti e sviluppatori del **DBMS** (il software); progettisti e **amministratori della base di dati**; analisti, progettisti e sviluppatori di **applicazioni**; **utenti**. A e D attribuiscono a un ruolo i compiti dei progettisti del DBMS.

### Parte 2 — Algebra relazionale

#### Quiz (2 punti) — Si considerino due attributi A e B con domini $D_A = \{x, y, z\}$ e $D_B = \{5, 12\}$. Quale tra le seguenti è una relazione su (A, B)?
- **(a)** $\{(z, 5), (z, 5)\}$
- **(b)** $\{(5, z), (x, 12)\}$
- **(c)** $\{(z, 5), (x, 12)\}$
- **(d)** $\{(x, 10), (z, 5)\}$

> [!success]- Risposta
> **(c)** Ogni tupla ha il primo valore in $D_A$ e il secondo in $D_B$. La (a) elenca due volte la stessa tupla, cosa non ammessa in una relazione; la (b) ha una tupla con i valori in posizione invertita (la struttura è posizionale); nella (d) il valore 10 non appartiene a $D_B$.

#### Esercizi
Stesso schema del Compito C: PERSONA(<u>CodiceFiscale</u>, Nome, Cognome, Età), CASACOSTRUTTRICE(<u>Codice</u>, NomeCasa, Nazione), MODELLOAUTO(<u>CodiceCasa, NomeModello</u>, Categoria), AUTOMOBILE(<u>Targa</u>, Casa, Modello, Anno, Proprietario), con gli stessi vincoli referenziali e nessun valore `NULL`.

(Nel testo originale l'ultimo attributo di AUTOMOBILE compare come "Proprietà" e la relazione riferita come "CASACOSTRUZIONE": sono refusi per `Proprietario` e CASACOSTRUTTRICE.)

##### Esercizio 2 (3 punti)
Elencare la targa e l'anno delle auto possedute da persone con meno di 20 anni.

> [!success]- Soluzione
> $$\pi_{\text{Targa},\text{Anno}}\big(\text{AUTOMOBILE} \bowtie_{\text{Proprietario}=\text{CodiceFiscale}} \sigma_{\text{Età}<20}(\text{PERSONA})\big)$$

##### Esercizio 3 (4 punti)
Elencare nome e cognome dei proprietari di auto prodotte da case costruttrici tedesche. Disegnare anche l'albero dell'espressione.

> [!success]- Soluzione
> $$\pi_{\text{Nome},\text{Cognome}}\Big(\text{PERSONA} \bowtie_{\text{CodiceFiscale}=\text{Proprietario}} \big(\text{AUTOMOBILE} \bowtie_{\text{Casa}=\text{Codice}} \sigma_{\text{Nazione}=\text{'Germania'}}(\text{CASACOSTRUTTRICE})\big)\Big)$$
>
> ```
>                    π Nome,Cognome
>                          │
>           ⋈ CodiceFiscale=Proprietario
>            ┌─────────────┴─────────────┐
>        PERSONA                  ⋈ Casa=Codice
>                           ┌──────────┴──────────┐
>                      AUTOMOBILE        σ Nazione='Germania'
>                                                  │
>                                          CASACOSTRUTTRICE
> ```
>
> MODELLOAUTO non serve: la casa costruttrice è già un attributo di AUTOMOBILE. Conviene usare il numero minimo di relazioni.

##### Esercizio 4 (3 punti)
Elencare nome e cognome delle persone che non posseggono nessuna automobile. Disegnare anche l'albero dell'espressione.

> [!success]- Soluzione
> $$\pi_{\text{Nome},\text{Cognome}}\Big(\text{PERSONA} \bowtie \big(\pi_{\text{CodiceFiscale}}(\text{PERSONA}) - \rho_{\text{CodiceFiscale} \leftarrow \text{Proprietario}}(\pi_{\text{Proprietario}}(\text{AUTOMOBILE}))\big)\Big)$$
>
> ```
>                      π Nome,Cognome
>                            │
>                ⋈ (naturale su CodiceFiscale)
>            ┌───────────────┴───────────────┐
>        PERSONA                             −
>                          ┌─────────────────┴─────────────────┐
>                  π CodiceFiscale           ρ CodiceFiscale ← Proprietario
>                          │                                   │
>                      PERSONA                          π Proprietario
>                                                              │
>                                                         AUTOMOBILE
> ```
>
> La differenza va fatta sui **codici fiscali** e non su nome e cognome: due omonimi verrebbero confusi (se uno dei due ha un'auto, l'altro sparirebbe dal risultato). Dopo la differenza si torna in join con PERSONA per recuperare nome e cognome.

### Parte 3 — Livello fisico del DBMS

#### 1.
*Testo originale: "Spiegare qual è la struttura di un indice Bitmap: rappresentandolo in forma tabellare, a cosa corrispondono le righe e le colonne, e quali valori assume ogni cella? Quali vantaggi ha un indice Bitmap rispetto ad un B Tree? Per quale motivo un indice Bitmap non sarebbe adatto per indicizzare una colonna che è chiave della tabella?"*

**1a.** Rappresentando un indice bitmap in forma tabellare, a cosa corrispondono righe, colonne e celle?
- **A)** Righe: blocchi del disco · colonne: chiavi della tabella · celle: puntatori ai record
- **B)** Righe: tuple della tabella · colonne: valori distinti della colonna · celle: 0 o 1
- **C)** Righe: valori distinti · colonne: pagine del buffer · celle: contatori di utilizzo
- **D)** Righe e colonne: tuple della tabella · celle: distanza tra le due tuple

> [!success]- Risposta
> **B)** Una cella vale 1 se la tupla ha quel valore, 0 altrimenti. Ad esempio, per `Stato_Civile` ci sono le colonne *Single* e *Married* e ogni riga ha un solo 1.

**1b.** Quale vantaggio ha un indice bitmap rispetto a un B-tree?
- **A)** È più efficiente del B-tree per le ricerche su intervalli di valori
- **B)** Regge meglio del B-tree tabelle con molti aggiornamenti concorrenti
- **C)** Funziona meglio del B-tree su colonne con valori tutti diversi tra loro
- **D)** Combina condizioni su più colonne con AND/OR bit a bit, molto veloci

> [!success]- Risposta
> **D)** Es. "uomini anziani non sposati" = AND dei bitmap *Male*, *65+* e NOT *Married*; l'indice è anche molto compatto. La B è falsa: gli aggiornamenti concorrenti sono proprio il punto debole del bitmap.

**1c.** Perché un indice bitmap non è adatto a una colonna che è chiave della tabella?
- **A)** Perché le colonne chiave non possono essere indicizzate in nessun modo
- **B)** Perché servirebbe una colonna di bit per ogni tupla: indice enorme
- **C)** Perché le colonne chiave possono contenere valori nulli in alcune tuple
- **D)** Perché un indice bitmap funziona solo su colonne di tipo numerico intero

> [!success]- Risposta
> **B)** In una chiave tutti i valori sono distinti e il bitmap ha una colonna per ogni valore distinto: servirebbero $n \times n$ bit, quasi tutti a zero. Per una chiave conviene un B+ tree (o un hash).

#### 2.
*Testo originale: "Nella scrittura dei record su file system, un DBMS può usare un formato dei record fisso oppure auto-descrittivo. Spiegare cosa si intende per formato dei record fisso e auto-descrittivo, e discutere vantaggi e svantaggi di entrambi."*

Quale affermazione sui formati dei record fisso e auto-descrittivo è corretta?
- **A)** Il formato fisso memorizza in ogni record anche nomi e tipi dei campi
- **B)** Il formato auto-descrittivo è più compatto, perché non ripete lo schema
- **C)** Il fisso è compatto ma rigido; l'auto-descrittivo flessibile ma più pesante
- **D)** I due formati differiscono soltanto per l'ordine in cui sono scritti i campi

> [!success]- Risposta
> **C)** Il formato fisso tiene lo schema nel direttorio e nei record solo i dati; quello auto-descrittivo memorizza nei record anche i metadati, quindi occupa più spazio e richiede più elaborazione. A e B attribuiscono a un formato le caratteristiche dell'altro. È la stessa domanda dell'appello del 21/01/2021.

#### 3.
*Testo originale: "È sempre utile creare un indice su una colonna (o su più colonne) del DBMS? In quali casi in genere si ritiene utile indicizzare una colonna? Perché non avrebbe senso indicizzare una colonna di una tabella di cui si vuole normalmente recuperare tramite query più della metà delle righe?"*

**3a.** È sempre utile creare un indice? In quali casi conviene?
- **A)** Sì: conviene sempre indicizzare tutte le colonne di tutte le tabelle
- **B)** No: conviene se si recupera spesso una piccola parte delle righe
- **C)** No: gli indici servono esclusivamente per le chiavi primarie
- **D)** Sì: gli indici non occupano spazio e non rallentano le modifiche

> [!success]- Risposta
> **B)** Un indice occupa spazio e rallenta inserimenti e modifiche. Le linee guida viste a lezione: indicizzare se si recupera spesso meno del 15% circa delle righe di una grande tabella, sulle colonne usate nei join e su colonne con valori relativamente univoci. Chiavi primarie e campi `UNIQUE` hanno già un indice automatico; può essere utile aggiungerne sulle chiavi esterne.

**3b.** Perché non ha senso indicizzare una colonna se le query recuperano di solito più della metà delle righe?
- **A)** Perché l'indice restituirebbe risultati errati quando le righe sono tante
- **B)** Perché il DBMS non permette indici su tabelle molto grandi
- **C)** Perché si leggerebbero comunque quasi tutti i blocchi, più l'indice
- **D)** Perché l'indice verrebbe cancellato automaticamente dal DBMS

> [!success]- Risposta
> **C)** Le righe da leggere sono sparse in quasi tutti i blocchi: con l'indice si leggerebbero comunque quasi tutti i blocchi dei dati, più quelli dell'indice e in ordine sparso, mentre una scansione sequenziale legge ogni blocco una sola volta. Il costo si misura in blocchi letti.

---

## Esame totale del 22 giugno 2023

> File: [[DB/TEORIA/vecchi_esami/Esame_Totale_BD_22_06_23 (1).pdf|Esame_Totale_BD_22_06_23 (1).pdf]] (scansione; alcune parti sono tagliate nell'originale)

### Domande

#### 1. Contrassegnare le affermazioni corrette (possono essere più di una) — 3 punti
- **(a)** Quello relazionale si dice un modello basato sui puntatori, in quanto i riferimenti fra dati in relazioni diverse sono rappresentati per mezzo di puntatori numerici che collegano le tuple
- **(b)** L'indipendenza dei dati permette di scrivere programmi conoscendo solo lo schema concettuale della base di dati
- **(c)** Nell'ambito del modello relazionale, data una relazione, esiste sempre una e una sola superchiave minimale
- **(d)** Una superchiave non minimale può essere costituita da un singolo attributo
- **(e)** Il modello relazionale è un modello sia logico che concettuale
- **(f)** Nell'ambito del modello relazionale, data una relazione, esiste sempre una chiave composta da un singolo attributo
- **(g)** Ogni relazione priva di vincoli di integrità referenziale soddisfa necessariamente la BCNF (Boyce-Codd Normal Form)
- **(h)** Il modello entità-relazione (ER) è un modello concettuale
- **(i)** I valori nulli non sono mai ammessi per gli attributi che formano una superchiave
- **(j)** Il linguaggio SQL include sia istruzioni Data Definition Language (DDL) sia istruzioni Data Manipulation Language (DML)
- **(k)** Lo schema esterno è una "vista" parziale o derivata di una parte della base di dati in un modello logico

> [!success]- Risposta
> Corrette: **(h), (j), (k)**.
> - (a) Falsa: il modello relazionale è **basato sui valori**.
> - (b) Falsa: i programmi usano lo schema logico o esterno, non quello concettuale.
> - (c) Falsa: le superchiavi minimali sono le chiavi, e possono essere più d'una.
> - (d) Falsa: una superchiave con un solo attributo è sempre minimale.
> - (e) Falsa: è un modello **logico**; l'ER è il modello concettuale.
> - (f) Falsa: la chiave può essere composta (es. `<Studente, Corso>`).
> - (g) 🔜 Falsa: la BCNF dipende dalle **dipendenze funzionali**, non dai vincoli referenziali.
> - (i) Falsa: il divieto di `NULL` vale per la **chiave primaria**. Una superchiave può contenere attributi non chiave che ammettono `NULL` (es. `<Matricola, Telefono>`).

#### 2.
*Testo originale: "Quali sono le differenze tra 'relazione matematica' e 'relazione secondo il modello relazionale dei dati'?"*

Qual è la differenza principale tra relazione matematica e relazione del modello relazionale dei dati?
- **A)** La matematica è un insieme di tuple, quella del modello è una lista ordinata
- **B)** Nella matematica conta la posizione, nel modello ogni dominio ha un nome
- **C)** Solo la relazione matematica può contenere valori nulli nelle sue tuple
- **D)** Nel modello relazionale le tuple possono ripetersi, in quella matematica no

> [!success]- Risposta
> **B)** Sono entrambe insiemi di tuple distinte senza ordine tra le tuple (contro A e D). Nel modello relazionale ogni dominio ha un nome (attributo) che ne descrive il ruolo, quindi la struttura non è posizionale ed è irrilevante anche l'ordine delle colonne.

#### 3.
*Testo originale: "Cos'è il Buffer manager di un DBMS, e quali compiti svolge?"*

**3a.** Che cos'è il buffer manager?
- **A)** Il modulo che traduce le query SQL in espressioni di algebra relazionale
- **B)** Il modulo che gestisce il buffer in RAM per ridurre gli accessi al disco
- **C)** Il modulo che controlla le autorizzazioni e i privilegi degli utenti
- **D)** Il file su cui si registrano le operazioni per il ripristino dopo un guasto

> [!success]- Risposta
> **B)** Il buffer è un'area di memoria centrale condivisa tra le transazioni. Il buffer manager riceve le richieste di lettura e scrittura dei blocchi, usa il buffer quando possibile e accede al disco solo quando indispensabile. La D descrive il log.

**3b.** Quale primitiva del buffer manager scrive una pagina su disco in modo **sincrono**?
- **A)** `fix`
- **B)** `setDirty`
- **C)** `unfix`
- **D)** `force`

> [!success]- Risposta
> **D)** `fix` richiede una pagina (legge da disco solo se non è nel buffer), `setDirty` la segna come modificata, `unfix` la rilascia; `flush` scrive in modo asincrono.

#### 4.
*Testo originale: "Quale tipo di indice conviene creare su una colonna che viene spesso interrogata con delle range query (es. 'restituisci tutte le matricole da 1000 a 2000')? Motivare la risposta."*

Quale indice conviene per le range query?
- **A)** Un indice hash, perché ogni ricerca costa circa un solo accesso
- **B)** Un indice bitmap, perché la colonna ha molti valori diversi
- **C)** Un B+ tree, perché le foglie collegate si scorrono in ordine
- **D)** Nessun indice, perché le range query non possono usare indici

> [!success]- Risposta
> **C)** Si trova la prima chiave dell'intervallo scendendo nell'albero e poi si scorrono le foglie collegate fino all'ultima. L'hash (A) disperde valori vicini in blocchi diversi ed è adatto solo all'uguaglianza; il bitmap (B) è adatto a colonne con **pochi** valori distinti.

#### 5.
*Testo originale: "Quali algoritmi si possono usare per effettuare un join? Scegliere uno degli algoritmi trattati a lezione, spiegarlo brevemente e discuterne i pro e i contro rispetto agli altri algoritmi."*

Quale descrizione degli algoritmi di join è corretta?
- **A)** Il nested-loop vale per ogni condizione; l'hash-join solo per l'uguaglianza
- **B)** L'hash-join vale per ogni condizione; il nested-loop solo per l'uguaglianza
- **C)** Il merge-join non richiede input ordinati e funziona con qualsiasi condizione
- **D)** L'unico algoritmo di join è il prodotto cartesiano seguito da una selezione

> [!success]- Risposta
> **A)** **Nested-loop**: per ogni tupla della tabella esterna si scandisce l'interna; sempre applicabile, ma costoso se l'interna è grande e non ha indici. **Merge-join**: richiede input ordinati sul campo di join (o un B+ tree), scanditi in parallelo una sola volta. **Hash-join**: la stessa funzione hash sulle due tabelle, poi si confrontano solo i bucket corrispondenti; vale solo per gli equi-join. L'ottimizzatore sceglie tra questi algoritmi stimando il costo dei piani alternativi.

### Algebra relazionale

Schema per la gestione di una libreria (ogni libro ha un unico autore):

- SCRITTORE(<u>CodiceScrittore</u>, Nome, Cognome, AnnoDiNascita, CittàDiNascita)
- LIBRO(<u>CodiceLibro</u>, Autore, Titolo, Categoria) — FK: `Autore → SCRITTORE(CodiceScrittore)`
- EDIZIONE(<u>Editore, Libro, Anno</u>, Prezzo, NumCopie, NumeroPagine) — FK: `Editore → EDITORE(Sigla)`, `Libro → LIBRO(CodiceLibro)`
- EDITORE(<u>Sigla</u>, Nome, CittàDiSede, NumeroImpiegati)

#### Esercizio 1 (3 punti)
Elencare il titolo dei libri di categoria "Giallo" pubblicati da editori con più di 100 impiegati.

> [!success]- Soluzione
> $$\pi_{\text{Titolo}}\Big(\sigma_{\text{Categoria}=\text{'Giallo'}}(\text{LIBRO}) \bowtie_{\text{CodiceLibro}=\text{Libro}} \big(\text{EDIZIONE} \bowtie_{\text{Editore}=\text{Sigla}} \sigma_{\text{NumeroImpiegati}>100}(\text{EDITORE})\big)\Big)$$
>
> EDIZIONE fa da "ponte" tra LIBRO ed EDITORE. SCRITTORE non serve.

#### Esercizio 2 (3 punti)
Elencare la sigla degli editori che hanno pubblicato almeno un'edizione nel 2020 ma non hanno pubblicato nessuna edizione nel 2021. Disegnare anche l'albero dell'espressione.

> [!success]- Soluzione
> $$\rho_{\text{Sigla} \leftarrow \text{Editore}}\Big(\pi_{\text{Editore}}(\sigma_{\text{Anno}=2020}(\text{EDIZIONE})) - \pi_{\text{Editore}}(\sigma_{\text{Anno}=2021}(\text{EDIZIONE}))\Big)$$
>
> ```
>                 ρ Sigla ← Editore
>                         │
>                         −
>            ┌────────────┴────────────┐
>        π Editore                 π Editore
>            │                         │
>      σ Anno=2020               σ Anno=2021
>            │                         │
>        EDIZIONE                  EDIZIONE
> ```
>
> Basta EDIZIONE, che contiene già la sigla dell'editore (la ridenominazione finale è facoltativa). Una sola selezione `Anno = 2020 AND Anno <> 2021` sarebbe sbagliata: è vera per ogni edizione del 2020, quindi non esclude gli editori che hanno pubblicato anche nel 2021.

### Normalizzazione 🔜

#### Esercizio 3 (4 punti)
Si consideri la relazione R(X, Y, Z, D, E, F, G) con chiave {X, Y} e dipendenze funzionali $X \to ZD$, $EF \to G$ e $XY \to ZDEFG$. R è in BCNF? Se sì, dimostrarlo. Altrimenti, decomporre R in BCNF mostrando i passaggi.

> [!success]- Soluzione
> Una relazione è in BCNF se per ogni dipendenza non banale $A \to B$ il lato sinistro $A$ è una **superchiave**.
> - $XY \to ZDEFG$: XY è la chiave ✅
> - $X \to ZD$: $X^+ = \{X, Z, D\}$, non è superchiave ❌
> - $EF \to G$: $EF^+ = \{E, F, G\}$, non è superchiave ❌
>
> → R **non è in BCNF**.
>
> **Decomposizione**
> 1. Si separa la dipendenza $X \to ZD$:
>    - $R_1(\underline{X}, Z, D)$ — chiave X ✅
>    - $R_2(X, Y, E, F, G)$ — chiave XY, ma contiene ancora $EF \to G$ ❌
> 2. Si separa la dipendenza $EF \to G$ da $R_2$:
>    - $R_3(\underline{E, F}, G)$ — chiave EF ✅
>    - $R_4(\underline{X, Y}, E, F)$ — chiave XY ✅
>
> **Risultato**: $R_1(\underline{X}, Z, D)$, $R_3(\underline{E, F}, G)$, $R_4(\underline{X, Y}, E, F)$, tutte in BCNF.
> La decomposizione è senza perdita (ogni passo separa una dipendenza il cui lato sinistro diventa chiave della nuova relazione) e conserva le dipendenze: $X \to ZD$ sta in $R_1$, $EF \to G$ in $R_3$, $XY \to EF$ in $R_4$, e $XY \to ZDG$ si ottiene da queste.

### Progettazione 🔜

#### Esercizio (11 punti) — Database "Lezioni"
Si consideri la base di dati di un archivio per la gestione delle lezioni di un corso di studi universitario.

Di ogni insegnamento (ad es. Programmazione 1) si conosce il nome, la sigla univoca, i docenti che lo erogano (possono essere più di uno), e le eventuali propedeuticità (ad es. Programmazione 1 è propedeutica ad Algoritmi e strutture dati). Per ogni insegnamento si sa in quali aule e orari (giorno dell'anno, data di inizio e data di fine) è erogata ogni lezione. Per ogni lezione si conosce anche il docente che la eroga. Ogni lezione è erogata da un solo docente, ma diverse lezioni dello stesso insegnamento possono essere erogate da docenti diversi. Del docente si conoscono nome, cognome e matricola (univoca). Delle aule si conosce il nome, la sede, il piano e la capienza. Le aule possono essere computerizzate o meno. Delle aule computerizzate si conosce anche il sistema operativo e la versione delle macchine. Possono esistere aule con lo stesso nome purché in sedi diverse. Delle sedi si conosce il nome, l'indirizzo, il telefono, e quali aule ospitano.

1. Si disegni lo schema entità-relazione che rappresenti la base di dati, includendo entità, associazioni, attributi e identificatori, vincoli di cardinalità, generalizzazioni, eventuali vincoli non esprimibili in ER e qualsiasi altro costrutto necessario. Per ogni generalizzazione si indichi se è parziale o totale, esclusiva o sovrapposta. Il testo è soggetto a interpretazione: ove necessario, indicare la propria interpretazione giustificando le scelte.
2. Si disegni lo schema ER ristrutturato (riportando solo le modifiche rispetto al precedente).
3. Si scriva lo schema relazionale ottenuto, indicando le chiavi (sottolineate), gli attributi che ammettono valori nulli (con asterisco), i vincoli di integrità referenziale ed eventuali altri vincoli utili.

> [!success]- Una possibile soluzione
> **Interpretazione**: "giorno dell'anno, data di inizio e data di fine" indica la data della lezione con l'**ora di inizio** e l'**ora di fine**. Una lezione è identificata dall'insegnamento, dalla data e dall'ora di inizio.
>
> **1. Schema ER**
>
> | Entità | Attributi (identificatore sottolineato) |
> |---|---|
> | INSEGNAMENTO | <u>Sigla</u>, Nome |
> | DOCENTE | <u>Matricola</u>, Nome, Cognome |
> | LEZIONE (debole) | <u>Data, OraInizio</u> + insegnamento, OraFine |
> | SEDE | <u>Nome</u>, Indirizzo, Telefono |
> | AULA (debole) | <u>Nome</u> + sede, Piano, Capienza |
> | AULA COMPUTERIZZATA (figlia di AULA) | SistemaOperativo, Versione |
>
> | Associazione | Tra | Cardinalità |
> |---|---|---|
> | EROGA | DOCENTE – INSEGNAMENTO | DOCENTE (0,N) · INSEGNAMENTO (1,N) |
> | PROPEDEUTICITÀ (ricorsiva) | INSEGNAMENTO (*propedeutico*) – INSEGNAMENTO (*successivo*) | (0,N) – (0,N) |
> | DI (identificante) | LEZIONE – INSEGNAMENTO | LEZIONE (1,1) · INSEGNAMENTO (0,N) |
> | TENUTA DA | LEZIONE – DOCENTE | LEZIONE (1,1) · DOCENTE (0,N) |
> | SVOLTA IN | LEZIONE – AULA | LEZIONE (1,1) · AULA (0,N) |
> | OSPITA (identificante) | AULA – SEDE | AULA (1,1) · SEDE (1,N) |
>
> **Generalizzazione** AULA → AULA COMPUTERIZZATA: **parziale** (esistono aule non computerizzate) ed **esclusiva** (c'è un solo figlio).
>
> **Vincoli non esprimibili in ER**
> - Il docente che tiene una lezione deve essere uno dei docenti che **erogano** quell'insegnamento.
> - La propedeuticità non può essere riflessiva né ciclica.
> - Un'aula non può ospitare due lezioni sovrapposte nel tempo; `OraFine > OraInizio`.
>
> **2. Ristrutturazione**: la generalizzazione si elimina **accorpando il figlio nel padre**: AULA riceve `SistemaOperativo*` e `Versione*`, nulli per le aule non computerizzate (sono solo due attributi, e il figlio non ha associazioni proprie).
>
> **3. Schema relazionale**
> - SEDE(<u>Nome</u>, Indirizzo, Telefono)
> - AULA(<u>Nome, Sede</u>, Piano, Capienza, SistemaOperativo\*, Versione\*) — FK `Sede → SEDE`
> - INSEGNAMENTO(<u>Sigla</u>, Nome)
> - DOCENTE(<u>Matricola</u>, Nome, Cognome)
> - EROGAZIONE(<u>Insegnamento, Docente</u>) — FK `Insegnamento → INSEGNAMENTO`, `Docente → DOCENTE`
> - PROPEDEUTICITÀ(<u>Insegnamento, Propedeutico</u>) — FK entrambi verso INSEGNAMENTO
> - LEZIONE(<u>Insegnamento, Data, OraInizio</u>, OraFine, Docente, NomeAula, SedeAula) — FK `<Insegnamento, Docente> → EROGAZIONE`, `<NomeAula, SedeAula> → AULA`
>
> **Altri vincoli**
> - La FK `<Insegnamento, Docente> → EROGAZIONE` impone il primo vincolo non esprimibile in ER (il docente della lezione eroga l'insegnamento).
> - `SistemaOperativo` e `Versione` sono entrambi nulli o entrambi non nulli.
> - `Insegnamento ≠ Propedeutico`; `OraFine > OraInizio`.
