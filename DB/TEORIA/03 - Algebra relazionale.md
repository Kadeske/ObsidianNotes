# 03 - Algebra relazionale

> Fonte: [[DB/TEORIA/PDF/03 - Algebra relazionale.pdf]]
> Slide tratte in parte da Atzeni, Ceri, Fraternali, Paraboschi, Torlone — *Basi di dati*, McGraw-Hill

---

## 1. Introduzione

- **Algebra** (in generale): branca della matematica che studia come rappresentare informazione tramite simboli e le regole per manipolarli.
- **Algebra relazionale**: un'algebra per modellare dati in strutture relazionali ed **esprimere interrogazioni**.

### Perché studiarla
- Definita nei primi anni '70 da **Edgar F. Codd** (IBM).
- È il **fondamento teorico** dei linguaggi di interrogazione relazionali, SQL compreso.
- È la base per **implementare e ottimizzare** i DBMS reali.
- Si studia di pari passo con SQL: gli operatori di base sono gli stessi, SQL ne aggiunge altri per query più complesse.

### Linguaggi dichiarativi vs procedurali

| **Dichiarativi** | **Procedurali** |
|---|---|
| Specificano le **proprietà** del risultato: *che cosa* si vuole | Specificano le **modalità** di generazione del risultato: *come* ottenerlo |
| Es. SQL (parzialmente) | Es. **algebra relazionale** |

### Che cos'è
Un insieme di **operatori** che:
1. operano su **relazioni**;
2. producono **relazioni** come risultato;
3. possono essere **composti** fra loro (*chiusura*).

### Panoramica degli operatori

| Categoria | Operatori |
|---|---|
| Insiemistici | Unione $\cup$, intersezione $\cap$, differenza $-$ |
| Ridenominazione | $\rho$ (`REN`) — cambia nome ad attributi e relazioni |
| Selezione e proiezione | $\sigma$ (`SEL`), $\pi$ (`PROJ`) — filtrano tuple e attributi |
| Join | Join naturale $\bowtie$, prodotto cartesiano $\times$, theta-join, equi-join, join esterni |

> [!info] Notazione
> Nel corso si usano due notazioni equivalenti: **a parole** (`SEL`, `PROJ`, `REN`, `JOIN`, `NAT_JOIN`) e **matematica** ($\sigma$, $\pi$, $\rho$, $\bowtie$, $\times$).

---

## 2. Operatori insiemistici

- Le relazioni sono **insiemi** di ennuple, e i risultati devono essere relazioni.
- Unione, intersezione e differenza si applicano **solo a relazioni definite sugli stessi attributi**.

**Esempio**

| **Laureati** | | |
|---|---|---|
| Matricola | Nome | Età |
| 7274 | Rossi | 42 |
| 7432 | Gialli | 54 |
| 9824 | Verdi | 45 |

| **Specialisti** | | |
|---|---|---|
| Matricola | Nome | Età |
| 9297 | Bianchi | 33 |
| 7432 | Gialli | 54 |
| 9824 | Verdi | 45 |

### Unione — $R_1 \cup R_2$
Tuple che compaiono in **almeno una** delle due relazioni.

| Matricola | Nome | Età |
|---|---|---|
| 7274 | Rossi | 42 |
| 7432 | Gialli | 54 |
| 9824 | Verdi | 45 |
| 9297 | Bianchi | 33 |

Essendo un insieme, le tuple comuni (7432/Gialli, 9824/Verdi) compaiono **una sola volta**.

### Differenza — $R_1 - R_2$
Tuple di $R_1$ che **non** compaiono in $R_2$.

`Laureati − Specialisti`:

| Matricola | Nome | Età |
|---|---|---|
| 7274 | Rossi | 42 |

### Intersezione — $R_1 \cap R_2$
Tuple presenti **in entrambe** le relazioni.

| Matricola | Nome | Età |
|---|---|---|
| 7432 | Gialli | 54 |
| 9824 | Verdi | 45 |

> [!note] L'intersezione è derivabile: $R_1 \cap R_2 = R_1 - (R_1 - R_2)$.

### Un'unione sensata ma impossibile

| Padre | Figlio |
|---|---|
| Adamo | Abele |
| Adamo | Caino |
| Abramo | Isacco |

| Madre | Figlio |
|---|---|
| Eva | Abele |
| Eva | Set |
| Sara | Isacco |

Concettualmente ha senso unire "chi è genitore di chi", ma `Paternità ∪ Maternità` **non è definita**: gli schemi sono diversi (`Padre` ≠ `Madre`). Serve la ridenominazione.

---

## 3. Ridenominazione — $\rho$ / `REN`

- Cambia il nome di **attributi** (e/o della relazione).
- **Modifica lo schema** lasciando **inalterata l'istanza**.

$$\text{REN}_{\text{Genitore} \leftarrow \text{Padre}}(\text{Paternità}) \quad\equiv\quad \rho_{\text{Genitore} \leftarrow \text{Padre}}(\text{Paternità})$$

Ora entrambe le relazioni hanno schema `(Genitore, Figlio)` e l'unione è possibile:

$$\rho_{\text{Genitore} \leftarrow \text{Padre}}(\text{Paternità}) \;\cup\; \rho_{\text{Genitore} \leftarrow \text{Madre}}(\text{Maternità})$$

| Genitore | Figlio |
|---|---|
| Adamo | Abele |
| Adamo | Caino |
| Abramo | Isacco |
| Eva | Abele |
| Eva | Set |
| Sara | Isacco |

### Ridenominazione di più attributi
Un'unica `REN` può rinominare più attributi contemporaneamente (le liste si corrispondono per posizione).

`Impiegati(Cognome, Ufficio, Stipendio)` e `Operai(Cognome, Fabbrica, Salario)`:

$$\rho_{\text{Sede},\text{Retrib} \leftarrow \text{Ufficio},\text{Stipendio}}(\text{Impiegati}) \;\cup\; \rho_{\text{Sede},\text{Retrib} \leftarrow \text{Fabbrica},\text{Salario}}(\text{Operai})$$

| Cognome | Sede | Retrib |
|---|---|---|
| Rossi | Roma | 55 |
| Neri | Milano | 64 |
| Bruni | Monza | 45 |
| Verdi | Latina | 55 |

### Sintassi alternativa
Per rinominare **sia la relazione sia tutti gli attributi** di un'espressione $E$ con $n$ attributi:

$$\rho_{R(A_1, A_2, \dots, A_n)}(E)$$

Es. da `ESAME(Studente, Corso, Voto)`: $\rho_{\text{APPELLO}(\text{Matricola},\,\text{CodC},\,\text{Esito})}(\text{ESAME})$

---

## 4. Selezione — $\sigma$ / `SEL`

Un **filtro orizzontale** sulle tuple, in base a condizioni sui valori.

- **Sintassi**: $\sigma_{\text{Condizione}}(E)$ — la condizione è un'espressione booleana (come nei vincoli di ennupla), con atomi $A \;\theta\; B$ o $A \;\theta\; c$ combinati con $\wedge, \vee, \neg$.
- **Semantica**: il risultato ha **lo stesso schema** dell'operando e contiene il **sottoinsieme** delle tuple che soddisfano la condizione.

**Impiegati**

| Matricola | Cognome | Filiale | Stipendio |
|---|---|---|---|
| 7309 | Rossi | Roma | 55 |
| 5998 | Neri | Milano | 64 |
| 9553 | Milano | Milano | 44 |
| 5698 | Neri | Napoli | 64 |

**1. Impiegati che guadagnano più di 50** — $\sigma_{\text{Stipendio} > 50}(\text{Impiegati})$

| Matricola | Cognome | Filiale | Stipendio |
|---|---|---|---|
| 7309 | Rossi | Roma | 55 |
| 5998 | Neri | Milano | 64 |
| 5698 | Neri | Napoli | 64 |

**2. …e lavorano a Milano** — $\sigma_{\text{Stipendio} > 50 \,\wedge\, \text{Filiale} = \text{'Milano'}}(\text{Impiegati})$

| Matricola | Cognome | Filiale | Stipendio |
|---|---|---|---|
| 5998 | Neri | Milano | 64 |

**3. Cognome uguale al nome della filiale** — $\sigma_{\text{Cognome} = \text{Filiale}}(\text{Impiegati})$

| Matricola | Cognome | Filiale | Stipendio |
|---|---|---|---|
| 9553 | Milano | Milano | 44 |

### Selezione e valori nulli

$\sigma_{\text{Età} > 40}(\text{Impiegati})$:

| Matricola | Cognome | Filiale | Età |
|---|---|---|---|
| 7309 | Rossi | Roma | 32 |
| 5998 | Neri | Milano | 45 |
| 9553 | Bruni | Milano | NULL |

→ risultato: solo **5998 Neri**. La tupla con `Età = NULL` sparisce: **una condizione atomica è vera solo per valori non nulli**.

> [!warning] Un risultato non desiderabile
> $$\sigma_{\text{Età}>30}(P) \cup \sigma_{\text{Età}\le 30}(P) \neq P \qquad \sigma_{\text{Età}>30 \,\vee\, \text{Età}\le 30}(P) \neq P$$
> Una tupla con `Età = NULL` non soddisfa nessuna delle due condizioni: ogni confronto con `NULL` resta *sconosciuto*, non diventa vero.

**Soluzione**: le condizioni apposite `IS NULL` e `IS NOT NULL`.

$$\sigma_{\text{Età}>30}(P) \cup \sigma_{\text{Età}\le 30}(P) \cup \sigma_{\text{Età IS NULL}}(P) = \sigma_{\text{Età}>30 \,\vee\, \text{Età}\le 30 \,\vee\, \text{Età IS NULL}}(P) = P$$

Esempio: $\sigma_{(\text{Età}>40) \,\vee\, (\text{Età IS NULL})}(\text{Impiegati})$ restituisce **Neri (45)** e **Bruni (NULL)**.

---

## 5. Proiezione — $\pi$ / `PROJ`

Un **filtro verticale**: sceglie un sottoinsieme degli **attributi**.

- **Sintassi**: $\pi_{\text{ListaAttributi}}(E)$
- **Semantica**: il risultato contiene le ennuple ottenute da **tutte** le ennuple dell'operando, **ristrette** agli attributi della lista.

> [!tip] Selezione e proiezione sono **ortogonali**
> | Selezione | Proiezione |
> |---|---|
> | Filtraggio **orizzontale** | Filtraggio **verticale** |
> | Sceglie un sottoinsieme delle **righe** | Sceglie un sottoinsieme delle **colonne** |

**Impiegati**

| Matricola | Cognome | Filiale | Stipendio |
|---|---|---|---|
| 7309 | Neri | Napoli | 55 |
| 5998 | Neri | Milano | 64 |
| 9553 | Rossi | Roma | 44 |
| 5698 | Rossi | Roma | 64 |

$\pi_{\text{Matricola},\text{Cognome}}(\text{Impiegati})$ → 4 tuple (Matricola è chiave).

$\pi_{\text{Cognome},\text{Filiale}}(\text{Impiegati})$:

| Cognome | Filiale |
|---|---|
| Neri | Napoli |
| Neri | Milano |
| Rossi | Roma |

La proiezione "grezza" avrebbe 4 righe, ma `(Rossi, Roma)` compare due volte e, essendo il risultato un insieme, **i duplicati collassano**.

### Cardinalità
- Una proiezione contiene **al più** tante ennuple quante l'operando, ma **può contenerne meno**.
- Se $K$ è una **superchiave** di $R$, allora $\pi_K(R)$ contiene **esattamente** $|R|$ tuple. ^quiz-proj

### Combinare selezione e proiezione

*Matricola e cognome di chi guadagna più di 50:*

$$\pi_{\text{Matricola},\text{Cognome}}\big(\sigma_{\text{Stipendio}>50}(\text{Impiegati})\big)$$

> [!warning] Attenzione all'ordine
> $\sigma_{\text{Stipendio}>50}\big(\pi_{\text{Matricola},\text{Cognome}}(\text{Impiegati})\big)$ è **errata**: dopo la proiezione l'attributo `Stipendio` non esiste più.

### Esercizi su `Persona(CF, Età, CittàNatale, CittàAttuale)`

| # | Domanda | Soluzione |
|---|---|---|
| 1 | In quali città abitano dei trentenni? | $\pi_{\text{CittàAttuale}}(\sigma_{\text{Età}=30}(\text{Persona}))$ — *non* $\sigma_{\text{Età}=30}(\pi_{\text{CittàAttuale}}(\ldots))$ |
| 2 | In quali città è nato qualcuno ma non abita nessuno? | $\rho_{\text{Città}\leftarrow\text{CittàNatale}}(\pi_{\text{CittàNatale}}(\text{Persona})) - \rho_{\text{Città}\leftarrow\text{CittàAttuale}}(\pi_{\text{CittàAttuale}}(\text{Persona}))$ |
| 3 | Città in cui è nato qualcuno **e** abita qualcuno | $\rho_{\text{Città}\leftarrow\text{CittàNatale}}(\pi_{\text{CittàNatale}}(\text{Persona})) \cap \rho_{\text{Città}\leftarrow\text{CittàAttuale}}(\pi_{\text{CittàAttuale}}(\text{Persona}))$ |
| 4 | In quali città risiede qualche nativo (nato e residente lì)? | $\pi_{\text{CittàNatale}}(\sigma_{\text{CittàNatale}=\text{CittàAttuale}}(\text{Persona}))$ |
| 5 | Tutte le città presenti nella base di dati | $\rho_{\text{Città}\leftarrow\text{CittàNatale}}(\pi_{\text{CittàNatale}}(\text{Persona})) \cup \rho_{\text{Città}\leftarrow\text{CittàAttuale}}(\pi_{\text{CittàAttuale}}(\text{Persona}))$ |
| 6 | In quali città abitano **solo** anziani (età ≥ 70)? | $\pi_{\text{CittàAttuale}}(\text{Persona}) - \pi_{\text{CittàAttuale}}(\sigma_{\text{Età}<70}(\text{Persona}))$ |

^es-persona

### Limiti di selezione e proiezione
Permettono di estrarre informazioni da **una** relazione, ma **non** di:
- correlare dati che vivono in **relazioni diverse**;
- confrontare **ennuple diverse** della stessa relazione.

---

## 6. Join

Il **join** è l'operatore più interessante dell'algebra relazionale: permette di **correlare dati in relazioni diverse**.

> [!example] Analogia del concorso pubblico
> I compiti sono anonimi; a ciascuno è associata una busta chiusa con il nome del candidato, contrassegnata con lo **stesso numero** del compito. Accostando compiti e buste con lo stesso numero si ottiene *Candidato → Voto*.
>
> | Numero | Voto |
> |---|---|
> | 1 | 25 |
> | 2 | 13 |
> | 3 | 27 |
> | 4 | 28 |
>
> | Numero | Candidato |
> |---|---|
> | 1 | Mario Rossi |
> | 2 | Nicola Russo |
> | 3 | Mario Bianchi |
> | 4 | Remo Neri |
>
> $\text{Voti} \bowtie \text{Candidati}$:
>
> | Numero | Candidato | Voto |
> |---|---|---|
> | 1 | Mario Rossi | 25 |
> | 2 | Nicola Russo | 13 |
> | 3 | Mario Bianchi | 27 |
> | 4 | Remo Neri | 28 |

### 6.1 Join naturale — $\bowtie$ / `NAT_JOIN`

- Operatore **binario** (generalizzabile a più relazioni).
- Produce un risultato sull'**unione degli attributi** degli operandi.
- Ogni ennupla del risultato è costruita a partire da **una tupla di ciascun operando**.

> [!definition] Semantica
> Combina le tuple che hanno **valori uguali in tutti gli attributi con lo stesso nome**, e gli attributi comuni compaiono **una sola volta**.
> Formalmente, con $R_1(X_1)$ e $R_2(X_2)$:
> $$R_1 \bowtie R_2 = \{\, t \text{ su } X_1 \cup X_2 \mid t[X_1] \in R_1 \,\wedge\, t[X_2] \in R_2 \,\}$$

#### Join completo
Ogni tupla contribuisce al risultato.

| Impiegato | Reparto |
|---|---|
| Rossi | A |
| Neri | B |
| Bianchi | B |

| Reparto | Capo |
|---|---|
| A | Mori |
| B | Bruni |

| Impiegato | Reparto | Capo |
|---|---|---|
| Rossi | A | Mori |
| Neri | B | Bruni |
| Bianchi | B | Bruni |

#### Join non completo
`Reparti = {(B, Mori), (C, Bruni)}`: il reparto A di Rossi non ha corrispondenza e il reparto C non ha impiegati, quindi queste tuple (**dangling**) non contribuiscono.

| Impiegato | Reparto | Capo |
|---|---|---|
| Neri | B | Mori |
| Bianchi | B | Mori |

#### Join vuoto
`Reparti = {(D, Mori), (C, Bruni)}`: nessun reparto di Impiegati (A, B) compare in Reparti → **zero tuple**.

#### Join con $n \times m$ ennuple
`Impiegati = {(Rossi, B), (Neri, B)}`, `Reparti = {(B, Mori), (B, Bruni)}`: B compare due volte su entrambi i lati (non è chiave) → $2 \times 2 = 4$ tuple.

| Impiegato | Reparto | Capo |
|---|---|---|
| Rossi | B | Mori |
| Rossi | B | Bruni |
| Neri | B | Mori |
| Neri | B | Bruni |

#### Cardinalità del join
Date $R_1(A, B)$ e $R_2(B, C)$:

| Condizione | Cardinalità |
|---|---|
| In generale | $0 \le \lvert R_1 \bowtie R_2 \rvert \le \lvert R_1 \rvert \cdot \lvert R_2 \rvert$ |
| Se $B$ è chiave in $R_2$ | $0 \le \lvert R_1 \bowtie R_2 \rvert \le \lvert R_1 \rvert$ |
| Se $B$ è chiave in $R_2$ **e** c'è un vincolo referenziale da $R_1.B$ a $R_2$ | $\lvert R_1 \bowtie R_2 \rvert = \lvert R_1 \rvert$ |

### 6.2 Join esterno (outer join)

Estende **con valori nulli** le ennuple che verrebbero tagliate fuori da un join (interno). Tre versioni, sullo stesso esempio del join non completo:

| Versione | Simbolo | Mantiene |
|---|---|---|
| **Sinistro** (`JOIN_LEFT`) | ⟕ | tutte le tuple del **primo** operando |
| **Destro** (`JOIN_RIGHT`) | ⟖ | tutte le tuple del **secondo** operando |
| **Completo** (`JOIN_FULL`) | ⟗ | tutte le tuple di **entrambi** |

**Sinistro**

| Impiegato | Reparto | Capo |
|---|---|---|
| Neri | B | Mori |
| Bianchi | B | Mori |
| Rossi | A | NULL |

**Destro**

| Impiegato | Reparto | Capo |
|---|---|---|
| Neri | B | Mori |
| Bianchi | B | Mori |
| NULL | C | Bruni |

**Completo** (unione di sinistro e destro)

| Impiegato | Reparto | Capo |
|---|---|---|
| Neri | B | Mori |
| Bianchi | B | Mori |
| Rossi | A | NULL |
| NULL | C | Bruni |

### 6.3 Prodotto cartesiano — $\times$

Combina **ogni** ennupla del primo operando con **ogni** ennupla del secondo (join su relazioni senza attributi in comune):

$$\lvert R_1 \times R_2 \rvert = \lvert R_1 \rvert \cdot \lvert R_2 \rvert$$

`Impiegati × Reparti` (3 × 2 = 6 righe):

| Impiegato | Reparto | Reparto | Capo |
|---|---|---|---|
| Rossi | A | A | Mori |
| Rossi | A | B | Bruni |
| Neri | B | A | Mori |
| Neri | B | B | Bruni |
| Bianchi | B | A | Mori |
| Bianchi | B | B | Bruni |

Compaiono tutte le combinazioni, anche quelle senza senso; inoltre `Reparto` compare **due volte** (il prodotto non unifica gli attributi come il join naturale). In pratica gli attributi omonimi vanno prima ridenominati.

> In notazione a parole, `R1 JOIN R2` **senza condizione** indica il prodotto cartesiano.

### 6.4 Theta-join ed equi-join

Il prodotto cartesiano ha senso (quasi) solo se seguito da una **selezione**:

$$R_1 \bowtie_{\text{Condizione}} R_2 \;=\; \sigma_{\text{Condizione}}(R_1 \times R_2)$$

- **Theta-join**: la condizione è (spesso) una congiunzione di atomi $A_1 \,\theta\, A_2$, con $\theta \in \{=, \neq, <, >, \le, \ge\}$. È il join che si userà più spesso.
- **Equi-join**: theta-join in cui $\theta$ è sempre l'**uguaglianza**. Nella pratica è il caso più frequente.

**Esempio** — `Impiegati(Impiegato, Reparto)`, `Reparti(Codice, Capo)`:

$$\text{Impiegati} \bowtie_{\text{Reparto}=\text{Codice}} \text{Reparti} = \sigma_{\text{Reparto}=\text{Codice}}(\text{Impiegati} \times \text{Reparti})$$

| Impiegato | Reparto | Codice | Capo |
|---|---|---|---|
| Rossi | A | A | Mori |
| Neri | B | B | Bruni |
| Bianchi | B | B | Bruni |

Stesso effetto del join naturale, ma `Reparto` e `Codice` restano **due colonne distinte**.

---

## 7. Query guidate

Schema di riferimento (`Matricola`, `Impiegato` e `Capo` sono codici; stipendio in migliaia di euro):

**Impiegati**

| Matricola | Nome | Età | Stipendio |
|---|---|---|---|
| 7309 | Rossi | 34 | 45 |
| 5998 | Bianchi | 37 | 38 |
| 9553 | Neri | 42 | 35 |
| 5698 | Bruni | 43 | 42 |
| 4076 | Mori | 45 | 50 |
| 8123 | Lupi | 46 | 60 |

**Supervisione**

| Impiegato | Capo |
|---|---|
| 7309 | 5698 |
| 5998 | 5698 |
| 9553 | 4076 |
| 5698 | 4076 |
| 4076 | 8123 |

> [!tip] Come ragionare
> Prima domanda: **in quante relazioni** si trovano i dati che servono? Si usa il **numero minimo** di relazioni possibile: la query è più leggibile e più leggera per il DBMS.

**Query 1** — *Matricola, nome, età e stipendio di chi guadagna più di 40* ^q1

$$\sigma_{\text{Stipendio}>40}(\text{Impiegati})$$

**Query 2** — *Solo matricola, nome ed età* ^q2

$$\pi_{\text{Matricola},\text{Nome},\text{Età}}\big(\sigma_{\text{Stipendio}>40}(\text{Impiegati})\big)$$
Lo stipendio serve per filtrare ma non compare nel risultato.

**Query 3** — *I capi degli impiegati che guadagnano più di 40* ^q3

$$\pi_{\text{Capo}}\big(\text{Supervisione} \bowtie_{\text{Impiegato}=\text{Matricola}} \sigma_{\text{Stipendio}>40}(\text{Impiegati})\big)$$
Un capo può comparire più volte nel join, ma la proiezione elimina i duplicati.

> [!warning] L'ordine degli operatori conta
> - ✅ $\pi_{\text{Capo}}\big(\sigma_{\text{Stipendio}>40}(\text{Supervisione} \bowtie_{\text{Impiegato}=\text{Matricola}} \text{Impiegati})\big)$
> - ❌ $\sigma_{\text{Stipendio}>40}\big(\pi_{\text{Capo}}(\text{Supervisione} \bowtie_{\text{Impiegato}=\text{Matricola}} \text{Impiegati})\big)$ — dopo $\pi_{\text{Capo}}$ l'attributo `Stipendio` non esiste più: **errore sintattico**.

**Query 4** — *Nome e stipendio dei capi degli impiegati che guadagnano più di 40* ^q4

$$\pi_{\text{Nome},\text{Stipendio}}\Big(\text{Impiegati} \bowtie_{\text{Matricola}=\text{Capo}} \pi_{\text{Capo}}\big(\text{Supervisione} \bowtie_{\text{Impiegato}=\text{Matricola}} \sigma_{\text{Stipendio}>40}(\text{Impiegati})\big)\Big)$$

**Expression tree** equivalente:

```
              π Nome,Stipendio
                     │
              ⋈ Matricola=Capo
               ┌─────┴─────┐
          Impiegati      π Capo
                            │
                   ⋈ Impiegato=Matricola
                     ┌──────┴──────┐
               Supervisione   σ Stipendio>40
                                   │
                               Impiegati
```

**Query 5 (difficile)** — *Impiegati che guadagnano più del proprio capo* (con matricola, nome e stipendio di entrambi) ^q5

$$\pi_{\text{Matr},\text{Nome},\text{Stip},\text{MatrC},\text{NomeC},\text{StipC}}\Big(\sigma_{\text{Stipendio}>\text{StipC}}\big(\rho_{\text{MatrC},\text{NomeC},\text{StipC},\text{EtàC} \leftarrow \text{Matricola},\text{Nome},\text{Stipendio},\text{Età}}(\text{Impiegati}) \bowtie_{\text{MatrC}=\text{Capo}} (\text{Supervisione} \bowtie_{\text{Impiegato}=\text{Matricola}} \text{Impiegati})\big)\Big)$$

```
        π Matricola,Nome,Stipendio,MatrC,NomeC,StipC
                        │
                σ Stipendio > StipC
                        │
                  ⋈ MatrC=Capo
            ┌───────────┴───────────┐
  ρ MatrC,NomeC,StipC,EtàC     ⋈ Impiegato=Matricola
         │                     ┌────────┴────────┐
     Impiegati            Impiegati       Supervisione
```

La stessa relazione `Impiegati` compare **due volte** (una per il ruolo di impiegato, una per il ruolo di capo): la ridenominazione serve a distinguere gli attributi.

> [!important] All'esame
> Si scrivono **sia l'albero sia la formula algebrica**. Nell'albero si possono usare sia le lettere greche sia le etichette (`PROJ`, `SEL`…).

**Query 6** — *Matricole dei capi i cui impiegati guadagnano **tutti** più di 40* ^q6

$$\pi_{\text{Capo}}(\text{Supervisione}) \;-\; \pi_{\text{Capo}}\big(\text{Supervisione} \bowtie_{\text{Impiegato}=\text{Matricola}} \sigma_{\text{Stipendio}\le 40}(\text{Impiegati})\big)$$

> [!tip] Tecnica: "per ogni" con la doppia negazione
> L'algebra relazionale non ha un operatore *per ogni*: si simula prendendo **tutti** i candidati e togliendo quelli che hanno **almeno un'eccezione**.

---

## 8. Esercizi con RelaX

Tool interattivo: <https://dbis-uibk.github.io/relax> → dal menu in alto a sinistra scegliere lo schema **"Progetti - by Torlone"**, con le relazioni `Impiegato`, `Dipartimento`, `Progetto`, `Partecipazione`.

**Importare nuovi schemi**: *Group Editor* → incollare il contenuto del file `.txt` (es. `supervisione.txt`, `stoccaggi.txt`) → *Preview* → *Use Group in Editor*. Lo schema resta caricato solo nella sessione del browser.

**Es. 1** — *Per ogni impiegato, il cognome e il nome del dipartimento in cui lavora* ^relax1

$$\pi_{\text{cognome},\text{nome}}(\text{Impiegato} \bowtie_{\text{dipartimento}=\text{codice}} \text{Dipartimento})$$

```
      π cognome,nome
            │
   ⋈ dipartimento=codice
      ┌─────┴─────┐
  Impiegato   Dipartimento
```

**Es. 2** — *Per ogni progetto con bilancio > 100.000 €: nome e bilancio del progetto e cognome degli impiegati che vi partecipano, uno per riga* ^relax2

$$\pi_{\text{nome},\text{bilancio},\text{cognome}}\big(\sigma_{\text{bilancio}>100000}(\text{Progetto}) \bowtie_{\text{sigla}=\text{progetto}} \text{Partecipazione} \bowtie_{\text{impiegato}=\text{matricola}} \text{Impiegato}\big)$$

- Solo progetti **con** partecipanti → join interno: $\text{Progetto} \bowtie_{\text{sigla}=\text{progetto}} \text{Partecipazione}$
- Anche progetti **senza** partecipanti → join sinistro: `Progetto ⟕ sigla=progetto Partecipazione`

**Es. 3** — *Cognome e matricola degli impiegati che guadagnano più del direttore del loro dipartimento* ^relax3

$$\pi_{\text{cognome},\text{matricola}}\Big(\sigma_{\text{stipendio}>\text{DStipendio}}\big(\text{Impiegato} \bowtie_{\text{dipartimento}=\text{codice}} \text{Dipartimento} \bowtie_{\text{direttore}=\text{DMatricola}} \rho_{\text{DMatricola}\leftarrow\text{matricola},\,\text{DStipendio}\leftarrow\text{stipendio}}(\pi_{\text{matricola},\text{stipendio}}(\text{Impiegato}))\big)\Big)$$

**Es. 4** — *Matricola degli impiegati che partecipano **solo** a progetti con budget > 100.000 €* ^relax4

$$\pi_{\text{impiegato}}(\text{Partecipazione}) - \pi_{\text{impiegato}}\big(\text{Partecipazione} \bowtie_{\text{progetto}=\text{sigla}} \sigma_{\text{bilancio}\le 100000}(\text{Progetto})\big)$$

**Es. 5 (attenzione ai dettagli!)** — *Matricola degli impiegati che **non partecipano a nessun** progetto con budget < 100.000 €* ^relax5

$$\pi_{\text{matricola}}(\text{Impiegato}) - \pi_{\text{impiegato}}\big(\text{Partecipazione} \bowtie_{\text{progetto}=\text{sigla}} \sigma_{\text{bilancio}<100000}(\text{Progetto})\big)$$

> [!note] Differenze tra Es. 4 ed Es. 5
> - Nell'Es. 5 si parte da **`Impiegato`** (non da `Partecipazione`): così sono inclusi anche gli impiegati che non partecipano ad alcun progetto.
> - Il confronto è `<` invece di `≤`.
> - Formalmente la differenza richiede schemi uguali (`matricola` vs `impiegato`): a rigore serve una ridenominazione (RelaX la tollera se gli schemi sono compatibili per posizione).

**Es. 6** — *Cognome, matricola e stipendio degli stessi impiegati dell'Es. 5* ^relax6

$$\pi_{\text{matricola},\text{cognome},\text{stipendio}}\Big(\text{Impiegato} \bowtie_{\text{matricola}=\text{matr}} \rho_{\text{matr}\leftarrow\text{matricola}}\big(\pi_{\text{matricola}}(\text{Impiegato}) - \pi_{\text{impiegato}}(\text{Partecipazione} \bowtie_{\text{progetto}=\text{sigla}} \sigma_{\text{bilancio}<100000}(\text{Progetto}))\big)\Big)$$

Dopo una differenza si **torna in join** con la relazione di partenza per recuperare gli attributi mancanti.

---

## 9. Viste (relazioni derivate)

Rappresentazioni diverse per gli stessi dati: costituiscono lo **schema esterno** del DBMS (architettura ANSI/SPARC: schemi esterni → schema logico → schema interno → base di dati).

| Relazioni **di base** | Relazioni **derivate** (viste) |
|---|---|
| Contenuto **autonomo**, memorizzato direttamente nel DB | Contenuto **funzione** di altre relazioni, definito tramite un'interrogazione |

Le viste possono essere definite anche su altre viste.

**Esempio** — `Afferenza(Impiegato, Reparto)`, `Direzione(Reparto, Capo)`:

$$\text{Supervisione} = \pi_{\text{Impiegato},\text{Capo}}(\text{Afferenza} \bowtie \text{Direzione})$$

(La relazione `Supervisione` usata negli esempi precedenti è proprio una vista di questo tipo.)

**Interrogazioni sulle viste**: si eseguono **sostituendo alla vista la sua definizione**.

$$\sigma_{\text{Capo}=\text{'Leoni'}}(\text{Supervisione}) \;\;\longrightarrow\;\; \sigma_{\text{Capo}=\text{'Leoni'}}\big(\pi_{\text{Impiegato},\text{Capo}}(\text{Afferenza} \bowtie \text{Direzione})\big)$$

**Motivazioni**
- **Schema esterno**: ogni utente vede solo ciò che gli interessa e che è autorizzato a vedere.
- **Strumento di programmazione**: semplifica la scrittura delle interrogazioni.
- **Riuso**: permette di usare programmi esistenti anche su schemi ristrutturati.

---

## 10. Limiti dell'algebra relazionale

Esistono interrogazioni interessanti **non esprimibili**:

1. **Calcolo di valori derivati** — si possono solo estrarre valori esistenti, mai calcolarne di nuovi:
   - a livello di tupla o singolo valore (conversioni, somme…);
   - su insiemi di tuple (somme, medie…).
2. **Ricorsione** — interrogazioni inerentemente ricorsive, come la **chiusura transitiva**.

**Esempio — chiusura transitiva**: per ogni impiegato trovare *tutti* i superiori (capo, capo del capo, …). ^es-chiusura

| Impiegato | Capo |
|---|---|
| Rossi | Mori |
| Neri | Mori |
| Mori | Lupi |

Chiusura desiderata:

| Impiegato | Superiore |
|---|---|
| Rossi | Mori |
| Rossi | Lupi |
| Neri | Mori |
| Neri | Lupi |
| Mori | Lupi |

Con un numero fissato di join si raggiunge solo una profondità fissata: **non esiste** un'espressione algebrica che calcoli la chiusura per gerarchie di profondità arbitraria.

> [!info] Ripasso online (Stanford Lagunita)
> - Relational Algebra 1 — <https://youtube.com/watch?v=tii7xcFilOA>
> - Relational Algebra 2 — <https://youtube.com/watch?v=GkBf2dZAES0>

---

## 11. Esercizi — schema «Stoccaggi»

- MAGAZZINO(<u>CodiceM</u>, Città, Via, NumeroCivico)
- AZIENDA(<u>CodiceA</u>, NomeAzienda, Città, CAP, Telefono)
- PRODOTTO(<u>CodiceP</u>, NomeProdotto, Azienda, Prezzo) — FK: `Azienda → AZIENDA.CodiceA`
- STOCCAGGIO(<u>Prodotto, Magazzino</u>, Quantità) — FK: `Prodotto → PRODOTTO.CodiceP`, `Magazzino → MAGAZZINO.CodiceM`

### Esercizi base

| Richiesta | Soluzione |
|---|---|
| Nome dei prodotti che costano meno di 10 € | $\pi_{\text{NomeProdotto}}(\sigma_{\text{Prezzo}<10}(\text{PRODOTTO}))$ |
| Numeri civici dei magazzini di via Ripamonti a Milano | $\pi_{\text{NumeroCivico}}(\sigma_{\text{Città}=\text{'Milano'} \wedge \text{Via}=\text{'Ripamonti'}}(\text{MAGAZZINO}))$ |
| Codice dei prodotti stoccati in un magazzino con lo stesso codice del prodotto | $\pi_{\text{Prodotto}}(\sigma_{\text{Prodotto}=\text{Magazzino}}(\text{STOCCAGGIO}))$ |
| Codice, nome e prezzo dei prodotti che costano più di 100 | $\pi_{\text{CodiceP},\text{NomeProdotto},\text{Prezzo}}(\sigma_{\text{Prezzo}>100}(\text{PRODOTTO}))$ |

^st-base

### Esercizi con join

**Nome dei prodotti delle aziende di Milano** ^st-milano

$$\pi_{\text{NomeProdotto}}\big(\text{PRODOTTO} \bowtie_{\text{Azienda}=\text{CodiceA}} \sigma_{\text{Città}=\text{'Milano'}}(\text{AZIENDA})\big)$$

**Nome e prezzo dei prodotti dell'azienda «Apple»** ^st-apple

$$\pi_{\text{NomeProdotto},\text{Prezzo}}\big(\text{PRODOTTO} \bowtie_{\text{Azienda}=\text{CodiceA}} \sigma_{\text{NomeAzienda}=\text{'Apple'}}(\text{AZIENDA})\big)$$

**Codice dei magazzini in cui è stoccato un prodotto di nome «iPhone»** ^st-iphone-mag

$$\pi_{\text{Magazzino}}\big(\text{STOCCAGGIO} \bowtie_{\text{Prodotto}=\text{CodiceP}} \sigma_{\text{NomeProdotto}=\text{'iPhone'}}(\text{PRODOTTO})\big)$$

**Città dei magazzini in cui è stoccato un «iPhone»** ^st-iphone-citta

$$\pi_{\text{Città}}\Big(\text{MAGAZZINO} \bowtie_{\text{CodiceM}=\text{Magazzino}} \big(\text{STOCCAGGIO} \bowtie_{\text{Prodotto}=\text{CodiceP}} \sigma_{\text{NomeProdotto}=\text{'iPhone'}}(\text{PRODOTTO})\big)\Big)$$

### Esercizi più complessi

**Codice dei prodotti stoccati nella città dell'azienda che li produce** ^st-citta-azienda

La città del magazzino va confrontata con la città dell'azienda: ci sono due attributi `Città` diversi → serve una **ridenominazione** prima del confronto.

$$\pi_{\text{CodiceP}}\Big(\sigma_{\text{Città}=\text{CittàM}}\big(\rho_{\text{CittàM}\leftarrow\text{Città}}(\text{MAGAZZINO}) \bowtie_{\text{CodiceM}=\text{Magazzino}} (\text{STOCCAGGIO} \bowtie_{\text{Prodotto}=\text{CodiceP}} (\text{PRODOTTO} \bowtie_{\text{Azienda}=\text{CodiceA}} \text{AZIENDA}))\big)\Big)$$

```
                       π CodiceP
                           │
                    σ Città = CittàM
                           │
                   ⋈ Magazzino=CodiceM
            ┌──────────────┴──────────────┐
   ρ CittàM ← Città               ⋈ Prodotto=CodiceP
          │                     ┌─────────┴─────────┐
      MAGAZZINO            STOCCAGGIO       ⋈ Azienda=CodiceA
                                            ┌───────┴───────┐
                                        PRODOTTO         AZIENDA
```

**Prodotti di aziende milanesi stoccati in magazzini di Roma** (nome prodotto, codice magazzino, quantità) ^st-mi-roma

$$\pi_{\text{NomeProdotto},\text{Magazzino},\text{Quantità}}\Big(\sigma_{\text{Città}=\text{'Roma'}}(\text{MAGAZZINO}) \bowtie_{\text{CodiceM}=\text{Magazzino}} \text{STOCCAGGIO} \bowtie_{\text{Prodotto}=\text{CodiceP}} \big(\text{PRODOTTO} \bowtie_{\text{Azienda}=\text{CodiceA}} \sigma_{\text{CittàA}=\text{'Milano'}}(\rho_{\text{CittàA}\leftarrow\text{Città}}(\text{AZIENDA}))\big)\Big)$$

```
             π NomeProdotto,Magazzino,Quantità
                           │
                   ⋈ Magazzino=CodiceM
            ┌──────────────┴──────────────┐
    σ Città='Roma'                ⋈ Prodotto=CodiceP
          │                     ┌─────────┴─────────┐
      MAGAZZINO            STOCCAGGIO       ⋈ Azienda=CodiceA
                                            ┌───────┴───────┐
                                        PRODOTTO     σ CittàA='Milano'
                                                            │
                                                    ρ CittàA ← Città
                                                            │
                                                         AZIENDA
```

### Esercizi con differenza e unione

**Codice dei magazzini in cui non è stoccato nessun prodotto** ^st-mag-vuoti

$$\rho_{\text{Magazzino}\leftarrow\text{CodiceM}}(\pi_{\text{CodiceM}}(\text{MAGAZZINO})) - \pi_{\text{Magazzino}}(\sigma_{\text{Quantità}>0}(\text{STOCCAGGIO}))$$

**Città sede di un'azienda o di un magazzino (o di entrambi)** ^st-citta-union

$$\pi_{\text{Città}}(\text{MAGAZZINO}) \cup \pi_{\text{Città}}(\text{AZIENDA})$$

### Esercizi con self-join (confronto fra tuple della stessa relazione)

Si usano **viste ausiliarie** (utili ma non strettamente necessarie) che ridenominano la stessa relazione:

- $\text{PROD1} = \rho_{C1,N1,A1,P1 \leftarrow \text{CodiceP},\text{NomeProdotto},\text{Azienda},\text{Prezzo}}(\text{PRODOTTO})$
- $\text{PROD2} = \rho_{C2,N2,A2,P2 \leftarrow \text{CodiceP},\text{NomeProdotto},\text{Azienda},\text{Prezzo}}(\text{PRODOTTO})$

**Coppie di prodotti diversi con lo stesso nome** ^st-coppie

$$\pi_{C1,C2}\big(\sigma_{(N1=N2) \wedge (C1 \neq C2)}(\text{PROD1} \times \text{PROD2})\big)$$

> [!tip] Raffinamento: evitare le coppie simmetriche
> Con $C1 \neq C2$ escono sia `[33, 66]` sia `[66, 33]`. Usando $C1 > C2$ esce una sola coppia per confronto:
> $$\pi_{C1,C2}\big(\sigma_{(N1=N2) \wedge (C1 > C2)}(\text{PROD1} \times \text{PROD2})\big)$$

**Codice dei prodotti stoccati in almeno 2 magazzini diversi** ^st-2mag

Con $\text{STOC1} = \rho_{P1,M1,Q1}(\text{STOCCAGGIO})$ e $\text{STOC2} = \rho_{P2,M2,Q2}(\text{STOCCAGGIO})$:
$$\pi_{P1}\big(\sigma_{(P1=P2) \wedge (M1 \neq M2)}(\text{STOC1} \times \text{STOC2})\big)$$

**Prezzo del prodotto più caro** ^st-max

Con $\text{PR1} = \rho_{P1\leftarrow\text{Prezzo}}(\pi_{\text{Prezzo}}(\text{PRODOTTO}))$ e $\text{PR2} = \rho_{P2\leftarrow\text{Prezzo}}(\pi_{\text{Prezzo}}(\text{PRODOTTO}))$:
$$\pi_{\text{Prezzo}}(\text{PRODOTTO}) - \rho_{\text{Prezzo}\leftarrow P2}\big(\pi_{P2}(\sigma_{P2<P1}(\text{PR1} \times \text{PR2}))\big)$$

> [!tip] Tecnica: il massimo senza `MAX`
> Si tolgono da tutti i prezzi quelli che sono **minori di almeno un altro prezzo**: ciò che resta è il prezzo più alto.

**Nome delle aziende che producono almeno 2 prodotti diversi con lo stesso nome** ^st-aziende-omonimi

$$\pi_{\text{NomeAzienda}}\Big(\text{AZIENDA} \bowtie_{\text{CodiceA}=A1} \sigma_{(A1=A2) \wedge (N1=N2) \wedge (P1 < P2)}(\text{PROD1} \times \text{PROD2})\Big)$$

> [!note] La condizione $P1 < P2$ (dalle slide) individua prodotti distinti tramite il prezzo; per includere anche prodotti diversi con lo stesso prezzo si può usare $C1 \neq C2$.

---

## Riepilogo degli operatori

| Operatore | Notazione | Schema del risultato | Cardinalità |
|---|---|---|---|
| Unione | $R_1 \cup R_2$ | uguale (schemi identici) | $\le \lvert R_1\rvert + \lvert R_2\rvert$ |
| Intersezione | $R_1 \cap R_2$ | uguale | $\le \min(\lvert R_1\rvert, \lvert R_2\rvert)$ |
| Differenza | $R_1 - R_2$ | uguale | $\le \lvert R_1\rvert$ |
| Ridenominazione | $\rho_{B\leftarrow A}(R)$ | attributi rinominati | $= \lvert R\rvert$ |
| Selezione | $\sigma_F(R)$ | uguale a $R$ | $\le \lvert R\rvert$ |
| Proiezione | $\pi_X(R)$ | $X$ | $\le \lvert R\rvert$ ($=$ se $X$ superchiave) |
| Join naturale | $R_1 \bowtie R_2$ | $X_1 \cup X_2$ | $[0, \lvert R_1\rvert \cdot \lvert R_2\rvert]$ |
| Prodotto cartesiano | $R_1 \times R_2$ | $X_1 \cup X_2$ (disgiunti) | $= \lvert R_1\rvert \cdot \lvert R_2\rvert$ |
| Theta-join | $R_1 \bowtie_F R_2 = \sigma_F(R_1 \times R_2)$ | $X_1 \cup X_2$ | $[0, \lvert R_1\rvert \cdot \lvert R_2\rvert]$ |
| Join esterni | ⟕, ⟖, ⟗ | $X_1 \cup X_2$ (con `NULL`) | $\ge$ join interno |

---

## Tabella riassuntiva per gli esercizi

### Dalla richiesta all'operatore

| Nella richiesta compare… | Schema di soluzione | Esempio nella lezione |
|---|---|---|
| Una condizione sulle tuple ("che guadagnano più di…") | $\pi_{\text{attributi}}(\sigma_{\text{cond}}(R))$: prima $\sigma$, poi $\pi$ | [[#^q2\|Query 2]] |
| Dati sparsi su **più tabelle** | Join sulla coppia FK = PK, poi $\sigma$ e $\pi$ | [[#^st-milano\|Prodotti di Milano]] |
| La **stessa tabella in due ruoli**, o due attributi **omonimi** | $\rho$ prima del join per distinguere gli attributi | [[#^q5\|Query 5]], [[#^st-citta-azienda\|Città dell'azienda]] |
| "…**ma non**…", "**nessun**…" | **Differenza**: insieme completo $-$ quelli che soddisfano | [[#^relax5\|RelaX Es. 5]], [[#^st-mag-vuoti\|Magazzini vuoti]] |
| "**solo**…", "**tutti**…" | Tutti i candidati $-$ quelli con **almeno un'eccezione** (condizione negata) | [[#^q6\|Query 6]], [[#^relax4\|RelaX Es. 4]] |
| "…**o**… (o entrambi)", "tutti i valori presenti" | **Unione**, con $\rho$ per allineare gli schemi | [[#^st-citta-union\|Città di aziende o magazzini]] |
| "…**sia**… **sia**…" | **Intersezione** (con $\rho$ se serve) | [[#^es-persona\|Persona, es. 3]] |
| "**almeno 2** diversi" | Self-join: $\rho$ due copie, $\times$, $\sigma$ con uguaglianza sull'oggetto e $\neq$ sull'elemento che deve differire | [[#^st-2mag\|Prodotti in ≥ 2 magazzini]] |
| "**coppie**" senza ripetizioni simmetriche | Come sopra, ma $>$ al posto di $\neq$ | [[#^st-coppie\|Coppie di prodotti]] |
| "il **massimo**" / "il **minimo**" | Tutti i valori $-$ quelli **minori** (o maggiori) di almeno un altro valore | [[#^st-max\|Prezzo più caro]] |
| "anche quelli **senza corrispondenza**" | **Join esterno** (sinistro, destro o completo) | [[#^relax2\|RelaX Es. 2]] |
| Dopo una differenza servono **altri attributi** | Rifare il join con la relazione di partenza | [[#^relax6\|RelaX Es. 6]] |
| Somme, medie, conteggi; gerarchie ricorsive | **Non esprimibile** in algebra relazionale | [[#^es-chiusura\|Chiusura transitiva]] |

### Controlli prima di consegnare

| Controllo                                                                            | Perché                                                                                 |
| ------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------- |
| Ogni $\sigma$ usa solo attributi **ancora presenti** nel suo operando                | $\sigma$ dopo un $\pi$ che ha eliminato l'attributo è un **errore sintattico**         |
| $\cup$, $\cap$ e $-$ hanno operandi con **lo stesso schema**                         | Altrimenti l'operazione non è definita: usa $\rho$                                     |
| Nel join naturale gli attributi omonimi sono **solo** quelli su cui si vuole il join | Attributi omonimi non voluti (es. `Città`, `Nome`) restringono il risultato per errore |
| Nella differenza si parte dall'insieme **completo** giusto                           | `Impiegato` include anche chi non partecipa a nessun progetto, `Partecipazione` no     |
| Le condizioni sui `NULL` usano `IS NULL`                                             | Un confronto con `NULL` non è mai vero                                                 |
| La proiezione **elimina i duplicati**                                                | Il risultato è un insieme                                                              |
| Si scrivono sia la **formula** sia l'**albero**                                      | È richiesto all'esame                                                                  |

### Cardinalità

| Espressione | Numero di tuple |
|---|---|
| $\sigma_F(R)$ | $0 \dots \lvert R \rvert$ |
| $\pi_X(R)$ | $1 \dots \lvert R \rvert$ ($= \lvert R \rvert$ se $X$ è superchiave; $0$ se $R$ è vuota) |
| $R_1 \cup R_2$ | $\max(\lvert R_1\rvert, \lvert R_2\rvert) \dots \lvert R_1\rvert + \lvert R_2\rvert$ |
| $R_1 \cap R_2$ | $0 \dots \min(\lvert R_1\rvert, \lvert R_2\rvert)$ |
| $R_1 - R_2$ | $0 \dots \lvert R_1\rvert$ |
| $R_1 \times R_2$ | esattamente $\lvert R_1\rvert \cdot \lvert R_2\rvert$ |
| $R_1(A,B) \bowtie R_2(B,C)$ | $0 \dots \lvert R_1\rvert \cdot \lvert R_2\rvert$ · $\le \lvert R_1\rvert$ se $B$ è chiave di $R_2$ · $= \lvert R_1\rvert$ se in più c'è una FK da $R_1.B$ a $R_2$ (senza `NULL`) |

## Esercizi della lezione

**Proiezione e selezione**

| # | Esercizio | Dove |
|---|---|---|
| 1 | Quiz: cardinalità di $\pi_K(R)$ con $K$ superchiave | [[#^quiz-proj]] |
| 2 | `Persona`: sei esercizi sulle città (trentenni, nati ma non residenti, nativi, solo anziani…) | [[#^es-persona]] |

**Query guidate** (schema `Impiegati`, `Supervisione`)

| # | Esercizio | Dove |
|---|---|---|
| 3 | Query 1 — chi guadagna più di 40 | [[#^q1]] |
| 4 | Query 2 — solo matricola, nome ed età | [[#^q2]] |
| 5 | Query 3 — capi di chi guadagna più di 40 | [[#^q3]] |
| 6 | Query 4 — nome e stipendio di quei capi | [[#^q4]] |
| 7 | Query 5 — impiegati che guadagnano più del proprio capo | [[#^q5]] |
| 8 | Query 6 — capi i cui impiegati guadagnano tutti più di 40 | [[#^q6]] |

**RelaX** (schema "Progetti - by Torlone")

| # | Esercizio | Dove |
|---|---|---|
| 9 | Es. 1 — cognome e nome del dipartimento | [[#^relax1]] |
| 10 | Es. 2 — progetti con bilancio > 100.000 € e partecipanti | [[#^relax2]] |
| 11 | Es. 3 — impiegati che guadagnano più del direttore | [[#^relax3]] |
| 12 | Es. 4 — impiegati che partecipano solo a progetti > 100.000 € | [[#^relax4]] |
| 13 | Es. 5 — impiegati che non partecipano a progetti < 100.000 € | [[#^relax5]] |
| 14 | Es. 6 — come Es. 5, con cognome e stipendio | [[#^relax6]] |

**Schema «Stoccaggi»**

| # | Esercizio | Dove |
|---|---|---|
| 15 | Esercizi base (prezzo < 10, via Ripamonti, codice prodotto = codice magazzino, prezzo > 100) | [[#^st-base]] |
| 16 | Prodotti delle aziende di Milano | [[#^st-milano]] |
| 17 | Prodotti dell'azienda «Apple» | [[#^st-apple]] |
| 18 | Magazzini con un «iPhone» | [[#^st-iphone-mag]] |
| 19 | Città dei magazzini con un «iPhone» | [[#^st-iphone-citta]] |
| 20 | Prodotti stoccati nella città dell'azienda produttrice | [[#^st-citta-azienda]] |
| 21 | Prodotti di aziende milanesi in magazzini di Roma | [[#^st-mi-roma]] |
| 22 | Magazzini senza prodotti | [[#^st-mag-vuoti]] |
| 23 | Città sede di aziende o magazzini | [[#^st-citta-union]] |
| 24 | Coppie di prodotti diversi con lo stesso nome | [[#^st-coppie]] |
| 25 | Prodotti in almeno 2 magazzini | [[#^st-2mag]] |
| 26 | Prezzo del prodotto più caro | [[#^st-max]] |
| 27 | Aziende con almeno 2 prodotti omonimi | [[#^st-aziende-omonimi]] |

**Limiti dell'algebra**

| # | Esercizio | Dove |
|---|---|---|
| 28 | Chiusura transitiva di `Supervisione` (non esprimibile) | [[#^es-chiusura]] |
