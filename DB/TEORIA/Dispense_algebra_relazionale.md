# Dispense di algebra relazionale

> Dispensa di approfondimento della lezione [[03 - Algebra relazionale]]. Ogni operatore è spiegato nel dettaglio e applicato alle tabelle di esempio definite qui sotto. In fondo c'è una [[#Tabella riassuntiva degli operatori|tabella riassuntiva]] con la sintassi formale e i collegamenti alle sezioni.

---

## 0. Base di dati di esempio

Una piccola università. Tutti gli esempi della dispensa usano queste istanze.

**Schema**
- STUDENTI(<u>Matricola</u>, Nome, Città, Anno)
- DOCENTI(<u>IdDoc</u>, Nome, Città)
- CORSI(<u>Codice</u>, Titolo, CFU, Docente) — FK `Docente → DOCENTI.IdDoc`
- ESAMI(<u>Studente, Corso</u>, Voto) — FK `Studente → STUDENTI.Matricola`, `Corso → CORSI.Codice`
- TUTOR(<u>Matricola</u>, Nome, Città, Anno) — **stesso schema** di STUDENTI (serve per gli operatori insiemistici)
- PROPEDEUTICITÀ(<u>Corso, Richiede</u>) — FK verso `CORSI.Codice` (serve per self-join e ricorsione)

**STUDENTI**

| Matricola | Nome | Città | Anno |
|---|---|---|---|
| 101 | Anna | Cagliari | 1 |
| 102 | Bruno | Sassari | 2 |
| 103 | Carla | Cagliari | 2 |
| 104 | Dario | Oristano | NULL |
| 105 | Elena | Nuoro | 1 |

**DOCENTI**

| IdDoc | Nome | Città |
|---|---|---|
| D1 | Rossi | Cagliari |
| D2 | Bianchi | Sassari |
| D3 | Verdi | Iglesias |

**CORSI**

| Codice | Titolo | CFU | Docente |
|---|---|---|---|
| BD | Basi di Dati | 9 | D1 |
| IUM | Interazione Uomo Macchina | 6 | D2 |
| ALG | Algoritmi | 9 | D1 |
| RET | Reti | 6 | D2 |

**ESAMI**

| Studente | Corso | Voto |
|---|---|---|
| 101 | BD | 28 |
| 101 | IUM | 30 |
| 102 | BD | 24 |
| 103 | ALG | 30 |
| 103 | BD | 27 |
| 104 | IUM | 22 |

**TUTOR**

| Matricola | Nome | Città | Anno |
|---|---|---|---|
| 102 | Bruno | Sassari | 2 |
| 103 | Carla | Cagliari | 2 |
| 201 | Fabio | Cagliari | 4 |

**PROPEDEUTICITÀ** (*il corso richiede il prerequisito*)

| Corso | Richiede |
|---|---|
| RET | ALG |
| ALG | BD |
| IUM | BD |

> [!info] Particolarità volute dell'istanza
> - **Elena (105)** non ha esami, il corso **RET** non ha esami e il docente **D3 Verdi** non tiene corsi: sono le tuple *dangling* per i join esterni.
> - **Dario** ha `Anno = NULL`: serve per mostrare come la selezione tratta i valori nulli.
> - STUDENTI e DOCENTI hanno **due attributi omonimi** (`Nome`, `Città`): servono per mostrare la trappola del join naturale e l'uso della ridenominazione.

---

## 1. Concetti generali

- L'algebra relazionale è un linguaggio **procedurale**: dice *come* calcolare il risultato (SQL è in gran parte **dichiarativo**).
- Ogni operatore prende **relazioni** e restituisce una **relazione** (*chiusura*): gli operatori si possono **comporre** liberamente.
- Il risultato è sempre un **insieme**: le tuple duplicate vengono eliminate.
- Due notazioni equivalenti: **a parole** (`SEL`, `PROJ`, `REN`, `JOIN`) e **matematica** ($\sigma$, $\pi$, $\rho$, $\bowtie$).

| Classe | Operatori |
|---|---|
| Unari | Selezione $\sigma$, proiezione $\pi$, ridenominazione $\rho$ |
| Insiemistici (binari) | Unione $\cup$, intersezione $\cap$, differenza $-$ |
| Di combinazione (binari) | Prodotto cartesiano $\times$, theta-join, equi-join, join naturale $\bowtie$, join esterni ⟕ ⟖ ⟗ |

---

## 2. Selezione

**Simbolo**: $\sigma_F(R)$ — a parole `SEL F (R)`

**Cosa fa**: filtro **orizzontale**. Restituisce le tuple di $R$ che soddisfano la condizione $F$.

- **Schema del risultato**: lo **stesso** di $R$.
- **Cardinalità**: $0 \le |\sigma_F(R)| \le |R|$.
- **Condizione** $F$: formula booleana di atomi $A \;\theta\; B$ o $A \;\theta\; c$, con $\theta \in \{=, \neq, <, >, \le, \ge\}$, combinati con $\wedge$ (AND), $\vee$ (OR), $\neg$ (NOT).

### Esempio 1 — condizione su una costante
*Studenti di Cagliari:* $\sigma_{\text{Città}=\text{'Cagliari'}}(\text{STUDENTI})$

| Matricola | Nome | Città | Anno |
|---|---|---|---|
| 101 | Anna | Cagliari | 1 |
| 103 | Carla | Cagliari | 2 |

### Esempio 2 — congiunzione (AND)
*Esami di BD con voto almeno 27:* $\sigma_{\text{Corso}=\text{'BD'} \,\wedge\, \text{Voto} \ge 27}(\text{ESAMI})$

| Studente | Corso | Voto |
|---|---|---|
| 101 | BD | 28 |
| 103 | BD | 27 |

### Esempio 3 — disgiunzione (OR)
*Esami con 30 oppure con voto sotto il 24:* $\sigma_{\text{Voto}=30 \,\vee\, \text{Voto} < 24}(\text{ESAMI})$

| Studente | Corso | Voto |
|---|---|---|
| 101 | IUM | 30 |
| 103 | ALG | 30 |
| 104 | IUM | 22 |

### Esempio 4 — negazione (NOT)
*Studenti che non sono di Cagliari:* $\sigma_{\neg(\text{Città}=\text{'Cagliari'})}(\text{STUDENTI})$ → Bruno, Dario, Elena.

### Esempio 5 — valori nulli
Un confronto con `NULL` non è mai vero: la tupla viene scartata.

- $\sigma_{\text{Anno} \ge 2}(\text{STUDENTI})$ → Bruno, Carla
- $\sigma_{\text{Anno} < 2}(\text{STUDENTI})$ → Anna, Elena

Le due selezioni insieme **non** restituiscono tutti gli studenti: manca Dario. Per includerlo bisogna nominare esplicitamente il caso nullo:

$$\sigma_{\text{Anno} \ge 2}(\text{STUDENTI}) \cup \sigma_{\text{Anno} < 2}(\text{STUDENTI}) \cup \sigma_{\text{Anno IS NULL}}(\text{STUDENTI}) = \text{STUDENTI}$$

> [!tip] La selezione può confrontare anche **due attributi della stessa tupla** (es. $\sigma_{\text{Città} = \text{CittàDoc}}$), ma non tuple diverse: per quello servono join o self-join.

---

## 3. Proiezione

**Simbolo**: $\pi_X(R)$ — a parole `PROJ X (R)`

**Cosa fa**: filtro **verticale**. Tiene solo gli attributi della lista $X$, per **tutte** le tuple di $R$.

- **Schema del risultato**: gli attributi $X$.
- **Cardinalità**: $|\pi_X(R)| \le |R|$, perché le tuple diventate uguali **collassano**. Se $X$ è una **superchiave**, la cardinalità è esattamente $|R|$.

### Esempio 1 — nessun duplicato
*Nome e città degli studenti:* $\pi_{\text{Nome},\text{Città}}(\text{STUDENTI})$ → 5 tuple (Anna Cagliari, Bruno Sassari, Carla Cagliari, Dario Oristano, Elena Nuoro).

### Esempio 2 — duplicati eliminati
*Città degli studenti:* $\pi_{\text{Città}}(\text{STUDENTI})$

| Città |
|---|
| Cagliari |
| Sassari |
| Oristano |
| Nuoro |

Gli studenti sono 5, le città 4: "Cagliari" compare due volte nella proiezione grezza e collassa in una sola tupla.

### Esempio 3 — chiave e non chiave
- $\pi_{\text{Corso}}(\text{ESAMI})$ → BD, IUM, ALG: **3** tuple su 6.
- $\pi_{\text{Studente},\text{Corso}}(\text{ESAMI})$ → **6** tuple, perché `<Studente, Corso>` è la chiave.

### Esempio 4 — selezione + proiezione
*Nomi degli studenti di Cagliari:* $\pi_{\text{Nome}}(\sigma_{\text{Città}=\text{'Cagliari'}}(\text{STUDENTI}))$ → Anna, Carla.

> [!warning] L'ordine conta
> $\sigma_{\text{Città}=\text{'Cagliari'}}(\pi_{\text{Nome}}(\text{STUDENTI}))$ è **sbagliata**: dopo la proiezione l'attributo `Città` non esiste più.
> Regola: prima **selezioni** con gli attributi che servono per filtrare, poi **proietti** quelli da mostrare.

---

## 4. Ridenominazione

**Simbolo**: $\rho_{B_1,\dots,B_k \leftarrow A_1,\dots,A_k}(R)$ oppure $\rho_{S(B_1,\dots,B_n)}(R)$ — a parole `REN B ← A (R)`

**Cosa fa**: cambia il **nome degli attributi** (ed eventualmente della relazione). Modifica lo **schema**, lascia invariata l'**istanza**.

- **Cardinalità**: $= |R|$.

### A cosa serve
1. Rendere **compatibili gli schemi** per $\cup$, $\cap$, $-$.
2. Far coincidere i nomi per il **join naturale**.
3. **Distinguere** gli attributi omonimi di tabelle diverse, o della stessa tabella usata due volte (**self-join**).

### Esempio 1 — un attributo
$\rho_{\text{Matricola} \leftarrow \text{Studente}}(\text{ESAMI})$ ha schema `(Matricola, Corso, Voto)` con le stesse 6 tuple.

### Esempio 2 — più attributi insieme
$\rho_{\text{NomeDoc},\text{CittàDoc} \leftarrow \text{Nome},\text{Città}}(\text{DOCENTI})$ ha schema `(IdDoc, NomeDoc, CittàDoc)`. Le due liste si corrispondono **per posizione**.

### Esempio 3 — relazione e tutti gli attributi
$\rho_{\text{S2}(\text{M2},\text{N2},\text{C2},\text{A2})}(\text{STUDENTI})$ crea una "copia" di STUDENTI con nomi tutti diversi, pronta per un self-join.

---

## 5. Operatori insiemistici

Si applicano solo a relazioni **con lo stesso schema** (stessi attributi). Se gli attributi hanno nomi diversi, prima si usa $\rho$; se sono di più, prima si usa $\pi$.

### 5.1 Unione
Tuple che stanno in **almeno una** delle due relazioni. Cardinalità: $\max(|R|,|S|) \le |R \cup S| \le |R| + |S|$.

*Studenti o tutor:* $\text{STUDENTI} \cup \text{TUTOR}$ → **6** tuple: le 5 di STUDENTI più Fabio (201). Bruno e Carla, presenti in entrambe, compaiono una volta sola.

*Tutte le città presenti nel database:* $\pi_{\text{Città}}(\text{STUDENTI}) \cup \pi_{\text{Città}}(\text{DOCENTI})$ → Cagliari, Sassari, Oristano, Nuoro, Iglesias. Qui l'attributo ha già lo stesso nome, quindi non serve $\rho$.

### 5.2 Intersezione
Tuple presenti **in entrambe**. Cardinalità: $0 \le |R \cap S| \le \min(|R|,|S|)$.

*Studenti che sono anche tutor:* $\text{STUDENTI} \cap \text{TUTOR}$ → Bruno (102), Carla (103).

*Città in cui vivono sia studenti sia docenti:* $\pi_{\text{Città}}(\text{STUDENTI}) \cap \pi_{\text{Città}}(\text{DOCENTI})$ → Cagliari, Sassari.

*Studenti che hanno sostenuto **sia** BD **sia** IUM:*
$$\pi_{\text{Studente}}(\sigma_{\text{Corso}=\text{'BD'}}(\text{ESAMI})) \cap \pi_{\text{Studente}}(\sigma_{\text{Corso}=\text{'IUM'}}(\text{ESAMI}))$$
$\{101, 102, 103\} \cap \{101, 104\} = \{101\}$ → Anna.

> L'intersezione è derivabile: $R \cap S = R - (R - S)$.

### 5.3 Differenza
Tuple di $R$ che **non** stanno in $S$. **Non è commutativa**. Cardinalità: $0 \le |R - S| \le |R|$.

- $\text{STUDENTI} - \text{TUTOR}$ → Anna, Dario, Elena
- $\text{TUTOR} - \text{STUDENTI}$ → Fabio
- $\pi_{\text{Città}}(\text{STUDENTI}) - \pi_{\text{Città}}(\text{DOCENTI})$ → Oristano, Nuoro
- $\pi_{\text{Città}}(\text{DOCENTI}) - \pi_{\text{Città}}(\text{STUDENTI})$ → Iglesias

#### Pattern "nessuno": insieme completo meno chi soddisfa
*Matricole degli studenti che **non** hanno sostenuto BD:*
$$\pi_{\text{Matricola}}(\text{STUDENTI}) - \rho_{\text{Matricola} \leftarrow \text{Studente}}(\pi_{\text{Studente}}(\sigma_{\text{Corso}=\text{'BD'}}(\text{ESAMI})))$$
$\{101,\dots,105\} - \{101, 102, 103\} = \{104, 105\}$. La ridenominazione serve perché la differenza richiede lo stesso schema.

> [!warning] Partire dall'insieme giusto
> Se si partisse da $\pi_{\text{Studente}}(\text{ESAMI})$ invece che da STUDENTI, Elena (che non ha esami) sparirebbe dal risultato.

#### Pattern "solo / tutti": tutti meno chi ha almeno un'eccezione
*Studenti che hanno preso **solo** voti ≥ 27:*
$$\pi_{\text{Studente}}(\text{ESAMI}) - \pi_{\text{Studente}}(\sigma_{\text{Voto} < 27}(\text{ESAMI}))$$
$\{101, 102, 103, 104\} - \{102, 104\} = \{101, 103\}$ → Anna e Carla.

---

## 6. Prodotto cartesiano

**Simbolo**: $R \times S$ — a parole `R JOIN S` senza condizione

**Cosa fa**: abbina **ogni** tupla di $R$ con **ogni** tupla di $S$.

- **Cosa specificare**: niente.
- **Schema del risultato**: tutti gli attributi di entrambe. Se ci sono attributi omonimi vanno prima rinominati con $\rho$.
- **Cardinalità**: esattamente $|R| \cdot |S|$.

### Esempio — tutte le combinazioni possibili
*Tutte le coppie (studente del 1° anno, corso da 9 CFU):*
$$\pi_{\text{Matricola}}(\sigma_{\text{Anno}=1}(\text{STUDENTI})) \times \pi_{\text{Codice}}(\sigma_{\text{CFU}=9}(\text{CORSI}))$$

| Matricola | Codice |
|---|---|
| 101 | BD |
| 101 | ALG |
| 105 | BD |
| 105 | ALG |

$2 \times 2 = 4$ tuple. Dario non compare perché il suo anno è `NULL`.

> [!warning] Attributi omonimi
> $\text{STUDENTI} \times \text{DOCENTI}$ avrebbe due attributi `Nome` e due `Città`: va scritto ad esempio come $\text{STUDENTI} \times \rho_{\text{NomeDoc},\text{CittàDoc} \leftarrow \text{Nome},\text{Città}}(\text{DOCENTI})$ ($5 \times 3 = 15$ tuple).

Da solo il prodotto cartesiano ha poco senso: quasi sempre è seguito da una **selezione**, e insieme formano il theta-join.

---

## 7. Theta-join

**Simbolo**: $R \bowtie_F S$ — a parole `R JOIN F S`

**Definizione**: $R \bowtie_F S = \sigma_F(R \times S)$

- **Cosa specificare**: una **condizione** $F$ formata da confronti $A \;\theta\; B$ (con $A$ attributo di $R$ e $B$ di $S$), di solito collegati da $\wedge$.
- **Schema del risultato**: tutti gli attributi di entrambe le relazioni.
- **Cardinalità**: $0 \le |R \bowtie_F S| \le |R| \cdot |S|$.

### Esempio — condizione con un confronto diverso dall'uguaglianza
*Esami con un voto più alto di quello preso da Dario (104):*
$$\pi_{\text{Studente},\text{Corso},\text{Voto}}\Big(\text{ESAMI} \bowtie_{\text{Voto} > \text{V2}} \rho_{\text{S2},\text{C2},\text{V2} \leftarrow \text{Studente},\text{Corso},\text{Voto}}(\sigma_{\text{Studente}=104}(\text{ESAMI}))\Big)$$

Il voto di Dario è 22; il risultato contiene tutti gli esami tranne il suo:

| Studente | Corso | Voto |
|---|---|---|
| 101 | BD | 28 |
| 101 | IUM | 30 |
| 102 | BD | 24 |
| 103 | ALG | 30 |
| 103 | BD | 27 |

---

## 8. Equi-join

**Simbolo**: $R \bowtie_{A=B} S$

**Cosa fa**: è un theta-join in cui la condizione contiene **solo uguaglianze**, di solito **chiave esterna = chiave primaria**. È il join che si usa più spesso.

- **Schema del risultato**: **entrambe** le colonne del confronto restano, anche se contengono gli stessi valori.

### Esempio 1 — due tabelle
$\text{ESAMI} \bowtie_{\text{Studente}=\text{Matricola}} \text{STUDENTI}$

| Studente | Corso | Voto | Matricola | Nome | Città | Anno |
|---|---|---|---|---|---|---|
| 101 | BD | 28 | 101 | Anna | Cagliari | 1 |
| 101 | IUM | 30 | 101 | Anna | Cagliari | 1 |
| 102 | BD | 24 | 102 | Bruno | Sassari | 2 |
| 103 | ALG | 30 | 103 | Carla | Cagliari | 2 |
| 103 | BD | 27 | 103 | Carla | Cagliari | 2 |
| 104 | IUM | 22 | 104 | Dario | Oristano | NULL |

Elena non compare: non ha esami (tupla *dangling*). `Studente` e `Matricola` restano due colonne distinte.

### Esempio 2 — catena di tre tabelle
*Nome dello studente, titolo del corso e voto di ogni esame:*
$$\pi_{\text{Nome},\text{Titolo},\text{Voto}}\big(\text{STUDENTI} \bowtie_{\text{Matricola}=\text{Studente}} \text{ESAMI} \bowtie_{\text{Corso}=\text{Codice}} \text{CORSI}\big)$$

| Nome | Titolo | Voto |
|---|---|---|
| Anna | Basi di Dati | 28 |
| Anna | Interazione Uomo Macchina | 30 |
| Bruno | Basi di Dati | 24 |
| Carla | Algoritmi | 30 |
| Carla | Basi di Dati | 27 |
| Dario | Interazione Uomo Macchina | 22 |

### Esempio 3 — quattro tabelle con ridenominazione
*Studenti che hanno sostenuto un esame con un docente della propria città:*
$$\pi_{\text{Nome}}\Big(\sigma_{\text{Città}=\text{CittàDoc}}\big(\text{STUDENTI} \bowtie_{\text{Matricola}=\text{Studente}} \text{ESAMI} \bowtie_{\text{Corso}=\text{Codice}} \text{CORSI} \bowtie_{\text{Docente}=\text{IdDoc}} \rho_{\text{NomeDoc},\text{CittàDoc} \leftarrow \text{Nome},\text{Città}}(\text{DOCENTI})\big)\Big)$$

| Esame | Città studente | Città docente | Esito |
|---|---|---|---|
| Anna – BD (Rossi) | Cagliari | Cagliari | ✅ |
| Anna – IUM (Bianchi) | Cagliari | Sassari | ❌ |
| Bruno – BD (Rossi) | Sassari | Cagliari | ❌ |
| Carla – ALG (Rossi) | Cagliari | Cagliari | ✅ |
| Carla – BD (Rossi) | Cagliari | Cagliari | ✅ |
| Dario – IUM (Bianchi) | Oristano | Sassari | ❌ |

Risultato: **Anna, Carla**. Senza $\rho$ ci sarebbero due attributi `Nome` e due `Città` e la condizione sarebbe ambigua.

**Albero dell'espressione**:
```
                         π Nome
                           │
                   σ Città = CittàDoc
                           │
                    ⋈ Docente=IdDoc
               ┌───────────┴───────────┐
        ⋈ Corso=Codice        ρ NomeDoc,CittàDoc ← Nome,Città
        ┌─────┴─────┐                   │
 ⋈ Matricola=Studente  CORSI         DOCENTI
   ┌─────┴─────┐
STUDENTI     ESAMI
```

---

## 9. Join naturale

**Simbolo**: $R \bowtie S$ — a parole `NAT_JOIN`

**Cosa fa**: combina le tuple che hanno **valori uguali su tutti gli attributi con lo stesso nome**.

- **Cosa specificare**: **niente**. La condizione è implicita: uguaglianza su tutti gli attributi comuni.
- **Schema del risultato**: l'unione degli attributi; quelli comuni compaiono **una sola volta**.
- Se **non ci sono attributi comuni** diventa un prodotto cartesiano: $\text{STUDENTI} \bowtie \text{CORSI} = \text{STUDENTI} \times \text{CORSI}$.

### Esempio 1 — far coincidere i nomi con ρ
$\text{CORSI} \bowtie \rho_{\text{Docente} \leftarrow \text{IdDoc}}(\text{DOCENTI})$ (attributo comune: `Docente`)

| Codice | Titolo | CFU | Docente | Nome | Città |
|---|---|---|---|---|---|
| BD | Basi di Dati | 9 | D1 | Rossi | Cagliari |
| IUM | Interazione Uomo Macchina | 6 | D2 | Bianchi | Sassari |
| ALG | Algoritmi | 9 | D1 | Rossi | Cagliari |
| RET | Reti | 6 | D2 | Bianchi | Sassari |

`Docente` compare una sola volta. D3 Verdi non tiene corsi e sparisce.

### Esempio 2 — cardinalità prevedibile
$\rho_{\text{Matricola} \leftarrow \text{Studente}}(\text{ESAMI}) \bowtie \text{STUDENTI}$ ha esattamente **6** tuple, quante ESAMI. `Matricola` è chiave di STUDENTI ed esiste una FK da ESAMI verso STUDENTI, quindi ogni esame si abbina a **uno e un solo** studente.

### Esempio 3 — ⚠️ la trappola degli attributi omonimi
*Volevo abbinare studenti e docenti della stessa città:* $\text{STUDENTI} \bowtie \text{DOCENTI}$

Gli attributi comuni sono `Nome` **e** `Città`: il join cerca coppie con lo **stesso nome e la stessa città**. Non ce ne sono, quindi il risultato è **vuoto**.

Versione corretta (confronto solo sulla città):
$$\pi_{\text{Nome},\text{Città}}(\text{STUDENTI}) \bowtie \rho_{\text{NomeDoc} \leftarrow \text{Nome}}(\pi_{\text{Nome},\text{Città}}(\text{DOCENTI}))$$
→ Anna–Rossi (Cagliari), Carla–Rossi (Cagliari), Bruno–Bianchi (Sassari).

### Cardinalità del join
Per $R_1(A, B)$ e $R_2(B, C)$:

| Condizione | Cardinalità |
|---|---|
| In generale | $0 \le \lvert R_1 \bowtie R_2 \rvert \le \lvert R_1 \rvert \cdot \lvert R_2 \rvert$ |
| $B$ chiave di $R_2$ | $0 \le \lvert R_1 \bowtie R_2 \rvert \le \lvert R_1 \rvert$ |
| $B$ chiave di $R_2$ **e** FK da $R_1.B$ a $R_2$ (senza `NULL`) | $\lvert R_1 \bowtie R_2 \rvert = \lvert R_1 \rvert$ |

---

## 10. Join esterni (outer join)

**Simboli**: ⟕ (sinistro, `JOIN_LEFT`), ⟖ (destro, `JOIN_RIGHT`), ⟗ (completo, `JOIN_FULL`)

**Cosa fa**: come il join interno, ma **conserva le tuple senza corrispondenza**, riempiendo con `NULL` gli attributi dell'altra relazione.

- **Cosa specificare**: il **lato** (sinistro, destro o completo) più la condizione di join, esplicita come nell'equi-join oppure implicita come nel join naturale.
- **Cardinalità**: almeno quella del join interno corrispondente.

| Tipo           | Conserva                                |
| -------------- | --------------------------------------- |
| **Sinistro** ⟕ | tutte le tuple del **primo** operando   |
| **Destro** ⟖   | tutte le tuple del **secondo** operando |
| **Completo** ⟗ | tutte le tuple di **entrambi**          |

Si usano quando la richiesta dice "**anche** quelli senza…".

### Esempio 1 — sinistro: anche gli studenti senza esami
$\pi_{\text{Nome},\text{Corso},\text{Voto}}(\text{STUDENTI} ⟕_{\text{Matricola}=\text{Studente}} \text{ESAMI})$

| Nome | Corso | Voto |
|---|---|---|
| Anna | BD | 28 |
| Anna | IUM | 30 |
| Bruno | BD | 24 |
| Carla | ALG | 30 |
| Carla | BD | 27 |
| Dario | IUM | 22 |
| Elena | NULL | NULL |

### Esempio 2 — sinistro: anche i corsi senza esami
$\pi_{\text{Titolo},\text{Studente},\text{Voto}}(\text{CORSI} ⟕_{\text{Codice}=\text{Corso}} \text{ESAMI})$ → le 6 righe degli esami più **Reti, NULL, NULL**.

### Esempio 3 — destro: anche i docenti senza corsi
$\pi_{\text{Titolo},\text{Nome}}(\text{CORSI} ⟖_{\text{Docente}=\text{IdDoc}} \text{DOCENTI})$

| Titolo | Nome |
|---|---|
| Basi di Dati | Rossi |
| Interazione Uomo Macchina | Bianchi |
| Algoritmi | Rossi |
| Reti | Bianchi |
| NULL | Verdi |

### Esempio 4 — completo: corrispondenze mancanti da entrambi i lati
*Studenti e docenti della stessa città, compresi quelli senza corrispondenza* (join naturale su `Città`):
$$\pi_{\text{Nome},\text{Città}}(\text{STUDENTI}) ⟗ \rho_{\text{NomeDoc} \leftarrow \text{Nome}}(\pi_{\text{Nome},\text{Città}}(\text{DOCENTI}))$$

| Nome | Città | NomeDoc |
|---|---|---|
| Anna | Cagliari | Rossi |
| Carla | Cagliari | Rossi |
| Bruno | Sassari | Bianchi |
| Dario | Oristano | NULL |
| Elena | Nuoro | NULL |
| NULL | Iglesias | Verdi |

---

## 11. Self-join

Non è un operatore nuovo: è un join (o un prodotto seguito da selezione) tra una relazione e **sé stessa**. Serve a **confrontare tuple diverse della stessa relazione**.

- **Cosa specificare**: una **ridenominazione** $\rho$ di almeno una delle due copie (altrimenti gli attributi coincidono) e la condizione.

### Esempio 1 — coppie di tuple
*Coppie di studenti della stessa città:* con $\text{S1} = \rho_{M1,N1,C1,A1 \leftarrow \text{Matricola},\text{Nome},\text{Città},\text{Anno}}(\text{STUDENTI})$ e S2 analoga:
$$\pi_{N1,N2}(\sigma_{C1=C2 \,\wedge\, M1<M2}(\text{S1} \times \text{S2}))$$
→ **(Anna, Carla)**. Con $M1 \neq M2$ uscirebbero sia (Anna, Carla) sia (Carla, Anna); con $<$ si tiene una sola coppia per confronto e si escludono anche le coppie di uno studente con sé stesso.

### Esempio 2 — "almeno 2"
*Studenti con almeno 2 esami:* con $\text{E1} = \rho_{S1,C1,V1}(\text{ESAMI})$ ed E2 analoga:
$$\pi_{S1}(\sigma_{S1=S2 \,\wedge\, C1 \neq C2}(\text{E1} \times \text{E2}))$$
→ **101, 103** (Anna e Carla).

### Esempio 3 — il massimo senza MAX
*Voto più alto:* con $\text{V1} = \rho_{V1 \leftarrow \text{Voto}}(\pi_{\text{Voto}}(\text{ESAMI}))$ e V2 analoga:
$$\pi_{\text{Voto}}(\text{ESAMI}) - \rho_{\text{Voto} \leftarrow V2}(\pi_{V2}(\sigma_{V2 < V1}(\text{V1} \times \text{V2})))$$
Voti: {28, 30, 24, 27, 22}. Quelli minori di almeno un altro: {28, 24, 27, 22}. Risultato: **30**.

### Esempio 4 — due livelli di una gerarchia
*Prerequisiti di secondo livello:*
$$\pi_{\text{Corso},R2}\big(\text{PROPEDEUTICITÀ} \bowtie_{\text{Richiede}=C2} \rho_{C2,R2 \leftarrow \text{Corso},\text{Richiede}}(\text{PROPEDEUTICITÀ})\big)$$
→ **(RET, BD)**: RET richiede ALG, che richiede BD.

---

## 12. Divisione (per ogni)

La divisione non è tra gli operatori presentati a lezione, ma le richieste "**per ogni** / **tutti i**" si esprimono con gli operatori di base tramite una **doppia differenza**.

*Studenti che hanno sostenuto **tutti** i corsi da 9 CFU (BD e ALG):*
$$\pi_{\text{Matricola}}(\text{STUDENTI}) - \pi_{\text{Matricola}}\Big(\big(\pi_{\text{Matricola}}(\text{STUDENTI}) \times \pi_{\text{Codice}}(\sigma_{\text{CFU}=9}(\text{CORSI}))\big) - \rho_{\text{Matricola},\text{Codice} \leftarrow \text{Studente},\text{Corso}}(\pi_{\text{Studente},\text{Corso}}(\text{ESAMI}))\Big)$$

1. Tutte le coppie (studente, corso da 9 CFU): $5 \times 2 = 10$.
2. Meno le coppie effettivamente sostenute → restano le coppie **mancanti**: 101-ALG, 102-ALG, 104-BD, 104-ALG, 105-BD, 105-ALG.
3. Gli studenti a cui manca qualcosa: {101, 102, 104, 105}.
4. Tutti gli studenti meno questi → **{103}** (Carla).

---

## 13. Confronto tra i join

Tutti i join combinano le tuple di due relazioni. Si differenziano per due aspetti:
1. **quale condizione** decide se due tuple si abbinano;
2. **cosa succede alle tuple senza corrispondenza** (*dangling*).

| Join | Cosa specifichi | Attributi comuni nel risultato | Tuple senza corrispondenza |
|---|---|---|---|
| [[#6. Prodotto cartesiano\|Prodotto cartesiano]] $\times$ | niente | duplicati (servirebbe $\rho$) | — (tutte le combinazioni) |
| [[#7. Theta-join\|Theta-join]] $\bowtie_F$ | condizione qualsiasi | entrambi | perse |
| [[#8. Equi-join\|Equi-join]] $\bowtie_{A=B}$ | uguaglianze | entrambi | perse |
| [[#9. Join naturale\|Join naturale]] $\bowtie$ | niente (stessi nomi) | **una sola volta** | perse |
| [[#10. Join esterni (outer join)\|Join esterni]] ⟕ ⟖ ⟗ | lato + condizione | come equi- o naturale | **conservate con `NULL`** |
| [[#11. Self-join\|Self-join]] | $\rho$ + condizione | rinominati | dipende dal join usato |

### Corrispondenza in SQL

```sql
SELECT * FROM ESAMI, STUDENTI;                                          -- prodotto cartesiano
SELECT * FROM ESAMI JOIN STUDENTI ON Studente = Matricola;              -- equi/theta-join (condizione in ON)
SELECT * FROM CORSI NATURAL JOIN DOCENTI;                               -- join naturale (attenzione ai nomi!)
SELECT * FROM STUDENTI LEFT JOIN ESAMI ON Matricola = Studente;         -- esterno sinistro
SELECT * FROM CORSI RIGHT JOIN DOCENTI ON Docente = IdDoc;              -- esterno destro
SELECT * FROM STUDENTI FULL JOIN DOCENTI ON STUDENTI.Città = DOCENTI.Città;  -- esterno completo
```

> [!tip] Regola pratica per gli esercizi
> - Usa quasi sempre l'**equi-join** sulla coppia **chiave esterna = chiave primaria**.
> - Passa all'**outer join** solo se servono anche le tuple senza corrispondenza.
> - Usa la **ridenominazione** quando c'è lo stesso nome di attributo in due tabelle o la stessa tabella in due ruoli.

---

## 14. Limiti dell'algebra relazionale

Non si possono esprimere:
1. **Valori calcolati**: conversioni, somme tra attributi, aggregati come `COUNT`, `SUM`, `AVG`. Ad esempio la media dei voti di Anna.
2. **Ricorsione**: la **chiusura transitiva**.

**Esempio**: *tutti i prerequisiti, diretti e indiretti, di ogni corso.* La chiusura di PROPEDEUTICITÀ è:

| Corso | Prerequisito |
|---|---|
| RET | ALG |
| RET | BD |
| ALG | BD |
| IUM | BD |

Con un self-join si raggiungono i prerequisiti di livello 2 ([[#11. Self-join|esempio 4]]), con due self-join quelli di livello 3, e così via. Nessuna espressione con un numero **fisso** di join funziona però per gerarchie di **profondità arbitraria**.

---

## Tabella riassuntiva degli operatori

| Operatore                                                 | Simbolo                       | Sintassi formale                                   | Esempio sulle tabelle della dispensa                                                                                                                    | Schema del risultato                 | Cardinalità                                |
| --------------------------------------------------------- | ----------------------------- | -------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------ | ------------------------------------------ |
| [[#2. Selezione\|Selezione]]                              | $\sigma$                      | $\sigma_F(R)$                                      | $\sigma_{\text{Città}=\text{'Cagliari'}}(\text{STUDENTI})$                                                                                              | uguale a $R$                         | $\le \lvert R\rvert$                       |
| [[#3. Proiezione\|Proiezione]]                            | $\pi$                         | $\pi_{A_1,\dots,A_k}(R)$                           | $\pi_{\text{Nome},\text{Città}}(\text{STUDENTI})$                                                                                                       | $A_1,\dots,A_k$                      | $\le \lvert R\rvert$ ($=$ se superchiave)  |
| [[#4. Ridenominazione\|Ridenominazione]]                  | $\rho$                        | $\rho_{B_1,\dots,B_k \leftarrow A_1,\dots,A_k}(R)$ | $\rho_{\text{Matricola} \leftarrow \text{Studente}}(\text{ESAMI})$                                                                                      | attributi rinominati                 | $= \lvert R\rvert$                         |
| [[#5.1 Unione\|Unione]]                                   | $\cup$                        | $R \cup S$ (stesso schema)                         | $\text{STUDENTI} \cup \text{TUTOR}$                                                                                                                     | uguale                               | $\le \lvert R\rvert + \lvert S\rvert$      |
| [[#5.2 Intersezione\|Intersezione]]                       | $\cap$                        | $R \cap S$ (stesso schema)                         | $\text{STUDENTI} \cap \text{TUTOR}$                                                                                                                     | uguale                               | $\le \min(\lvert R\rvert, \lvert S\rvert)$ |
| [[#5.3 Differenza\|Differenza]]                           | $-$                           | $R - S$ (stesso schema)                            | $\text{STUDENTI} - \text{TUTOR}$                                                                                                                        | uguale                               | $\le \lvert R\rvert$                       |
| [[#6. Prodotto cartesiano\|Prodotto cartesiano]]          | $\times$                      | $R \times S$                                       | $\pi_{\text{Matricola}}(\text{STUDENTI}) \times \pi_{\text{Codice}}(\text{CORSI})$                                                                      | $X_R \cup X_S$ (disgiunti)           | $= \lvert R\rvert \cdot \lvert S\rvert$    |
| [[#7. Theta-join\|Theta-join]]                            | $\bowtie_F$                   | $R \bowtie_F S = \sigma_F(R \times S)$             | $\text{ESAMI} \bowtie_{\text{Voto} > \text{V2}} \rho_{\text{S2},\text{C2},\text{V2} \leftarrow \text{Studente},\text{Corso},\text{Voto}}(\text{ESAMI})$ | $X_R \cup X_S$                       | $\le \lvert R\rvert \cdot \lvert S\rvert$  |
| [[#8. Equi-join\|Equi-join]]                              | $\bowtie_{A=B}$               | $R \bowtie_{A=B} S$                                | $\text{ESAMI} \bowtie_{\text{Studente}=\text{Matricola}} \text{STUDENTI}$                                                                               | $X_R \cup X_S$ (entrambe le colonne) | $\le \lvert R\rvert \cdot \lvert S\rvert$  |
| [[#9. Join naturale\|Join naturale]]                      | $\bowtie$                     | $R \bowtie S$                                      | $\text{CORSI} \bowtie \rho_{\text{Docente} \leftarrow \text{IdDoc}}(\text{DOCENTI})$                                                                    | $X_R \cup X_S$ (comuni una volta)    | $\le \lvert R\rvert \cdot \lvert S\rvert$  |
| [[#10. Join esterni (outer join)\|Join esterno sinistro]] | ⟕                             | `R ⟕ F S`                                          | `STUDENTI ⟕ Matricola=Studente ESAMI`                                                                                                                   | come il join, con `NULL`             | $\ge$ join interno                         |
| [[#10. Join esterni (outer join)\|Join esterno destro]]   | ⟖                             | `R ⟖ F S`                                          | `CORSI ⟖ Docente=IdDoc DOCENTI`                                                                                                                         | come il join, con `NULL`             | $\ge$ join interno                         |
| [[#10. Join esterni (outer join)\|Join esterno completo]] | ⟗                             | `R ⟗ F S`                                          | `STUDENTI ⟗ DOCENTI` (su `Città`, dopo $\pi$ e $\rho$)                                                                                                  | come il join, con `NULL`             | $\ge$ join interno                         |
| [[#11. Self-join\|Self-join]]                             | $\rho$ + $\bowtie$ / $\times$ | $\sigma_F(R \times \rho_{\dots}(R))$               | $\pi_{S1}(\sigma_{S1=S2 \wedge C1 \neq C2}(\text{E1} \times \text{E2}))$                                                                                | attributi rinominati                 | dipende dalla condizione                   |
| [[#12. Divisione (per ogni)\|Divisione ("per ogni")]]     | doppia $-$                    | $\pi_A(R) - \pi_A((\pi_A(R) \times S) - T)$        | studenti con tutti i corsi da 9 CFU                                                                                                                     | $A$                                  | $\le \lvert \pi_A(R)\rvert$                |
