# Quiz a risposta multipla — Basi di Dati (teoria)

> Domande sull'intero programma di teoria visto finora, divise per lezione. Ogni domanda ha **una sola risposta corretta**: la soluzione, con una breve spiegazione, è nel riquadro richiudibile sotto le opzioni (clicca su **Risposta** per aprirlo).

| Sezione | Lezione | Domande |
|---|---|---|
| [[#DB-02 — DBMS e modello relazionale\|DB-02]] | [[02 - DBMS e Modello Relazionale]] | 1–11 |
| [[#DB-03 — Algebra relazionale\|DB-03]] | [[03 - Algebra relazionale]] | 12–24 |
| [[#DB-04 — Architettura fisica dei DBMS\|DB-04]] | [[04 - Architettura fisica dei DBMS]] | 25–41 |
| [[#DB-05 — Caratteristiche evolute dei DBMS\|DB-05]] | [[05 - Caratteristiche evolute dei DBMS]] | 42–52 |
| [[#DB-06 — Database per Big Data ed AI\|DB-06]] | [[06 - Database per Big Data ed AI]] | 53–62 |

---

## DB-02 — DBMS e modello relazionale

### 1. Che cosa garantisce l'indipendenza fisica dei dati?
- **A)** Le viste possono essere modificate senza modificare lo schema logico
- **B)** La realizzazione fisica dei dati può cambiare senza modificare i programmi
- **C)** Ogni utente vede solo i dati che è autorizzato a vedere
- **D)** I dati sopravvivono alla terminazione dei programmi che li usano

> [!success]- Risposta
> **B)** Il livello logico e quello esterno sono indipendenti dal fisico: si possono cambiare file, indici e strutture di memorizzazione senza toccare i programmi. La A descrive l'indipendenza **logica**, la C la privatezza, la D la persistenza.

### 2. Quale di queste istruzioni SQL appartiene al DDL?
- **A)** `SELECT`
- **B)** `INSERT`
- **C)** `UPDATE`
- **D)** `ALTER TABLE`

> [!success]- Risposta
> **D)** Il DDL (*Data Definition Language*) definisce e modifica gli **schemi**: `CREATE`, `ALTER`, `DROP TABLE`. `SELECT`, `INSERT` e `UPDATE` agiscono sulle **istanze** e appartengono al DML.

### 3. Quale affermazione su una relazione matematica è FALSA?
- **A)** L'ordine delle tuple è irrilevante
- **B)** Può contenere due tuple identiche
- **C)** In ogni tupla l'i-esimo valore proviene dall'i-esimo dominio
- **D)** È un sottoinsieme di un prodotto cartesiano

> [!success]- Risposta
> **B)** Una relazione è un **insieme** di tuple, e un insieme non contiene elementi ripetuti. Tutte le altre affermazioni sono vere.

### 4. Che cos'è una chiave?
- **A)** Una superchiave formata da un solo attributo
- **B)** Un insieme di attributi che non contiene valori nulli
- **C)** Una superchiave minimale
- **D)** L'insieme di tutti gli attributi della relazione

> [!success]- Risposta
> **C)** Una chiave è una superchiave da cui non si può togliere alcun attributo senza perdere l'unicità. Può avere più attributi (es. `<Studente, Corso>`); l'insieme di tutti gli attributi è sempre una superchiave, ma non sempre è minimale.

### 5. Perché ogni relazione ha almeno una chiave?
- **A)** Perché una relazione non contiene tuple uguali, quindi l'insieme di tutti i suoi attributi è una superchiave, che contiene una superchiave minimale
- **B)** Perché il DBMS aggiunge automaticamente una chiave artificiale
- **C)** Perché ogni relazione deve avere una chiave primaria dichiarata
- **D)** Perché gli attributi non possono assumere valori nulli

> [!success]- Risposta
> **A)** È la dimostrazione del teorema di esistenza delle chiavi: tutti gli attributi insieme distinguono sempre le tuple (che sono distinte), e togliendo attributi finché possibile si arriva a una chiave.

### 6. In `ESAMI(Studente, Corso, Voto)` uno studente può sostenere più esami, ma non lo stesso corso due volte. Qual è la chiave?
- **A)** `Studente`
- **B)** `<Studente, Voto>`
- **C)** `Corso`
- **D)** `<Studente, Corso>`

> [!success]- Risposta
> **D)** `Studente` da solo impedirebbe più esami per studente; `Corso` da solo impedirebbe a più studenti di sostenere lo stesso corso; `<Studente, Voto>` impedirebbe lo stesso voto in due esami diversi.

### 7. Quale attributo può assumere il valore `NULL`?
- **A)** Un attributo della chiave primaria
- **B)** Una chiave esterna che non fa parte della chiave primaria
- **C)** Una chiave esterna che fa parte della chiave primaria
- **D)** Nessuno: `NULL` non è ammesso nel modello relazionale

> [!success]- Risposta
> **B)** Gli attributi della chiave primaria non possono essere nulli, perché servono a identificare le tuple e a riferirle da altre tabelle. Una chiave esterna che non fa parte della PK può invece essere `NULL` (es. una recensione di un utente eliminato).

### 8. Il vincolo `DataFine ≥ DataInizio` è un vincolo:
- **A)** di dominio
- **B)** di chiave
- **C)** di tupla
- **D)** referenziale

> [!success]- Risposta
> **C)** Confronta valori di attributi diversi **della stessa tupla**. Un vincolo di dominio riguarda un singolo valore (es. `Stipendio > 0`), uno referenziale coinvolge tabelle diverse.

### 9. Un vincolo di integrità referenziale tra l'attributo X di R1 e la relazione R2 impone che:
- **A)** i valori di X in R1 siano tutti distinti
- **B)** ogni valore della chiave primaria di R2 compaia in X
- **C)** X sia la chiave primaria di R1
- **D)** i valori di X in R1 compaiano come valori della chiave primaria di R2

> [!success]- Risposta
> **D)** È la definizione di chiave esterna: ogni riferimento deve puntare a una tupla esistente di R2. Non serve che i valori di X siano distinti (uno studente compare in molti esami) né che tutte le tuple di R2 siano riferite.

### 10. `RIGA_ORDINE(NumeroOrdine, CodiceProdotto, Quantità)` ha chiave primaria `<NumeroOrdine, CodiceProdotto>` e chiave esterna `CodiceProdotto → PRODOTTO`. Quale azione NON si può usare alla cancellazione di un prodotto?
- **A)** `ON DELETE SET NULL`
- **B)** `ON DELETE CASCADE`
- **C)** `ON DELETE RESTRICT`
- **D)** `ON DELETE NO ACTION`

> [!success]- Risposta
> **A)** `CodiceProdotto` fa parte della chiave primaria di `RIGA_ORDINE`, quindi non può diventare `NULL`. Le altre azioni sono applicabili (anche se `CASCADE` cancellerebbe righe d'ordine storiche).

### 11. Quale affermazione sulle chiavi è vera?
- **A)** Una relazione può avere più chiavi, ma una sola chiave primaria
- **B)** Una chiave può essere sottoinsieme di un'altra chiave
- **C)** Ogni attributo appartiene al massimo a una chiave
- **D)** Esistono attributi che non appartengono a nessuna superchiave

> [!success]- Risposta
> **A)** Tra le chiavi se ne sceglie una come primaria. La B viola la minimalità; la C è falsa (es. `<A,B>` e `<A,C>` possono essere entrambe chiavi); la D è falsa perché l'insieme di tutti gli attributi è sempre una superchiave.

---

## DB-03 — Algebra relazionale

### 12. L'algebra relazionale è un linguaggio:
- **A)** dichiarativo
- **B)** procedurale
- **C)** di definizione dei dati
- **D)** di programmazione general purpose

> [!success]- Risposta
> **B)** Specifica **come** ottenere il risultato, con una sequenza di operatori. SQL è (in gran parte) dichiarativo: specifica **cosa** si vuole ottenere.

### 13. L'unione R ∪ S è definita solo se:
- **A)** R e S hanno la stessa cardinalità
- **B)** R e S hanno almeno un attributo in comune
- **C)** R e S sono definite sugli stessi attributi
- **D)** R e S non hanno tuple in comune

> [!success]- Risposta
> **C)** Unione, intersezione e differenza richiedono lo **stesso schema**; se i nomi degli attributi sono diversi si usa prima la ridenominazione $\rho$.

### 14. Se X è una superchiave di R, quante tuple contiene $\pi_X(R)$?
- **A)** Una sola
- **B)** Al più $|R|$, ma possono essere meno
- **C)** Più di $|R|$
- **D)** Esattamente $|R|$

> [!success]- Risposta
> **D)** Due tuple distinte di R hanno valori diversi su una superchiave, quindi nella proiezione non collassano. In generale una proiezione ha al più $|R|$ tuple (B vale solo se X non è superchiave).

### 15. La relazione P ha un attributo `Età` con alcuni valori `NULL`. Cosa vale per $\sigma_{\text{Età}>30}(P) \cup \sigma_{\text{Età}\le 30}(P)$?
- **A)** È sempre uguale a P
- **B)** Può contenere meno tuple di P
- **C)** Può contenere più tuple di P
- **D)** È sempre vuota

> [!success]- Risposta
> **B)** Un confronto con `NULL` non è mai vero, quindi le tuple con `Età` nulla non soddisfano nessuna delle due condizioni e mancano dal risultato. Per includerle serve anche $\sigma_{\text{Età IS NULL}}(P)$.

### 16. Che cosa produce il join naturale tra R(A, B) e S(C, D)?
- **A)** Un risultato sempre vuoto
- **B)** Un errore di sintassi
- **C)** Il prodotto cartesiano R × S
- **D)** L'unione R ∪ S

> [!success]- Risposta
> **C)** Senza attributi in comune la condizione implicita del join naturale è vuota: ogni tupla di R si combina con ogni tupla di S.

### 17. R1(A, B) e R2(B, C): B è chiave di R2 ed esiste una chiave esterna (senza `NULL`) da R1.B a R2. Quante tuple ha $R_1 \bowtie R_2$?
- **A)** Esattamente $|R_1|$
- **B)** Esattamente $|R_2|$
- **C)** Esattamente $|R_1| \cdot |R_2|$
- **D)** Un numero qualsiasi tra 0 e $|R_1| \cdot |R_2|$

> [!success]- Risposta
> **A)** Per la chiave esterna ogni tupla di R1 trova almeno una corrispondenza; poiché B è chiave di R2, ne trova **esattamente una**.

### 18. Qual è la differenza tra l'equi-join $R \bowtie_{A=B} S$ e il join naturale?
- **A)** L'equi-join conserva anche le tuple senza corrispondenza
- **B)** Il join naturale ammette condizioni con `<` e `>`
- **C)** Nell'equi-join le colonne confrontate restano entrambe; nel join naturale gli attributi comuni compaiono una sola volta
- **D)** Nessuna: producono sempre lo stesso schema

> [!success]- Risposta
> **C)** Il join naturale confronta implicitamente gli attributi con lo stesso nome e li unifica; l'equi-join confronta attributi indicati esplicitamente (anche con nomi diversi) e li tiene entrambi. Nessuno dei due conserva le tuple senza corrispondenza (lo fanno i join esterni).

### 19. Che cosa fa il join esterno sinistro $R ⟕ S$?
- **A)** Conserva solo le tuple di R che hanno corrispondenza in S
- **B)** Conserva tutte le tuple di S, completando con `NULL`
- **C)** Restituisce l'unione di R e S
- **D)** Conserva tutte le tuple di R, completando con `NULL` quelle senza corrispondenza

> [!success]- Risposta
> **D)** Il join esterno estende con `NULL` le tuple che il join interno scarterebbe; nel sinistro sono quelle del **primo** operando. La A è il join interno, la B il join esterno destro.

### 20. Su `Impiegati(Matricola, Nome, Stipendio)`, quale espressione è ERRATA?
- **A)** $\pi_{\text{Nome}}(\sigma_{\text{Stipendio}>50}(\text{Impiegati}))$
- **B)** $\sigma_{\text{Stipendio}>50}(\pi_{\text{Nome}}(\text{Impiegati}))$
- **C)** $\sigma_{\text{Stipendio}>50}(\pi_{\text{Nome},\text{Stipendio}}(\text{Impiegati}))$
- **D)** $\pi_{\text{Matricola}}(\text{Impiegati})$

> [!success]- Risposta
> **B)** Dopo $\pi_{\text{Nome}}$ l'attributo `Stipendio` non esiste più, quindi la selezione non può usarlo. La C è corretta perché la proiezione conserva `Stipendio`.

### 21. Come si trova il prezzo massimo di una relazione PRODOTTO in algebra relazionale?
- **A)** Con l'operatore MAX
- **B)** Togliendo da tutti i prezzi quelli minori di almeno un altro prezzo (self-join e differenza)
- **C)** Con una proiezione, che elimina i duplicati
- **D)** Non si può esprimere

> [!success]- Risposta
> **B)** L'algebra relazionale non ha un operatore MAX, ma il massimo si esprime: con due copie ridenominate dei prezzi si trovano quelli che sono minori di qualcun altro, e la differenza lascia solo il più alto.

### 22. Quale di queste interrogazioni NON si può esprimere in algebra relazionale?
- **A)** Tutti i superiori, diretti e indiretti, di ogni impiegato
- **B)** Gli studenti che non hanno sostenuto esami
- **C)** Le coppie di prodotti diversi con lo stesso nome
- **D)** I corsi che nessuno ha superato

> [!success]- Risposta
> **A)** È una **chiusura transitiva**, che richiede ricorsione: con un numero fisso di join si raggiunge solo una profondità fissa. Le altre si risolvono con differenza (B, D) e self-join (C).

### 23. A che cosa equivale il theta-join $R \bowtie_F S$?
- **A)** $\sigma_F(R) \times S$
- **B)** $\sigma_F(R \times S)$
- **C)** $\pi_F(R \times S)$
- **D)** $\sigma_F(R \cup S)$

> [!success]- Risposta
> **B)** È un prodotto cartesiano seguito da una selezione sulla condizione F. La A sarebbe corretta solo se F riguardasse esclusivamente attributi di R.

### 24. Con `ESAMI(Studente, Corso, Voto)`, quale espressione trova gli studenti che hanno preso **solo** voti ≥ 27?
- **A)** $\pi_{\text{Studente}}(\sigma_{\text{Voto}\ge 27}(\text{ESAMI}))$
- **B)** $\pi_{\text{Studente}}(\text{ESAMI}) \cap \pi_{\text{Studente}}(\sigma_{\text{Voto}<27}(\text{ESAMI}))$
- **C)** $\pi_{\text{Studente}}(\sigma_{\text{Voto}<27}(\text{ESAMI}))$
- **D)** $\pi_{\text{Studente}}(\text{ESAMI}) - \pi_{\text{Studente}}(\sigma_{\text{Voto}<27}(\text{ESAMI}))$

> [!success]- Risposta
> **D)** "Solo" si esprime come *tutti meno quelli con almeno un'eccezione*. La A trova chi ha **almeno un** voto ≥ 27, includendo chi ha anche voti più bassi.

---

## DB-04 — Architettura fisica dei DBMS

### 25. Nel modello di costo usato per i DBMS, da che cosa dipende il costo di un'operazione?
- **A)** Dal numero di istruzioni eseguite dalla CPU
- **B)** Dal numero di tuple del risultato
- **C)** Dal numero di blocchi letti o scritti in memoria secondaria
- **D)** Dalla dimensione della memoria centrale

> [!success]- Risposta
> **C)** Un accesso al disco costa quattro o più ordini di grandezza più di un'operazione in RAM, quindi si contano solo gli accessi ai blocchi.

### 26. Che cosa fa la primitiva `fix` quando la pagina richiesta è già nel buffer?
- **A)** La rilegge dal disco per avere la versione aggiornata
- **B)** La scrive su disco prima di restituirla
- **C)** Azzera il suo contatore
- **D)** Ne restituisce l'indirizzo e incrementa il contatore, senza accedere al disco

> [!success]- Risposta
> **D)** È il caso che giustifica il buffer: una pagina già presente non costa accessi al disco. Il contatore registra che un'altra transazione la sta usando.

### 27. A che cosa serve il bit *dirty* di una pagina del buffer?
- **A)** Indica che la pagina è stata modificata e va riscritta su disco prima di essere sostituita
- **B)** Indica che la pagina è usata da almeno una transazione
- **C)** Indica che la pagina contiene dati corrotti
- **D)** Riserva la pagina a una sola transazione

> [!success]- Risposta
> **A)** Viene impostato da `setDirty` e permette di **rimandare** la scrittura su disco. La B è il ruolo del contatore.

### 28. Tutte le pagine del buffer hanno contatore maggiore di zero e la politica è *no-steal*. Che cosa succede a una nuova `fix` di una pagina non presente?
- **A)** La richiesta viene messa in attesa
- **B)** Si sceglie una pagina vittima tra quelle in uso
- **C)** Il buffer viene ingrandito
- **D)** La transazione viene annullata

> [!success]- Risposta
> **A)** Con *no-steal* non si sottrae una pagina in uso: si aspetta che qualche transazione la rilasci. La B è il comportamento *steal*.

### 29. Qual è la differenza tra `force` e `flush`?
- **A)** `force` scrive una pagina su disco in modo sincrono, `flush` in modo asincrono
- **B)** `force` legge una pagina, `flush` la scrive
- **C)** `force` è usata solo dal buffer manager, `flush` solo dalle transazioni
- **D)** Nessuna, sono sinonimi

> [!success]- Risposta
> **A)** Con `force` chi la chiama attende che la scrittura sia completata (es. al commit); `flush` scrive in background quando il buffer manager lo ritiene opportuno.

### 30. Quando è necessaria un'organizzazione *spanned* dei record?
- **A)** Quando i record hanno formato auto-descrittivo
- **B)** Quando un record è più grande di un blocco
- **C)** Quando il file è ordinato
- **D)** Quando si usa un column store

> [!success]- Risposta
> **B)** *Spanned* significa che un record può essere diviso su più blocchi. *Unspanned* è più semplice e veloce, ma richiede che ogni record stia in un blocco.

### 31. Per quale tipo di carico è più vantaggioso un column store?
- **A)** Lettura e modifica di molti campi dello stesso record
- **B)** Inserimenti frequenti di singole righe
- **C)** Statistiche e analisi che leggono poche colonne di moltissime righe
- **D)** Accesso diretto a un record tramite la sua chiave primaria

> [!success]- Risposta
> **C)** Scrivendo i dati colonna per colonna si leggono solo le colonne necessarie. Il row store è più efficiente per leggere e scrivere più campi dello stesso record.

### 32. Su quale file si può costruire un indice sparso?
- **A)** Su qualsiasi file, ordinato o no
- **B)** Solo su un file disordinato
- **C)** Solo su un file ordinato secondo il campo dell'indice
- **D)** Solo su un file con record a lunghezza variabile

> [!success]- Risposta
> **C)** L'indice sparso ha una voce per blocco: funziona solo se dalla prima chiave di un blocco si può dedurre l'intervallo di chiavi che contiene. Su un file disordinato serve un indice denso.

### 33. Quale vantaggio ha un indice denso rispetto a uno sparso?
- **A)** Occupa meno spazio
- **B)** Si può costruire solo su file ordinati
- **C)** Non richiede aggiornamenti quando si inseriscono record
- **D)** Permette di sapere se esiste un record con una certa chiave guardando solo l'indice

> [!success]- Risposta
> **D)** Contiene una voce per ogni valore della chiave. L'indice sparso, al contrario, occupa meno spazio (quindi una parte maggiore sta in RAM).

### 34. Un file ordinato ha 2 record per blocco e indice sparso `10, 30, 50`; BLOCK2 contiene 30 e 40. Che cosa succede all'indice se si cancella il record 30?
- **A)** Nulla
- **B)** La voce 30 diventa 40
- **C)** La voce 30 viene rimossa
- **D)** L'indice va ricostruito da capo

> [!success]- Risposta
> **B)** 30 era la chiave del blocco: la voce si aggiorna al primo record superstite. Cancellando 40 l'indice non cambierebbe; cancellando entrambi la voce verrebbe rimossa.

### 35. Che cosa distingue un B+ tree da un B-tree?
- **A)** Il B+ tree non è bilanciato
- **B)** Nel B+ tree le foglie sono collegate in lista e solo loro puntano ai dati
- **C)** Il B+ tree ha nodi con un solo figlio
- **D)** Il B+ tree non supporta gli inserimenti

> [!success]- Risposta
> **B)** Per questo il B+ tree è ottimo per le ricerche su intervalli: trovato il primo valore, si scorrono le foglie. Nel B-tree anche i nodi intermedi puntano direttamente ai dati.

### 36. Quale proprietà ha il riempimento dei nodi di un B-tree?
- **A)** È sempre del 100%
- **B)** Può scendere a zero
- **C)** Resta sempre superiore al 50%
- **D)** È sempre esattamente del 70%

> [!success]- Risposta
> **C)** Split e merge mantengono ogni nodo pieno almeno a metà; il riempimento **medio** è circa del 70%.

### 37. Per quale interrogazione un file hash è poco efficiente?
- **A)** `WHERE Matricola = 104`
- **B)** `WHERE Voto BETWEEN 27 AND 30`
- **C)** `WHERE Codice = 'BD'`
- **D)** Nessuna: è sempre la struttura migliore

> [!success]- Risposta
> **B)** La funzione hash disperde valori vicini in blocchi lontani, quindi per un intervallo bisogna leggere tutto il file. Per le condizioni di uguaglianza (A, C) costa circa 1 accesso.

### 38. Nell'hashing estendibile un blocco pieno ha profondità locale $j = 1$, mentre la profondità globale è $i = 2$. Che cosa succede inserendovi una nuova chiave?
- **A)** Si raddoppia il direttorio
- **B)** Si crea un blocco di overflow
- **C)** La chiave viene rifiutata
- **D)** Il blocco si divide in due senza modificare la dimensione del direttorio

> [!success]- Risposta
> **D)** Con $j < i$ il blocco è puntato da più voci del direttorio: basta dividerlo e ripartire le voci. Il direttorio raddoppia solo quando $j = i$.

### 39. In quale caso è adatto un indice bitmap?
- **A)** Colonne con pochi valori distinti, interrogate in combinazione con altre
- **B)** Colonne con valori quasi tutti distinti, come una chiave primaria
- **C)** Tabelle aggiornate molto frequentemente
- **D)** Ricerche su intervalli di date

> [!success]- Risposta
> **A)** Si crea una sequenza di bit per ogni valore, e le condizioni su più colonne diventano AND/OR bit a bit. È poco adatto a dati molto dinamici, perché le scritture concorrenti sull'indice si bloccano a vicenda.

### 40. Con A attributo di R2, perché l'ottimizzatore riscrive $\sigma_{A=10}(R_1 \bowtie R_2)$ come $R_1 \bowtie \sigma_{A=10}(R_2)$?
- **A)** Perché la seconda forma riduce la dimensione del risultato intermedio del join
- **B)** Perché la prima forma è sintatticamente errata
- **C)** Perché le due espressioni danno risultati diversi
- **D)** Perché il join va sempre eseguito per ultimo

> [!success]- Risposta
> **A)** È l'euristica *push selections down*: le due espressioni sono **equivalenti**, ma filtrando prima il join lavora su meno tuple.

### 41. Quale algoritmo di join si può usare SOLO per gli equi-join?
- **A)** Nested-loop
- **B)** Hash-join
- **C)** Prodotto cartesiano
- **D)** Nessuno: tutti valgono per qualsiasi condizione

> [!success]- Risposta
> **B)** Due tuple vanno confrontate solo se finiscono nello stesso bucket, il che ha senso solo per l'uguaglianza. Il nested-loop funziona con qualsiasi condizione.

---

## DB-05 — Caratteristiche evolute dei DBMS

### 42. Quale proprietà ACID garantisce che una transazione venga eseguita per intero o per niente?
- **A)** Consistenza
- **B)** Isolamento
- **C)** Atomicità
- **D)** Durability

> [!success]- Risposta
> **C)** Se la transazione non si completa (guasto, violazione di vincoli, richiesta dell'utente), il DBMS annulla tutte le sue operazioni.

### 43. Durante una transazione un vincolo di chiave esterna viene violato, ma prima del commit viene inserita la tupla mancante. Che cosa succede?
- **A)** La transazione viene annullata appena il vincolo è violato
- **B)** La transazione si conclude correttamente, perché alla fine il vincolo è rispettato
- **C)** Il DBMS cancella la tupla che viola il vincolo
- **D)** Il vincolo viene disattivato

> [!success]- Risposta
> **B)** La consistenza richiede che i vincoli siano soddisfatti **al termine** della transazione; violazioni temporanee durante l'esecuzione sono ammesse.

### 44. Due transazioni concorrenti leggono entrambe che il posto 07F è libero e lo prenotano entrambe. Le due transazioni vengono completate per intero. Quale proprietà è violata?
- **A)** Atomicità
- **B)** Durability
- **C)** Consistenza dei vincoli di dominio
- **D)** Isolamento

> [!success]- Risposta
> **D)** Entrambe le transazioni sono complete, quindi l'atomicità è rispettata: il problema è l'**interferenza** tra transazioni concorrenti, che l'isolamento deve impedire rendendo il risultato equivalente a un'esecuzione seriale.

### 45. Che cosa permette al DBMS di garantire che un bonifico confermato non vada perso in caso di blackout?
- **A)** Il log scritto su memoria stabile, riletto al riavvio
- **B)** L'indice primario sulla tabella dei conti
- **C)** La politica *no-steal* del buffer
- **D)** I trigger AFTER

> [!success]- Risposta
> **A)** È la **durability**: le operazioni e il record di `COMMIT` sono nel log, che sopravvive al guasto; al riavvio il DBMS lo rilegge e ripristina gli effetti delle transazioni confermate.

### 46. Un programma Python usa `psycopg2` con le impostazioni predefinite, esegue alcuni `UPDATE` e termina senza chiamare `conn.commit()`. Che cosa succede alle modifiche?
- **A)** Vengono salvate, perché l'autocommit è attivo
- **B)** Vengono perse, perché l'autocommit è disattivato di default
- **C)** Vengono salvate solo le prime
- **D)** Vengono salvate alla connessione successiva

> [!success]- Risposta
> **B)** A differenza di pgAdmin, `psycopg2` apre implicitamente una transazione al primo `execute()`, che resta aperta fino a `commit()` o `rollback()`. Per avere l'autocommit serve `conn.autocommit = True`.

### 47. Che cosa può fare un trigger in modalità `BEFORE`?
- **A)** Modificare liberamente altre tabelle del database
- **B)** Agire solo dopo che la modifica è stata scritta
- **C)** Condizionare i valori `New` prima che la modifica venga eseguita
- **D)** Essere attivato solo da istruzioni `SELECT`

> [!success]- Risposta
> **C)** I trigger `BEFORE` servono a verificare una modifica prima che avvenga e a "modificare la modifica" (es. limitare un aumento di stipendio), ma non possono modificare lo stato del database.

### 48. Quali variabili si usano in un trigger statement-level?
- **A)** `old_table` e `new_table`
- **B)** `old` e `new`
- **C)** `before` e `after`
- **D)** Nessuna

> [!success]- Risposta
> **A)** Un trigger statement-level viene eseguito una volta per comando e vede l'intera porzione di tabella modificata. `old` e `new` (singole tuple) sono le variabili dei trigger row-level.

### 49. Più trigger sono associati allo stesso evento. In che ordine vengono eseguiti?
- **A)** Prima gli `AFTER`, poi la modifica, poi i `BEFORE`
- **B)** Prima i `BEFORE`, poi la modifica, poi gli `AFTER`
- **C)** In ordine casuale
- **D)** Tutti contemporaneamente, dopo la modifica

> [!success]- Risposta
> **B)** Tra trigger della stessa categoria l'ordine dipende dal DBMS (data di creazione o ordine alfabetico).

### 50. Che cosa si può concludere se il grafo di triggering è aciclico?
- **A)** Che l'esecuzione dei trigger termina sempre
- **B)** Che l'esecuzione dei trigger non termina mai
- **C)** Che i trigger non vengono mai attivati
- **D)** Nulla sulla terminazione

> [!success]- Risposta
> **A)** Senza cicli una catena di attivazioni non può ripetersi all'infinito. Se il grafo ha cicli la terminazione **non è garantita**, ma può comunque avvenire (come per T1 e T2 nella lezione).

### 51. Che cosa fa `REVOKE ... CASCADE`?
- **A)** Blocca la revoca se il privilegio è stato trasmesso ad altri
- **B)** Revoca il privilegio e lo propaga a tutte le tabelle collegate da chiavi esterne
- **C)** Revoca il privilegio anche agli utenti a cui era stato trasmesso
- **D)** Revoca tutti i privilegi dell'utente

> [!success]- Risposta
> **C)** Se gli utenti avevano concesso il privilegio ad altri (con `WITH GRANT OPTION`), la revoca si propaga in cascata. La A descrive `RESTRICT`.

### 52. Come si permette a un utente di vedere solo alcune righe di una tabella?
- **A)** Con `GRANT SELECT` su alcune colonne
- **B)** Definendo una vista con una condizione di selezione e concedendo i privilegi sulla vista
- **C)** Con un trigger `BEFORE SELECT`
- **D)** Non è possibile in SQL

> [!success]- Risposta
> **B)** La vista filtra le righe e l'utente riceve i privilegi solo su di essa, non sulla tabella di base. `GRANT` su colonne (A) limita le colonne, non le righe.

---

## DB-06 — Database per Big Data ed AI

### 53. Quali sono le "3V" dei Big Data?
- **A)** Volume, Velocità, Varietà
- **B)** Volume, Valore, Veridicità
- **C)** Velocità, Visibilità, Variabilità
- **D)** Validità, Volume, Vincoli

> [!success]- Risposta
> **A)** Quantità di dati, rapidità con cui vengono generati, eterogeneità dei formati.

### 54. Che cosa significa il termine NoSQL?
- **A)** Database che rifiutano completamente il linguaggio SQL
- **B)** Not only SQL: una famiglia di modelli di dati diversi dal relazionale classico
- **C)** Database privi di qualsiasi linguaggio di interrogazione
- **D)** Database relazionali senza chiavi

> [!success]- Risposta
> **B)** Il nome indica una famiglia di modelli (chiave-valore, colonne, documenti, grafi), non il rifiuto di SQL: molti sistemi NoSQL offrono linguaggi SQL-like.

### 55. Nell'approccio BASE, che cosa significa la "E"?
- **A)** Efficient
- **B)** Exclusive
- **C)** Eventually consistent
- **D)** Encrypted

> [!success]- Risposta
> **C)** *Basically Available, Soft state, Eventually consistent*: il sistema diventa consistente **alla fine**, quando gli aggiornamenti si sono propagati, invece che in ogni istante come in ACID.

### 56. Quale categoria NoSQL è più adatta a una cache di sessione per un sito ad alto traffico?
- **A)** Graph database
- **B)** Wide-column store
- **C)** Document store
- **D)** Key-value store

> [!success]- Risposta
> **D)** Serve un accesso immediato per chiave (l'identificativo di sessione) senza interrogazioni sul contenuto: è il caso d'uso tipico dei key-value store (es. Redis).

### 57. Quale categoria è più adatta a suggerire "persone che potresti conoscere" in un social network?
- **A)** Graph database
- **B)** Key-value store
- **C)** Wide-column store
- **D)** Data warehouse

> [!success]- Risposta
> **A)** La query è una traversata delle relazioni (amici di amici), che in un modello relazionale richiederebbe molti join.

### 58. Secondo il teorema CAP, che cosa bisogna scegliere in presenza di una partizione di rete?
- **A)** Tra latenza e consistenza
- **B)** Tra consistenza e disponibilità
- **C)** Tra disponibilità e tolleranza alle partizioni
- **D)** Nulla: si possono avere tutte e tre le proprietà

> [!success]- Risposta
> **B)** Se il sistema deve tollerare le partizioni (P), non può essere contemporaneamente consistente (C) e disponibile (A). La A descrive la parte "Else" del teorema PACELC.

### 59. Un sistema classificato PA/EL (es. Cassandra):
- **A)** sceglie la consistenza sia in partizione sia in condizioni normali
- **B)** non tollera le partizioni
- **C)** in partizione sceglie la disponibilità, altrimenti la bassa latenza
- **D)** garantisce transazioni ACID distribuite

> [!success]- Risposta
> **C)** PACELC: in caso di **P**artizione sceglie **A** (disponibilità), **E**lse (senza partizioni) sceglie **L** (latenza). La A descrive PC/EC, tipico dei sistemi bancari.

### 60. Una fintech deve gestire pagamenti atomici tra milioni di utenti su più data center. Che tipo di sistema serve?
- **A)** Un key-value store puro
- **B)** Un data lake
- **C)** Un database vettoriale
- **D)** Un sistema NewSQL (es. Spanner, CockroachDB)

> [!success]- Risposta
> **D)** Servono insieme transazioni ACID su più record (addebitare A e accreditare B) e scalabilità orizzontale: è il compromesso che i sistemi NewSQL cercano di offrire. Un key-value store puro non garantisce transazioni multi-record.

### 61. Che cosa caratterizza un data lake rispetto a un data warehouse?
- **A)** Conserva dati grezzi ed eterogenei e applica lo schema solo in lettura (*schema-on-read*)
- **B)** Contiene solo dati puliti e strutturati
- **C)** È pensato principalmente per la reportistica di business
- **D)** Applica lo schema in fase di scrittura (*schema-on-write*)

> [!success]- Risposta
> **A)** Il data lake memorizza i dati così come arrivano (log, JSON, immagini…), tipicamente su storage a oggetti, per data science e machine learning. B, C e D descrivono il data warehouse.

### 62. Che cos'è la RAG (*Retrieval-Augmented Generation*)?
- **A)** Una tecnica di compressione degli embedding
- **B)** Un linguaggio di interrogazione per database a grafo
- **C)** Una tecnica in cui un LLM recupera documenti rilevanti (es. da un database vettoriale) e li usa come contesto per rispondere
- **D)** Un algoritmo di riaddestramento dei modelli linguistici

> [!success]- Risposta
> **C)** È il modo più comune per dare a un LLM accesso a conoscenza aggiornata o privata **senza riaddestrarlo**; i documenti vengono cercati per similarità tra embedding.
