# 04 - Architettura fisica dei DBMS

> Fonte: [[DB/TEORIA/PDF/04 - Architettura fisica dei DBMS.pdf]]
> **Materiale**: Atzeni, Ceri, Fraternali, Paraboschi, Torlone — *Basi di dati*, 5ª ed., McGraw-Hill 2018, **cap. 11**
> oppure Garcia-Molina, Ullman, Widom — *Database Systems: The Complete Book*, 2/E, Pearson 2009, sez. 13.1–13.2, 13.5–13.8, 14.1–14.3, 15.1–15.4.1

**Argomenti**
1. Gestione della memoria (buffer, file e record, organizzazione fisica)
2. Indici (primari/secondari, densi/sparsi, B-tree e B+ tree, bitmap)
3. Hashing, esecuzione e ottimizzazione delle query

---

# Parte 1 — Gestione della memoria

## Dal livello logico al livello fisico

Questa unità descrive come i concetti del modello relazionale (lezioni [[02 - DBMS e Modello Relazionale|02]] e [[03 - Algebra relazionale|03]]) vengono realizzati concretamente su disco. La tabella fa da mappa per tutta l'unità (sintesi aggiunta, non presente nelle slide).

| Livello logico (cosa vede l'utente) | Livello fisico (come lo realizza il DBMS) | Cos'è, sul piano fisico | Dove nell'unità |
|---|---|---|---|
| **Base di dati** | Insieme di **file** gestiti dal DBMS | Il DBMS organizza da sé il contenuto dei file, senza affidarsi al file system | [[#1.4 File e record\|1.4]] |
| **Relazione** (tabella) | Uno o più **file** | Sequenza di blocchi che contengono i record della tabella | [[#1.4 File e record\|1.4]] |
| — (nessun corrispettivo) | **Blocco** su disco · **pagina** in RAM | Unità minima di lettura e scrittura (alcuni KB); la pagina è la copia del blocco nel buffer. Il costo di un'operazione si misura in **numero di blocchi** letti o scritti | [[#1.2 Memoria principale e secondaria\|1.2]], [[#1.3 Buffer manager\|1.3]] |
| **Tupla** (riga) | **Record** | Sequenza di byte con i valori della tupla | [[#1.4 File e record\|1.4]] |
| **Attributo** | **Campo** | Porzione del record che contiene il valore di un attributo | [[#1.4 File e record\|1.4]] |
| **Valore** (con il suo dominio) | **Byte**, secondo formato e lunghezza del campo | Es. intero su 2 byte, stringa di 10 caratteri su 10 byte | [[#Serializzazione dei record\|1.4]] |
| **Schema** della relazione | **Formato** del record | Formato fisso: lo schema sta nel direttorio · formato auto-descrittivo: i metadati stanno nel record stesso | [[#Serializzazione dei record\|1.4]] |
| Ordine delle tuple: **irrilevante** | Ordine dei record nei blocchi: **conta** per le prestazioni | Righe intere per blocco (row store) o colonne per blocco (column store); file disordinato (heap) o ordinato su un campo | [[#1.5 Row store vs column store\|1.5]], [[#1.6 Strutture sequenziali\|1.6]] |
| — (nessun corrispettivo) | **Indice** | Struttura ausiliaria per trovare i record senza leggere tutto il file; cambia i **tempi**, non il **risultato** delle query. Su chiavi primarie e attributi `UNIQUE` viene creato automaticamente | [[#Parte 2 — Indici\|Parte 2]] |
| **Interrogazione** (SQL, algebra) | **Piano di accesso** | Sequenza di operatori fisici (scansione, accesso diretto, nested-loop, merge-join, hash-join…) scelta dall'ottimizzatore | [[#Parte 3 — Hashing, esecuzione e ottimizzazione delle query\|Parte 3]] |

> [!tip] Indipendenza fisica
> Tutto ciò che sta nella colonna "livello fisico" si può cambiare (formato dei record, ordine nei file, indici, piani di accesso) **senza modificare** schemi logici, query e programmi: è l'**indipendenza fisica** vista in [[02 - DBMS e Modello Relazionale#1.7 Indipendenza dei dati|DB-02]]. Le due righe senza corrispettivo logico (blocco e indice) esistono solo per le prestazioni.

## 1.1 Architettura di alto livello del DBMS

```
              Comandi SQL
                   │
                   ▼
        ┌──────────────────────┐
        │ Interprete delle query│
        └──────────────────────┘
                   │  query plan
                   ▼
        ┌──────────────────────┐
        │   Execution engine    │
        └──────────────────────┘
                   │  read/write "virtuali"
                   ▼
        ┌──────────────────────┐
        │    Buffer manager     │◄──► Primary storage (RAM)
        └──────────────────────┘
                   │  read/write fisiche
                   ▼
        ┌──────────────────────┐
        │   Storage manager     │
        └──────────────────────┘
                   │
                   ▼
        Secondary storage (disco)
```

## 1.2 Memoria principale e secondaria

- I programmi possono fare riferimento **solo** a dati in **memoria principale**.
- Le basi di dati devono stare in **memoria secondaria** per due motivi: **dimensioni** e **persistenza**.
- I dati in memoria secondaria sono utilizzabili **solo dopo il trasferimento** in memoria principale.

### Blocchi e pagine
- I dispositivi di memoria secondaria sono organizzati in **blocchi** di lunghezza fissa (alcuni KB).
- Le uniche operazioni possibili sono **lettura e scrittura di un intero blocco**.
- **Pagina**: unità di suddivisione logica della RAM. Per semplicità si assume *dimensione pagina = dimensione blocco*.

### Tempi di accesso

| Supporto | Tempo medio di accesso |
|---|---|
| **HDD** (posizionamento testina + latenza + trasferimento) | ~1–10 ms |
| **SSD** | ~50 volte meno dell'HDD (ma più costoso) |
| **Memoria terziaria** (nastri, optical jukebox) | ~5–60 s |

> [!important] Modello di costo
> Un accesso a memoria secondaria costa **quattro o più ordini di grandezza** in più di un'operazione in memoria centrale. Si semplifica quindi così:
> **il costo di un'operazione del DBMS dipende esclusivamente dal numero di accessi a memoria secondaria** (numero di blocchi letti o scritti).
> Accessi a blocchi "vicini" costano meno (**contiguità**).

## 1.3 Buffer manager

- **Buffer**: area di memoria centrale **preallocata**, gestita dal DBMS e **condivisa** fra le transazioni; è il "deposito temporaneo" delle porzioni di DB correntemente usate.
- **Scopo del buffer manager**: **ridurre il numero di accessi** alla memoria secondaria.

### Dati gestiti
- Il **contenuto** del buffer (le pagine).
- Un **direttorio** che per ogni pagina mantiene:
  - il file e il numero del blocco corrispondente su disco;
  - due variabili di stato:
    - un **contatore** di quanti programmi stanno usando la pagina;
    - un **bit di stato (dirty)** che indica se la pagina è stata modificata (la scrittura su disco può essere **differita**).

### Funzioni
- Riceve richieste di lettura/scrittura di blocchi (che contengono record).
- Usa il buffer quando possibile e accede alla memoria secondaria **solo quando indispensabile**.

```
                        MEMORIA PRIMARIA                 MEMORIA SECONDARIA
                    ┌───────────────────────┐
  transazione       │ contatore │ bit │ pag │ ──flush──►  blocco
 ("programma")      │ contatore │ bit │ pag │ ──force──►  blocco
      │             │    …      │  …  │  …  │             …
      │ fix/unfix   └───────────────────────┘
      │ setDirty              ▲
      ▼                       │ steal / no-steal
  GESTORE DEL BUFFER ─────────┘
```

### Primitive (interfaccia)

| Primitiva | Effetto |
|---|---|
| `fix` | Richiede una pagina: legge da disco **solo se non è già nel buffer**; incrementa il contatore |
| `setDirty` | Comunica che la pagina è stata **modificata** |
| `unfix` | La transazione ha finito di usare la pagina: decrementa il contatore |
| `force` | Trasferisce **in modo sincrono** una pagina su disco |
| `flush` | Scrittura **asincrona** su disco (a discrezione del buffer manager) |

### Esecuzione della `fix`
1. Cerca la pagina nel buffer → se c'è, ne restituisce l'indirizzo.
2. Altrimenti cerca una pagina **libera** (contatore = 0) e la sovrascrive:
   - se ce n'è più di una si sceglie con una politica (**FIFO**, **LRU**, …);
   - la pagina sovrascritta, se *dirty*, viene prima salvata su disco.
3. Se **nessun contatore è a zero**:
   - **steal**: si sceglie una "vittima" fra le pagine occupate;
   - **no-steal**: l'operazione viene messa in attesa.

## 1.4 File e record

> [!definition] Record (definizione aggiunta, non presente nelle slide)
> Un **record** è la rappresentazione **fisica** di una **tupla** su disco: la sequenza di byte che contiene i valori dei suoi **campi** (gli attributi).
>
> Ogni tabella è memorizzata in uno o più **file**; ogni file è diviso in **blocchi** (l'unità letta e scritta dal disco); ogni blocco contiene uno o più **record**. Es. la tupla `(55, smith, 02)` di `Employee` diventa un record di 14 byte (2 + 10 + 2), con i campi `E#`, `E.name`, `Dept`.
>
> Per la corrispondenza completa tra concetti logici e fisici vedi [[#Dal livello logico al livello fisico]].

L'organizzazione interna dei file è gestita **direttamente dal DBMS**: struttura dei singoli blocchi e distribuzione dei record nei blocchi.

```
          "sub"-file                    "sub"-file
Blocco 1 [ r1 | r2 | … | rk ]   Blocco 2 [ rk+1 | rk+2 | … ]
                       └──────── file ────────┘
```

### Serializzazione dei record
Alternative:
- formato **fisso** oppure **auto-descrittivo**;
- lunghezza dei campi **fissa** oppure **variabile**.

esempi: 1: tutto fisso 2: auto - variabile 
**Formato fisso, lunghezza fissa** — lo schema è nel direttorio, il record contiene solo i dati.
Es. `Employee`: `E#` (intero, 2 byte), `E.name` (10 char), `Dept` (codice, 2 byte):

```
| 55 | s m i t h     | 02 |
| 83 | j o n e s     | 01 |
```

**Formato auto-descrittivo, lunghezza variabile** — il record contiene anche i **metadati**:

```
| 2 | 5 | I | 46 | 4 | S | 4 | F O R D |
```
- `2` → numero di campi
- `5` → codice/nome della prima colonna, `I` → tipo Integer, `46` → valore
- `4`, `S`, `4` → codice 2ª colonna, tipo String, lunghezza
- `F O R D` → valore della stringa

> [!question]- Vantaggi e svantaggi?
> - **Auto-descrittivo**: flessibile (record con campi diversi, attributi opzionali, schema evolutivo), ma occupa più spazio e richiede più elaborazione per decodificare.
> - **Fisso**: compatto e veloce (offset dei campi noti a priori), ma rigido e spreca spazio con valori variabili/assenti.

^q-serializzazione

### Unspanned vs spanned

- **Unspanned**: ogni record sta in **un solo** blocco.
  ```
  block 1: [ R1 | R2 | ░░ ]   block 2: [ R3 | R4 | R5 ]
  ```
- **Spanned**: un record può essere **diviso** su più blocchi.
  ```
  block 1: [ R1 | R2 | R3(a) ]   block 2: [ R3(b) | R4 | R5 | R6 | R7(a) ]
  ```

| Unspanned | Spanned |
|---|---|
| Molto più semplice da implementare e veloce | Necessario per record **più grandi di un blocco** |
| Spreca spazio | Sfrutta meglio lo spazio |

## 1.5 Row store vs column store

Esempio: `Acquisto(id, cust, prod, store, price, date, qty)`

**Row store** — si scrive su disco **in ordine di record**: ogni blocco contiene tuple intere e consecutive.

| Blocco | id | cust | prod | store | price | date | qty |
|---|---|---|---|---|---|---|---|
| BLOCK1 | 1 | Rossi | Mouse | MI | 12 | 10/1 | 2 |
| | 2 | Bianchi | Tastiera | TO | 25 | 11/1 | 1 |
| BLOCK2 | 3 | Verdi | Monitor | RM | 150 | 12/1 | 1 |
| | 4 | Russo | Mouse | MI | 12 | 13/1 | 3 |
| BLOCK3 | 5 | Rossi | Monitor | TO | 150 | 14/1 | 2 |
| | 6 | Bianchi | Tastiera | RM | 25 | 15/1 | 1 |

**Column store** — si scrive **in ordine di colonna**: ogni blocco contiene i valori di una colonna.

| BLOCK1 (id) | BLOCK2 (cust) | BLOCK3 (prod) |
|---|---|---|
| 1 | Rossi | Mouse |
| 2 | Bianchi | Tastiera |
| 3 | Verdi | Monitor |
| 4 | Russo | Mouse |
| 5 | Rossi | Monitor |
| 6 | Bianchi | Tastiera |

Gli ID possono essere **impliciti** (la riga N di ogni blocco è la stessa tupla) oppure **espliciti**, marcando ogni valore con l'id della tupla: `(1, Rossi)`, `(2, Bianchi)`, …

| Vantaggi del **column store** | Vantaggi del **row store** |
|---|---|
| Letture efficienti per **data mining** e **statistiche** (si leggono solo le colonne necessarie) | Più efficiente per **letture e scritture di più campi dello stesso record** |

> Nel seguito si assume un **row store**.

## 1.6 Strutture sequenziali

Esiste un ordinamento fra le tuple che può essere rilevante per la gestione.

- **Organizzazione disordinata** (*heap*): ordinamento fisico ma non logico; gli inserimenti avvengono al posto di record cancellati oppure in coda (con riorganizzazioni periodiche).
- **Organizzazione ordinata**: l'ordine dei record è coerente con quello di un campo; usata principalmente per memorizzare gli **indici**.

Non esiste un'organizzazione migliore in assoluto: è un compromesso fra **flessibilità**, **consumo di spazio**, **complessità** e **performance**.

---

# Parte 2 — Indici

## 2.1 Definizione

> [!definition] Indice
> Struttura **ausiliaria** per l'accesso efficiente ai record sulla base dei valori di uno o più campi. Tali campi sono detti **chiave** dell'indice (ma non sono necessariamente identificanti).

**Idea**: l'indice analitico di un libro — una lista di coppie *(termine, pagina)*, ordinata alfabeticamente sui termini, posta in fondo al libro e separabile da esso.

## 2.2 Tipi di indice

| Classificazione                   | Tipo           | Descrizione                                                                                                       |
| --------------------------------- | -------------- | ----------------------------------------------------------------------------------------------------------------- |
| Rispetto all'ordinamento del file | **Primario**   | Su un campo sul cui ordinamento è basata la memorizzazione                                                        |
|                                   | **Secondario** | Su una struttura non ordinata, oppure su una struttura ordinata ma su un campo che non ne determina l'ordinamento |
| Rispetto al numero di voci        | **Denso**      | Contiene una entry per **ciascun valore** del campo chiave                                                        |
|                                   | **Sparso**     | Contiene **meno** entry dei valori distinti della chiave (tipicamente una per blocco)                             |

### Indice denso vs sparso (file ordinato, 2 record per blocco)

```
Indice denso        File su disco               Indice sparso
 10 → P1  ─┐       BLOCK1 [10 D10 | 20 D20]  ◄── 10 → P1
 20 → P1  ─┘       BLOCK2 [30 D30 | 40 D40]  ◄── 30 → P2
 30 → P2  ─┐       BLOCK3 [50 D50 | 60 D60]  ◄── 50 → P3
 40 → P2  ─┘       BLOCK4 [70 D70 | 80 D80]  ◄── 70 → P4
 50 → P3           BLOCK5 [90 D90 |100 D100] ◄── 90 → P5
 …                                                …
```
(`Py` = puntatore al blocco y, `Dx` = altri campi della riga con chiave x)

Con un indice la ricerca legge **pochi blocchi dell'indice + un blocco di dati**, invece di scandire il file.

| **Sparso** | **Denso** |
|---|---|
| Occupa **meno spazio** → una parte maggiore dell'indice sta in RAM | Permette di sapere **se esiste** un record con una certa chiave **guardando solo l'indice** |
| Utilizzabile solo su file **ordinati** sulla chiave | Utilizzabile anche su file **non ordinati** |

### Indici multilivello
Si può costruire un **indice sparso sull'indice** (di 1° livello, denso o sparso), ottenendo un indice di **2° livello**, e così via:

```
2° livello (sparso)    1° livello (sparso)     Dati
 10 → B101    ───►      10, 30, 50, 70      (BLOCK101)
 90 → B102    ───►      90, 110, 130, 150   (BLOCK102)
170 → B103    ───►     170, 190, 210, 230   (BLOCK103)
 …
```

### Indici primari
- ✅ Semplici da implementare; efficienti per **scansioni** (letture di sequenze contigue di record).
- ❌ Modifiche costose; difficile mantenere l'ordine.

### Indici secondari
Su un file **disordinato** (es. chiavi `30, 50, 20, 70, 80, 40, 100, 10, 90, 60`):
- un indice **sparso** non ha senso (non si può dedurre il contenuto di un blocco dal suo primo valore);
- serve un indice **denso** di 1° livello (ordinato), sul quale si può costruire un indice **sparso di 2° livello**.

```
Sparso 2° liv.   Denso 1° liv.       File disordinato
  10   ──►       10 20 30 40    ──►  [30 50] [20 70] [80 40]
  50   ──►       50 60 70 …          [100 10] [90 60]
  90   ──►       …
```

## 2.3 Cancellazione da indice sparso

File ordinato con 2 record per blocco e indice sparso con una voce per blocco (la prima chiave del blocco).

**Situazione iniziale**
```
Indice sparso         File su disco
 10 → BLOCK1   ───►   BLOCK1 [ 10 | 20 ]
 30 → BLOCK2   ───►   BLOCK2 [ 30 | 40 ]
 50 → BLOCK3   ───►   BLOCK3 [ 50 | 60 ]
 70 → BLOCK4   ───►   BLOCK4 [ 70 | 80 ]
```

Tutti i casi seguenti riguardano **BLOCK2**:

| Caso                                              | Nel blocco                                   | Nell'indice                                                                                        |
| ------------------------------------------------- | -------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| **1** — elimino un record **non chiave** (es. 40) | Lo spazio si libera                          | **Nessuna modifica**: la chiave 30 è ancora valida                                                 |
| **2** — elimino il record **chiave** (es. 30)     | Il record viene eliminato                    | La chiave si aggiorna al **primo record superstite** (30 → 40)                                     |
| **3** — elimino **entrambi** (30 e 40)            | Il blocco diventa vuoto e **riutilizzabile** | La voce viene **rimossa** (si passa da 10 direttamente a 50); i puntatori successivi si aggiornano |
|                                                   |                                              |                                                                                                    |

**Dopo il caso 3**
```
Indice sparso         File su disco
 10 → BLOCK1   ───►   BLOCK1 [ 10 | 20 ]
                      BLOCK2 [    |    ]   ← vuoto, riutilizzabile
 50 → BLOCK3   ───►   BLOCK3 [ 50 | 60 ]
 70 → BLOCK4   ───►   BLOCK4 [ 70 | 80 ]
```

^ex-canc-sparso

> [!question]- Prova tu
> - Cosa cambierebbe cancellando anche il record **50**, chiave di BLOCK3? → la voce 50 diventerebbe **60** (caso 2).
> - Se, dopo aver svuotato BLOCK2, si inserisse un record con chiave **35**, dove andrebbe? → nel blocco libero fra BLOCK1 (≤ 20) e BLOCK3 (≥ 50), cioè nel BLOCK2 riutilizzabile, con una nuova voce 35 nell'indice.

^q-prova-tu

## 2.4 Inserimento in indice sparso

Esempio delle slide: blocchi da 2 posizioni, **non tutti pieni**, e indice sparso con la prima chiave di ogni blocco.

**Situazione iniziale**
```
Indice sparso         File su disco
 10 → BLOCK1   ───►   BLOCK1 [ 10 | 20 ]   ← pieno
 30 → BLOCK2   ───►   BLOCK2 [ 30 |    ]
 40 → BLOCK3   ───►   BLOCK3 [ 40 | 50 ]   ← pieno
 60 → BLOCK4   ───►   BLOCK4 [ 60 |    ]
```

**Inserimento del record 25.** Per mantenere l'ordine, 25 va **dopo 20 e prima di 30**: tra BLOCK1, che è pieno, e BLOCK2.

**Caso A — c'è spazio in BLOCK2** (come nella situazione iniziale): si inserisce 25 in testa a BLOCK2 spostando 30, e la voce dell'indice si aggiorna alla nuova prima chiave.
```
 10 → BLOCK1   ───►   BLOCK1 [ 10 | 20 ]
 25 → BLOCK2   ───►   BLOCK2 [ 25 | 30 ]   ← la voce 30 diventa 25
 40 → BLOCK3   ───►   BLOCK3 [ 40 | 50 ]
 60 → BLOCK4   ───►   BLOCK4 [ 60 |    ]
```

**Caso B — non c'è spazio** (la domanda delle slide: "e se non ci fosse spazio dopo il 30?"). Due possibilità:
- **blocco di overflow** (la soluzione mostrata nelle slide): si alloca un nuovo blocco collegato a BLOCK1 e si scrive lì il 25. L'indice **non cambia**: chi cerca 25 arriva a BLOCK1 e poi segue il collegamento.
  ```
   10 → BLOCK1   ───►   BLOCK1 [ 10 | 20 ] ──► OVERFLOW [ 25 |    ]
   30 → BLOCK2   ───►   BLOCK2 [ 30 | 35 ]   ← ipotesi: BLOCK2 pieno
   40 → BLOCK3   ───►   BLOCK3 [ 40 | 50 ]
   60 → BLOCK4   ───►   BLOCK4 [ 60 |    ]
  ```
- **nuovo blocco nella sequenza** (split): si inserisce un blocco tra BLOCK1 e BLOCK2 e si aggiunge la voce `25` all'indice. Se anche il blocco dell'indice è pieno, va diviso a sua volta (le slide parlano di "splittare la radice dell'indice"): è il meccanismo che i B-tree rendono sistematico (vedi [[#2.6 Alberi di ricerca|2.6]]).

(Il record 35 del caso B è aggiunto solo per rendere BLOCK2 pieno.)

> [!question]- Svantaggi dei blocchi di overflow?
> Le ricerche devono seguire le catene di overflow (più accessi a disco), l'ordinamento fisico si degrada e servono riorganizzazioni periodiche.

^q-overflow

## 2.5 Caratteristiche e problemi degli indici ordinati

- ✅ Accesso diretto **efficiente sulla chiave**, sia **puntuale** sia **per intervalli**.
- ❌ Inefficaci per ricerche su **altri campi** (non chiave).
- ❌ Modifiche della chiave, inserimenti ed eliminazioni **inefficienti** (come in tutte le strutture ordinate).

**Tecniche per alleviare i problemi**: blocchi di overflow · marcatura per le eliminazioni · riempimento parziale · riorganizzazioni periodiche.

> [!important] Il problema
> Tutte le strutture viste finora sono basate su strutture **ordinate**, quindi poco flessibili in presenza di **elevata dinamicità**. Nei DBMS si usano **indici dinamici multilivello**: i **B-tree** (alberi di ricerca bilanciati).

## 2.6 Alberi di ricerca

### Albero binario di ricerca
Albero binario etichettato in cui, per ogni nodo, il sottoalbero **sinistro** contiene solo etichette **minori** e quello **destro** solo etichette **maggiori**.

```
        4
      /   \
     1     8
      \   / \
       3 5   9
```

Tempo di ricerca e inserimento = **profondità** del nodo → **logaritmico** nel caso medio (inserimenti in ordine casuale).

### Albero di ricerca di ordine $P$
Generalizza l'albero binario: un nodo contiene **più chiavi** e ha **più figli**.
1. Ogni nodo ha (fino a) $P$ figli e (fino a) $P-1$ chiavi, **ordinate**.
2. L'$i$-esimo sottoalbero contiene chiavi **maggiori** della $(i-1)$-esima e **minori** della $i$-esima.
3. Ogni ricerca o modifica visita un **cammino radice → foglia**.
4. Nelle strutture fisiche **un nodo corrisponde a un blocco**: più chiavi per nodo significa albero più basso e meno accessi al disco.

**Esempio: albero di ricerca di ordine 3** (fino a 3 figli e 2 chiavi per nodo)

![[DB04-albero-ordine-3.png|520]]

*Ricerca di 27*: nella radice `[30 | 60]`, 27 < 30 → primo figlio `[10 | 20]`; 27 > 20 → terzo figlio `[24 | 27]` → trovato. Si leggono **3 nodi**, cioè 3 blocchi.

L'albero però **non è bilanciato**: per arrivare a 93 o 96 servono 4 livelli, per 40 ne bastano 2. Con inserimenti sfortunati un ramo può allungarsi senza limite: la struttura è (potenzialmente) **rigida**.

Un **B-tree** è un albero di ricerca mantenuto **bilanciato** (tutte le foglie alla stessa profondità) grazie a:
1. **riempimento parziale** dei nodi (mediamente ~70%), che lascia spazio per gli inserimenti;
2. **riorganizzazioni locali** (split e merge) quando un nodo si riempie o si svuota troppo.

### Organizzazione dei nodi del B-tree

![[DB04-nodo-btree.png|620]]

Un nodo alterna **puntatori** ($P_i$) e **chiavi** ordinate ($K_i$):

| Puntatore | Porta al sottoalbero con le chiavi |
|---|---|
| $P_0$ (prima di $K_1$) | $K < K_1$ |
| $P_i$ (tra $K_i$ e $K_{i+1}$) | $K_i \le K < K_{i+1}$ |
| $P_{F+1}$ (dopo l'ultima chiave $K_F$) | $K \ge K_F$ |

**Ricerca della chiave $K$ in un nodo**: si scorrono le chiavi finché non si trova la prima $K_i$ **maggiore** di $K$ e si segue il puntatore alla sua sinistra; se $K$ è maggiore o uguale a tutte, si segue l'ultimo puntatore. Si ripete fino alla foglia.

### Split e merge
1. Inserimenti ed eliminazioni sono preceduti da una **ricerca fino a una foglia**.
2. Se c'è posto nella foglia, si inserisce lì.
3. Altrimenti il nodo viene **suddiviso (split)** in due: la chiave mediana sale al genitore, che riceve un puntatore in più.
4. Se non c'è posto nemmeno nel genitore, si risale ulteriormente; se si divide la radice, se ne crea una nuova e l'albero **cresce di un livello** (dall'alto, per questo resta bilanciato).
5. Simmetricamente, un'eliminazione che svuota troppo un nodo causa una **fusione (merge)** con un fratello.

> Il riempimento dei nodi rimane **sempre superiore al 50%**.

**Esempio di split** — una foglia da 3 chiavi `[10 | 20 | 30]` è piena; inserendo 25 si ottiene l'overflow `[10 | 20 | 25 | 30]`, quindi la foglia si divide in `[10 | 20]` e `[30]` e la chiave mediana **25 sale al genitore** come nuovo separatore. ^ex-split

![[DB04-split-esempio.png|620]]

**Split e merge sullo stesso albero** (nodi da 4 chiavi):

![[DB04-split-merge.png|560]]

| Passo | Cosa succede |
|---|---|
| **Situazione iniziale** | Radice `[k1 k6]`; la foglia `[k1 k2 k4 k5]` è **piena** (4/4) |
| **a. insert k3 → split** | k3 andrebbe tra k2 e k4, ma non c'è posto: la foglia si divide in `[k1 k2]` e `[k3 k4 k5]` e k3 sale alla radice, che diventa `[k1 k3 k6]` |
| **b. delete k2 → merge** | Partendo dal risultato di a: togliendo k2 la foglia `[k1]` scende sotto il 50%, quindi si **fonde** con la sorella in `[k1 k3 k4 k5]` e il separatore k3 sparisce dalla radice, che torna `[k1 k6]` |

Video di esempio: <https://www.youtube.com/watch?v=coRJrcIYbF4>

### B-tree vs B+ tree

| | **B-tree** | **B+ tree** |
|---|---|---|
| Dove stanno i puntatori ai dati | In **tutti i nodi**, anche quelli intermedi | **Solo nelle foglie** |
| Chiavi | Ogni chiave compare **una sola volta** | Le chiavi dei nodi intermedi sono **copie** che fanno solo da separatori: ogni chiave compare comunque in una foglia |
| Foglie | Indipendenti | **Collegate in una lista** ordinata |
| Ricerca puntuale | A volte si ferma prima, in un nodo intermedio | Arriva sempre fino a una foglia |
| Ricerca per intervallo | Bisogna risalire e riscendere nell'albero | Si trova il primo valore e si **scorrono le foglie** |
| Uso | — | **Lo standard nei DBMS** |

**B-tree** — le chiavi k1, k6, k10 della radice puntano **direttamente** ai dati (t(k1), t(k6), t(k10)), come le chiavi delle foglie:

![[DB04-btree.png|620]]

**B+ tree** — i nodi intermedi (*paolo*, *mauro*, *renzo*) servono solo per orientarsi; tutte le chiavi sono nelle foglie, che sono collegate tra loro e puntano ai dati:

![[DB04-bplustree.png|620]]

*Ricerca di "renzo"*: radice `[paolo]`, renzo ≥ paolo → figlio destro `[renzo]`; renzo ≥ renzo → foglia `[renzo | teresa]` → trovato.

*Ricerca dei nomi tra "dino" e "paolo"*: si scende una sola volta fino alla foglia che contiene *dino*, poi si seguono i collegamenti tra le foglie: dino → mauro → paolo. In un B-tree con le stesse chiavi, alcune (ad esempio *paolo* nella radice) starebbero solo nei nodi intermedi: per leggerle in ordine bisognerebbe risalire e riscendere nell'albero.

### Efficienza dei B-tree
- Molto efficienti quando il numero di chiavi **per blocco è grande** (fan-out elevato).
- Split e merge sono **poco frequenti** e in genere limitati a piccole porzioni dell'albero.
- **Numero di letture per una ricerca** = profondità dell'albero **+ 1** (per leggere il record).
- **Profondità tipica: 3 o 4** (salvo DB molto grandi).

---

# Parte 3 — Hashing, esecuzione e ottimizzazione delle query

## 3.1 Strutture ad accesso calcolato (hashing)

**Obiettivo**: accedere **direttamente** ai record con un certo valore di un campo, senza scorrere un indice livello per livello. Il campo è detto impropriamente "chiave", anche se non è necessariamente identificante.

**Idea**: invece di *cercare* dove sta un record, lo si **calcola**. Una **funzione hash** $h$ trasforma il valore della chiave in un **indirizzo** (il numero di un blocco), e il record viene scritto e cercato proprio lì. Lo spazio degli indirizzi è di dimensione **leggermente superiore** al necessario, per lasciare posti liberi.

| | B+ tree | Hashing |
|---|---|---|
| Come si trova il record | Si scende dalla radice a una foglia | Si calcola $h(k)$ |
| Accessi per una ricerca | Profondità + 1 (tipicamente 4–5) | **≈ 1** |

### Tabella hash

![[DB04-tabella-hash.png|560]]

1. Si calcola $h(\text{key})$: nell'esempio $h(\text{key}) = 1$.
2. Nell'**indice hash** (il blocco B1, con le posizioni 0–3) la posizione 1 contiene il puntatore $P_{203}$.
3. Il puntatore porta al blocco di dati **B203**, che contiene il record cercato.

Se l'indice hash è piccolo e sta in memoria centrale, la ricerca costa **un solo accesso al disco**. Si può anche fare a meno dell'indice e usare $h(k)$ direttamente come numero di blocco del file (*file hash*).

### La funzione hash

$$h : C \to I \qquad |I| \ll |C|$$

- $C$ = insieme dei **possibili valori** della chiave (es. tutte le matricole a 6 cifre: un milione);
- $I$ = insieme degli **indirizzi** disponibili (es. 50 posizioni).

Una **buona** funzione hash distribuisce i valori della chiave sugli indirizzi in modo **uniforme**, così che ogni indirizzo riceva più o meno lo stesso numero di record. La funzione usata negli esempi delle slide è il **resto della divisione**: $h(k) = k \bmod n$, dove $n$ è il numero di indirizzi.

### Collisioni

Poiché i possibili valori sono molti più degli indirizzi ($|C| \gg |I|$), $h$ **non può essere iniettiva**: esistono **collisioni**, cioè chiavi diverse che finiscono nello **stesso indirizzo**.

**Esempio** (slide): 40 record in una tabella di **50 posizioni**, con $h(k) = k \bmod 50$ (cioè le ultime due cifre della chiave, modulo 50). ^ex-collisioni

| Situazione | Posizioni | Chiavi |
|---|---|---|
| **4 chiavi** nella stessa posizione | 1 (pos. 10) | 205610, 201260, 102360, 205460 |
| **3 chiavi** nella stessa posizione | 2 (pos. 2 e 5) | pos. 2: 205802, 200902, 116202 · pos. 5: 66005, 116455, 200205 |
| **2 chiavi** nella stessa posizione | 5 (es. pos. 1 e 46) | pos. 1: 66301, 205751 · pos. 46: 200296, 205796 · … |

Anche se le posizioni sono più dei record (50 contro 40), le collisioni sono **inevitabili**: già con pochi record è molto probabile che due finiscano nello stesso posto.

| Ridurre la probabilità di collisione | Gestire le collisioni che avvengono comunque |
|---|---|
| Usare una **buona** funzione hash (distribuzione casuale e uniforme) | **Posizioni successive**: se la posizione è occupata, si usa la prima libera dopo di essa (es. la chiave destinata alla 10 finisce nella 11) |
| Aumentare lo **spazio ridondante** (più posizioni rispetto ai record) | **Tabella di overflow**: i record in eccesso vanno in un'area separata, collegata alla posizione originale |

In entrambi i casi chi cerca una chiave in collisione deve fare **accessi in più**: per questo il costo medio è "di poco superiore" a 1.

### File hash con fattore di blocco

Nella pratica ogni indirizzo non è una singola posizione ma un **blocco** che contiene più record (il **fattore di blocco**). Stessi 40 record, stesse 50 posizioni totali, ma organizzate in **5 blocchi da 10 posizioni**, con $h(k) = k \bmod 5$:

![[DB04-file-hash-blocchi.png|620]]

Ogni colonna è un blocco. Solo due blocchi ricevono **11** record invece di 10, quindi ci sono **solo due overflow** (205845 e 206092, in rosso), contro le molte collisioni dell'esempio precedente.

> [!tip] Perché funziona
> Una posizione singola "trabocca" appena riceve 2 record; un blocco da 10 trabocca solo se ne riceve più di 10. Le fluttuazioni della distribuzione si **compensano** su gruppi grandi: più alto è il fattore di blocco, più rari sono gli overflow (a parità di spazio totale).

### Come gestire la crescita dei dati

Se il numero di record cresce molto, gli overflow si moltiplicano e le prestazioni peggiorano. Due soluzioni:
- **overflow e riorganizzazioni periodiche**: di tanto in tanto si ricostruisce il file con più blocchi e una nuova funzione hash (operazione costosa);
- **hashing dinamico**: la struttura cresce **gradualmente**, un blocco alla volta.

### Hashing dinamico (estendibile)

La funzione hash produce $b$ bit, ma se ne usano solo i **primi $i$**, e $i$ **cresce nel tempo**.

- Un **direttorio** ha $2^i$ voci, una per ogni combinazione dei primi $i$ bit; ogni voce punta a un blocco. $i$ è la **profondità globale**.
- Ogni blocco ha una **profondità locale** $j \le i$: quanti bit iniziali hanno in comune tutte le sue chiavi. Se $j < i$, il blocco è puntato da **più voci** del direttorio (che differiscono solo negli ultimi bit).
- **Ricerca**: si leggono i primi $i$ bit di $h(k)$, si va alla voce corrispondente del direttorio e si legge il blocco (≈ 1 accesso, se il direttorio sta in memoria).

**Inserimento in un blocco pieno:**

| Caso | Cosa si fa |
|---|---|
| $j < i$ | **Split del blocco senza toccare il direttorio**: il blocco si divide in due in base al bit $j+1$, entrambi passano a profondità $j+1$, e le voci che puntavano al vecchio blocco vengono ripartite tra i due |
| $j = i$ | **Raddoppio del direttorio**: $i$ aumenta di 1, ogni voce viene sdoppiata (le due copie puntano allo stesso blocco), poi si procede con lo split come sopra |

**Esempio** (slide): $h(k)$ restituisce 4 bit, ogni blocco contiene 2 chiavi. Accanto a ogni blocco c'è la sua profondità locale. ^ex-hash-est

**1. Situazione iniziale**, $i = 1$: le chiavi che iniziano con `0` vanno nel primo blocco, quelle con `1` nel secondo.

![[DB04-hash-est-1.png|320]]

**2. Insert 1010** → va nel blocco `1`, che è pieno (1001, 1100) e ha $j = 1 = i$: **raddoppio del direttorio** ($i = 2$, voci `00 01 10 11`) e split del blocco in base al secondo bit: `10` → {1001, 1010}, `11` → {1100}, entrambi con profondità 2. Il blocco {0001} non cambia e resta puntato sia da `00` sia da `01` (profondità locale 1).

![[DB04-hash-est-2.png|280]]

**3. Insert 0111** → va nel blocco puntato da `01`, cioè {0001}, che ha posto: diventa {0001, 0111}.

![[DB04-hash-est-3.png|380]]

**4. Insert 0000** → va nello stesso blocco, che ora è pieno, ma $j = 1 < i = 2$: **split senza raddoppio**. Le chiavi si ripartiscono in base al secondo bit: `00` → {0000, 0001}, `01` → {0111}, entrambi con profondità 2.

**5. Insert 1001** (un secondo 1001) → va nel blocco `10` = {1001, 1010}, pieno, con $j = 2 = i$: **nuovo raddoppio** ($i = 3$) e split in base al terzo bit: `100` → {1001, 1001}, `101` → {1010}, entrambi con profondità 3.

![[DB04-hash-est-4.png|380]]

![[DB04-hash-est-5.png|300]]

Nella situazione finale il direttorio ha 8 voci ma solo 5 blocchi: i blocchi con profondità 2 sono puntati da due voci ciascuno (`000` e `001`, `010` e `011`, `110` e `111`).

> [!note] Chiave duplicata
> Le due copie di **1001** finiscono necessariamente nello stesso blocco: nessun bit le può separare, perché hanno lo stesso valore di $h$. Qui ci stanno (il blocco ha 2 posti), ma un **terzo** 1001 non potrebbe essere sistemato con nessun numero di raddoppi: con troppi duplicati l'hashing estendibile richiede comunque blocchi di overflow.

### Osservazioni sui file hash
- ✅ Organizzazione **più efficiente** per l'accesso diretto con condizioni di **uguaglianza** (`WHERE Matricola = 104`).
- ≈ Costo medio **di poco superiore all'unità** (≈ 1 accesso), a causa delle collisioni.
- ❌ **Non efficiente** per ricerche su **intervalli** (`WHERE Voto BETWEEN 27 AND 30`): valori vicini finiscono in blocchi lontani, quindi bisogna leggere tutto il file.
- ⚠️ Il caso peggiore (tutte le chiavi nello stesso indirizzo) è molto costoso, ma improbabile con una buona funzione hash.

## 3.2 Indice bitmap

Adatto a colonne con **pochi valori distinti** (es. stato civile: single/sposato; fascia d'età: 0-17, 18-34, 35-49, 50-65, 65+; regione).

Si crea **una colonna di bit per ogni valore** possibile: il bit vale 1 se la riga ha quel valore.

| Person_ID | Marital_Status | Single | Married |
|---|---|---|---|
| 1001 | Single | 1 | 0 |
| 1002 | Single | 1 | 0 |
| 1003 | Married | 0 | 1 |
| 1004 | Single | 1 | 0 |
| 1005 | Married | 0 | 1 |
| … | … | … | … |

| Person_ID | Age_Range | 18-34 | 35-49 | 50-65 | 65+ |
|---|---|---|---|---|---|
| 1001 | 18-34 | 1 | 0 | 0 | 0 |
| 1002 | 65+ | 0 | 0 | 0 | 1 |
| 1003 | 65+ | 0 | 0 | 0 | 1 |
| 1004 | 50-65 | 0 | 0 | 1 | 0 |
| 1005 | 35-49 | 0 | 1 | 0 | 0 |
| … | … | … | … | … | … |

**Query su più colonne** — *"Trova tutti gli uomini anziani non sposati"*: si combinano i bitmap con operazioni bit a bit:
$$\text{65+} \;\wedge\; \neg\,\text{Married} \;\wedge\; \text{Male}$$

| Person_ID | 65+ | Married | Male | Risultato |
|---|---|---|---|---|
| 1002 | 1 | 0 | 1 | ✅ |
| 1003 | 1 | 1 | 0 | ❌ |
| 1007 | 1 | 1 | 0 | ❌ |

^ex-bitmap

| ✅ Pro | ❌ Contro |
|---|---|
| Efficiente per query su **molti campi**, ciascuno con **pochi valori** | Non adatto a dati **molto dinamici** (insert/update/delete frequenti): un'operazione può «bloccare» migliaia di altre operazioni sulla stessa tabella (scritture concorrenti sull'indice) |

## 3.3 Indici in SQL

Non è parte dello standard, ma è presente in tutti i DBMS:

```sql
CREATE [UNIQUE] INDEX NomeIndice ON NomeTabella(ListaAttributi);
DROP INDEX NomeIndice;
```
`UNIQUE` specifica che `ListaAttributi` è una **superchiave**.

```sql
CREATE INDEX NomeIndice ON Impiegato(Cognome, Citta);
```

### Linee guida
1. Creare un indice se si recupera **frequentemente meno del 15%** delle righe di una **grande** tabella.
2. Indicizzare le colonne usate nei **join** (chiavi primarie e campi `UNIQUE` hanno indici automatici; può essere utile indicizzare le **chiavi esterne**).
3. Indicizzare colonne con valori **relativamente univoci** (eccetto per gli indici bitmap).

## 3.4 Esecuzione delle query

- Il **query processor** (o *ottimizzatore*) è un modulo del DBMS.
- Le query SQL sono **dichiarative** (alto livello); l'algebra relazionale offre un approccio **procedurale**.
- L'ottimizzatore sceglie la **strategia realizzativa** a partire dall'istruzione SQL.

```
 SQL
  │
  ▼
 Analisi lessicale, sintattica e semantica  ◄── schema
  │ algebra
  ▼
 Ottimizzazione algebrica                   ◄── Catalogo
  │ algebra                                     (schema, profili, dipendenze)
  ▼
 Ottimizzazione basata sui costi            ◄── profili
  │
  ▼
 Piano di accesso
```

## 3.5 Ottimizzazione algebrica

Il termine "ottimizzazione" è improprio: il processo usa **euristiche**.

> [!definition] Equivalenza
> Due espressioni sono **equivalenti** se producono lo stesso risultato **qualunque sia l'istanza** della base di dati. I DBMS eseguono espressioni equivalenti a quelle date ma meno costose.

**Euristica fondamentale**: eseguire **selezioni e proiezioni il prima possibile** per ridurre le dimensioni dei risultati intermedi (*push selections down*, *push projections down*).

**Push selections down** (con $A$ attributo di $R_2$):
$$\sigma_{A=10}(R_1 \bowtie R_2) \;\equiv\; R_1 \bowtie \sigma_{A=10}(R_2)$$

^ex-pushdown

La seconda forma riduce significativamente la dimensione del risultato intermedio.

### Rappresentazione interna: alberi
- **Foglie**: i dati (relazioni).
- **Nodi intermedi**: operatori (prima algebrici, poi operatori di accesso effettivi).

```
   σ A=10                         ⋈
     │                          /   \
     ⋈           ──────►      R1    σ A=10
   /   \       più efficiente          │
  R1    R2                            R2
```

## 3.6 Esecuzione delle operazioni

I DBMS implementano gli operatori algebrici tramite operazioni di **basso livello**, che possono realizzare più operatori "in un colpo solo":
- **fondamentali**: scansione e accesso diretto;
- **di livello più alto**: ordinamento e join.

| Accesso diretto possibile con | Scansione efficiente con |
|---|---|
| Indici (B-tree, …) | B+ tree (foglie collegate) |
| Strutture hash | |

## 3.7 Algoritmi di join

$R_1 \bowtie R_2$ è l'operazione **più costosa** (e molto frequente). I metodi più noti:

### 1) Nested-loop (*iteration join*)

```text
for each r ∈ R1 do               -- tabella esterna: scansione
    for each s ∈ R2 do           -- tabella interna: scansione o accesso via indice
        if r.C = s.C then output <r, s>
```

### 2) Merge-join (*merge scan*)

```text
(1) ordinare R1 e R2 su C (o usare B+ tree su C)
(2) i ← 1; j ← 1
    while i ≤ |R1| and j ≤ |R2| do
        if   R1[i].C = R2[j].C then output tuple con valore R1[i].C; avanza i, j
        elif R1[i].C > R2[j].C then j ← j + 1
        else                        i ← i + 1
```
Le due tabelle ordinate vengono scandite **in parallelo** (scan sinistro e scan destro), una sola volta ciascuna.

### 3) Hash-join

Funzione hash $h$ con codominio $0 \dots k$; bucket $G_0 \dots G_k$ per $R_1$ e $H_0 \dots H_k$ per $R_2$.
1. Applicare $h$ alle tuple di $R_1$ (sul valore di $C$) → bucket $G$.
2. Applicare $h$ alle tuple di $R_2$ → bucket $H$.
3. Per $i = 0 \dots k$: confrontare le tuple di $G_i$ con quelle di $H_i$.

**Esempio (hash pari/dispari)** — $R_1 = \{2,4,3,5,8,9\}$, $R_2 = \{5,4,12,3,13,8,11,14\}$:

| Bucket | $R_1$ | $R_2$ |
|---|---|---|
| Pari | 2, 4, 8 | 4, 12, 8, 14 |
| Dispari | 3, 5, 9 | 5, 3, 13, 11 |

^ex-hashjoin

Tuple con valori uguali finiscono **necessariamente nello stesso bucket**; dentro ogni bucket si confrontano tutte le coppie (nested-loop locale) e solo le chiavi uguali producono output → risultato: 4, 8, 3, 5.

## 3.8 Il processo di ottimizzazione basata sui costi

1. Si costruisce un **albero di decisione** con le alternative (**piani di esecuzione**).
2. Si stima il **costo** di ciascun piano.
3. Si sceglie il piano di **costo minore**.
4. L'ottimizzatore trova di solito una **"buona" soluzione**, non necessariamente l'ottimo: non deve essere troppo complesso, altrimenti si perde il vantaggio dell'ottimizzazione.

**Esempio**: $R \bowtie_1 S \bowtie_2 T$

- **Ordini di join**: $(R \bowtie_1 S) \bowtie_2 T$, $(R \bowtie_1 T) \bowtie_2 S$, $(S \bowtie_1 T) \bowtie_2 R$.
- Per ciascun join, 5 alternative: nested-loop con l'una o l'altra relazione interna, merge-scan, hash-join con hash sull'una o sull'altra relazione.

$$3 \text{ ordini} \times 5 \times 5 = \mathbf{75} \text{ strategie alternative}$$

^ex-piani

---

> [!tip] Concetti chiave
> Costo = numero di accessi a blocchi · buffer manager (`fix`, `unfix`, `setDirty`, `force`, `flush`; steal/no-steal) · record fissi/auto-descrittivi, spanned/unspanned · row vs column store · indici primari/secondari, densi/sparsi, multilivello · B-tree e B+ tree (split/merge) · hashing statico ed estendibile · indici bitmap · ottimizzazione algebrica (push down) e basata sui costi · nested-loop, merge-join, hash-join.

---

# Tabella riassuntiva per gli esercizi

Costi espressi in **numero di accessi a blocchi** ($N$ = numero di blocchi del file).

### Strutture di accesso

| Struttura | Ricerca puntuale | Ricerca per intervallo | Inserimenti / cancellazioni | Quando usarla |
|---|---|---|---|---|
| **File disordinato** (heap) | Scansione: in media $N/2$, $N$ se il valore non c'è | $N$ (scansione completa) | Economici (in coda o al posto dei cancellati) | Tabelle piccole, letture di tutta la tabella |
| **File ordinato** | Ricerca binaria, $\approx \log_2 N$ | Ricerca del primo valore + blocchi contigui | Costosi: bisogna mantenere l'ordine | Accessi frequenti sul campo di ordinamento |
| **Indice primario sparso** | Indice + 1 blocco di dati | Ottimo: i dati sono contigui | Costosi (overflow, riorganizzazioni) | Campo su cui è ordinato il file |
| **Indice secondario denso** | Indice + 1 blocco per record trovato | Ogni record può stare in un blocco diverso | Va aggiornato l'indice | Ricerche su un campo che non ordina il file |
| **B+ tree** | Profondità + 1 (tipicamente 3–4 + 1) | Ottimo: foglie collegate in lista | Split/merge locali e poco frequenti | Scelta standard dei DBMS |
| **Hash** | $\approx 1$ (caso peggiore costoso ma improbabile) | **Non adatto** | Buoni; la crescita richiede overflow o hashing dinamico | Solo condizioni di **uguaglianza** |
| **Bitmap** | AND/OR bit a bit fra bitmap | — | Poco adatto a dati molto dinamici | Colonne con **pochi valori distinti**, query su più campi |

### Indici: denso vs sparso, primario vs secondario

| | Denso | Sparso |
|---|---|---|
| Voci | Una per valore della chiave | Meno (tipicamente una per blocco) |
| Spazio | Maggiore | Minore: più indice in RAM |
| "Esiste la chiave $k$?" | Basta l'indice | Serve leggere il blocco |
| File disordinato (secondario) | ✅ | ❌ (solo come 2° livello sopra un denso) |

### Algoritmi di join

| Algoritmo | Idea | Prerequisiti / quando conviene |
|---|---|---|
| **Nested-loop** | Per ogni tupla della tabella esterna si scandisce l'interna | Sempre applicabile, anche a theta-join qualsiasi; conviene se l'interna è piccola o ha un **indice** sul campo di join |
| **Merge-join** | Scansione parallela dei due input ordinati | Input **ordinati** sul campo di join (o B+ tree); equi-join |
| **Hash-join** | Stessa $h$ su entrambe le tabelle, si confrontano solo i bucket corrispondenti $G_i$/$H_i$ | Solo **equi-join**; non serve ordinamento |

### Regole operative

| Procedura | Regola |
|---|---|
| **`fix`** | Pagina nel buffer? → restituisci l'indirizzo. Altrimenti scegli una pagina con contatore 0 (FIFO/LRU), salvala se *dirty*, sovrascrivila. Nessuna libera → **steal** (vittima) o **no-steal** (attesa) |
| **Cancellazione da indice sparso** | Record non chiave → indice invariato · record chiave → la voce diventa il primo record superstite · blocco svuotato → la voce sparisce |
| **Inserimento in indice sparso** senza spazio | Blocco di **overflow** oppure **split** del blocco |
| **Inserimento in B-tree** | Ricerca fino alla foglia · se c'è posto inserisci · se è piena **split**: la chiave mediana sale al genitore (e si propaga se serve) |
| **Cancellazione da B-tree** | Se un nodo scende sotto il 50% di riempimento → **merge** |
| **Hashing estendibile**, blocco pieno | Profondità locale $j < i$ → **split** del blocco ($j{+}1$) e aggiornamento dei puntatori · $j = i$ → **raddoppio del direttorio** ($i{+}1$) e split |
| **Ottimizzazione algebrica** | Sposta $\sigma$ e $\pi$ il più in basso possibile nell'albero (*push down*) |
| **Numero di piani** | (ordini di join possibili) × (alternative per ogni join), es. $3 \times 5 \times 5 = 75$ |

# Esercizi della lezione

**Domande poste nella lezione**

| # | Domanda | Dove |
|---|---|---|
| 1 | Vantaggi e svantaggi dei record auto-descrittivi | [[#^q-serializzazione]] |
| 2 | Cancellazione del record 50 e inserimento della chiave 35 nell'indice sparso | [[#^q-prova-tu]] |
| 3 | Svantaggi dei blocchi di overflow | [[#^q-overflow]] |

**Esempi da saper rifare**

| # | Esempio | Dove |
|---|---|---|
| 4 | Cancellazione da indice sparso (3 casi) | [[#^ex-canc-sparso]] |
| 5 | Split di una foglia di B-tree | [[#^ex-split]] |
| 6 | Collisioni con $h(k) = k \bmod 50$ | [[#^ex-collisioni]] |
| 7 | Hashing estendibile passo per passo | [[#^ex-hash-est]] |
| 8 | Indice bitmap: "uomini anziani non sposati" | [[#^ex-bitmap]] |
| 9 | *Push selections down* | [[#^ex-pushdown]] |
| 10 | Hash-join con hash pari/dispari | [[#^ex-hashjoin]] |
| 11 | Conteggio delle strategie per $R \bowtie S \bowtie T$ | [[#^ex-piani]] |
