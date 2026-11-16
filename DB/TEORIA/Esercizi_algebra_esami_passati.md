# Esercizi di algebra relazionale — esami passati

> Tutti gli esercizi di algebra relazionale presenti in `DB/TEORIA/vecchi_esami`, raggruppati per schema. Per ogni esercizio sono indicati l'appello di provenienza e il punteggio. La soluzione è nel riquadro richiudibile sotto il testo.
> Per gli operatori vedi [[Dispense_algebra_relazionale]]; per il resto delle prove vedi [[Esami_passati_DB]].

| # | Schema | Richiesta | Appello | Tecnica principale |
|---|---|---|---|---|
| 1 | Provini | Registi con provini il 1/1/2021 ad attori over 70 | 21/01/2021 | Join a tre tabelle + selezioni |
| 2 | Provini | Attori con provini MGM ma nessuno Warner | 21/01/2021 | Differenza ("ma nessuno") |
| 3 | Automobili | Targa delle auto SUV | 08/11/2022 (C) | Join su chiave composta |
| 4 | Automobili | Diciottenni con un SUV | 08/11/2022 (C) | Join a tre tabelle |
| 5 | Automobili | Persone con sia una BMW sia una KIA | 08/11/2022 (C) | Intersezione ("sia… sia…") |
| 6 | Automobili | Targa e anno delle auto di under 20 | 08/11/2022 (D) | Join + selezione |
| 7 | Automobili | Proprietari di auto tedesche | 08/11/2022 (D) | Join a tre tabelle |
| 8 | Automobili | Persone senza automobili | 08/11/2022 (D) | Differenza sulle chiavi + join |
| 9 | Libreria | Gialli di editori con più di 100 impiegati | 22/06/2023 | Join a tre tabelle |
| 10 | Libreria | Editori con edizioni nel 2020 ma non nel 2021 | 22/06/2023 | Differenza |

In fondo ci sono anche i [[#Quiz di algebra relazionale|due quiz di algebra]] dei parziali del 2022.

---

## Schema 1 — Provini cinematografici

> Appello del **21/01/2021** · file [[DB/TEORIA/vecchi_esami/BD1-Esercizi-21gen2021-1.pdf|BD1-Esercizi-21gen2021-1.pdf]]

- REGISTA(<u>CFregista</u>, Nome, Cognome, CasaDiProduzione)
- ATTORE(<u>CFattore</u>, Nome, Cognome, Età)
- PROVINO(<u>Regista, Attore, Data</u>) — FK: `Regista → REGISTA(CFregista)`, `Attore → ATTORE(CFattore)`

### Esercizio 1 (3 punti)
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

### Esercizio 2 (3 punti)
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

---

## Schema 2 — Case costruttrici, automobili e proprietari

> Prima prova parziale dell'**08/11/2022**, Compito C (file [[DB/TEORIA/vecchi_esami/primo_parziale_8_11_2022 (1).pdf|primo_parziale_8_11_2022 (1).pdf]]) e Compito D (file [[DB/TEORIA/vecchi_esami/Parziale_1_BD_-_08.11.2022 (2).pdf|Parziale_1_BD_-_08.11.2022 (2).pdf]]). I due compiti usano lo stesso schema con domande diverse.

Si assuma che nessun campo possa avere valore `NULL`.

- PERSONA(<u>CodiceFiscale</u>, Nome, Cognome, Età)
- CASACOSTRUTTRICE(<u>Codice</u>, NomeCasa, Nazione)
- MODELLOAUTO(<u>CodiceCasa, NomeModello</u>, Categoria) — FK: `CodiceCasa → CASACOSTRUTTRICE`
- AUTOMOBILE(<u>Targa</u>, Casa, Modello, Anno, Proprietario) — FK: `Casa → CASACOSTRUTTRICE`, `<Casa, Modello> → MODELLOAUTO`, `Proprietario → PERSONA`

(Nel testo del Compito D l'ultimo attributo di AUTOMOBILE compare come "Proprietà" e la relazione riferita come "CASACOSTRUZIONE": sono refusi.)

### Esercizio 3 (3 punti) — Compito C
Elencare la targa delle auto di categoria 'SUV'.

> [!success]- Soluzione
> $$\pi_{\text{Targa}}\big(\text{AUTOMOBILE} \bowtie_{\text{Casa}=\text{CodiceCasa} \,\wedge\, \text{Modello}=\text{NomeModello}} \sigma_{\text{Categoria}=\text{'SUV'}}(\text{MODELLOAUTO})\big)$$
>
> **Attenzione**: la chiave di MODELLOAUTO è **composta** (`CodiceCasa`, `NomeModello`), quindi il join va fatto su **entrambi** gli attributi. Confrontando solo `Modello = NomeModello`, due case con un modello omonimo verrebbero confuse.

### Esercizio 4 (4 punti) — Compito C
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

### Esercizio 5 (3 punti) — Compito C
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

### Esercizio 6 (3 punti) — Compito D
Elencare la targa e l'anno delle auto possedute da persone con meno di 20 anni.

> [!success]- Soluzione
> $$\pi_{\text{Targa},\text{Anno}}\big(\text{AUTOMOBILE} \bowtie_{\text{Proprietario}=\text{CodiceFiscale}} \sigma_{\text{Età}<20}(\text{PERSONA})\big)$$
>
> La selezione su `Età` si applica a PERSONA **prima** del join, così il join lavora su meno tuple.

### Esercizio 7 (4 punti) — Compito D
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

### Esercizio 8 (3 punti) — Compito D
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

---

## Schema 3 — Libreria

> Esame totale del **22/06/2023** · file [[DB/TEORIA/vecchi_esami/Esame_Totale_BD_22_06_23 (1).pdf|Esame_Totale_BD_22_06_23 (1).pdf]]

Si assume che ogni libro abbia un unico autore.

- SCRITTORE(<u>CodiceScrittore</u>, Nome, Cognome, AnnoDiNascita, CittàDiNascita)
- LIBRO(<u>CodiceLibro</u>, Autore, Titolo, Categoria) — FK: `Autore → SCRITTORE(CodiceScrittore)`
- EDIZIONE(<u>Editore, Libro, Anno</u>, Prezzo, NumCopie, NumeroPagine) — FK: `Editore → EDITORE(Sigla)`, `Libro → LIBRO(CodiceLibro)`
- EDITORE(<u>Sigla</u>, Nome, CittàDiSede, NumeroImpiegati)

### Esercizio 9 (3 punti)
Elencare il titolo dei libri di categoria "Giallo" pubblicati da editori con più di 100 impiegati.

> [!success]- Soluzione
> $$\pi_{\text{Titolo}}\Big(\sigma_{\text{Categoria}=\text{'Giallo'}}(\text{LIBRO}) \bowtie_{\text{CodiceLibro}=\text{Libro}} \big(\text{EDIZIONE} \bowtie_{\text{Editore}=\text{Sigla}} \sigma_{\text{NumeroImpiegati}>100}(\text{EDITORE})\big)\Big)$$
>
> EDIZIONE fa da "ponte" tra LIBRO ed EDITORE. SCRITTORE non serve.

### Esercizio 10 (3 punti)
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

---

## Quiz di algebra relazionale

I parziali del 2022 aprono la parte di algebra con un quiz a risposta multipla. Sono riportati come nell'originale (una sola risposta corretta).

### Quiz 1 (2 punti) — 08/11/2022, Compito C
Considerare la relazione R(A, B, C). Da quali tuple è composta la relazione $\text{SEL}_{A>B}(R)$?
- **(a)** Le tuple di R in cui il valore di A è maggiore di quello di B, più quelle per cui A è NULL ma B è diverso da NULL
- **(b)** Le tuple di R in cui il valore di A è maggiore di quello di B, più quelle per cui B è NULL ma A è diverso da NULL
- **(c)** Le tuple di R in cui il valore di A è maggiore di quello di B, più quelle per cui A è NULL oppure B è NULL (o entrambi sono NULL)
- **(d)** Le tuple di R in cui il valore di A e di B è diverso da NULL e il valore di A è maggiore di quello di B

> [!success]- Risposta
> **(d)** Se A o B è `NULL` il confronto `A > B` non è vero e la tupla viene scartata.

### Quiz 2 (2 punti) — 08/11/2022, Compito D
Si considerino due attributi A e B con domini $D_A = \{x, y, z\}$ e $D_B = \{5, 12\}$. Quale tra le seguenti è una relazione su (A, B)?
- **(a)** $\{(z, 5), (z, 5)\}$
- **(b)** $\{(5, z), (x, 12)\}$
- **(c)** $\{(z, 5), (x, 12)\}$
- **(d)** $\{(x, 10), (z, 5)\}$

> [!success]- Risposta
> **(c)** Ogni tupla ha il primo valore in $D_A$ e il secondo in $D_B$. La (a) elenca due volte la stessa tupla, cosa non ammessa in una relazione; la (b) ha una tupla con i valori in posizione invertita (la struttura è posizionale); nella (d) il valore 10 non appartiene a $D_B$.

---

## Schemi ricorrenti negli esercizi d'esame

| Nella richiesta compare… | Soluzione tipica | Esercizi |
|---|---|---|
| Condizioni su tabelle diverse | Selezioni sulle singole tabelle, poi join sulle coppie chiave esterna = chiave primaria | 1, 4, 6, 7, 9 |
| Chiave esterna **composta** | Join su **tutti** gli attributi della chiave (con $\wedge$) | 3, 4 |
| "…ma nessuno…", "…ma non…" | Differenza tra due proiezioni | 2, 10 |
| "Nessun/nessuna" (senza un secondo insieme) | Insieme completo meno chi soddisfa, poi join per recuperare gli altri attributi | 8 |
| "Sia… sia…" | Intersezione (o self-join) | 5 |
| Attributi omonimi in tabelle diverse | Proiezione anticipata o ridenominazione | 1, 8 |
| Albero dell'espressione | Sempre richiesto insieme alla formula negli esercizi da 3–4 punti | 1, 2, 4, 5, 7, 8, 10 |
