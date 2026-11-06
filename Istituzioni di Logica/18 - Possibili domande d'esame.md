---
tags: [logica, istituzioni-di-logica, esame, domande, ripasso]
fonte: "Possibili domande esame 2024/2025" (primi due moduli)
---
# Possibili domande d'esame — risposte guidate
← [[17 - Esercizi aggiuntivi ND e PK]] | [[00 - Indice]]

> [!info] Come leggere questa nota
> L'elenco ufficiale copre i **primi due moduli**. Per ogni domanda sui moduli 2–3 trovi:
> - cosa si valuta (dalla scheda Esse3);
> - una traccia di risposta;
> - il link alla nota.
>
> Le domande del modulo 1 (*Ragionare nel quotidiano*) sono in fondo, con risposte brevi.
>
> ⚠️ Nelle domande "fai la derivazione" si guarda **più al processo che al prodotto**: esercitati a **spiegare a voce** ogni passo.

---

## Modulo 2 — Percorsi di logica e dispense

### 1. "Le presento una dimostrazione (già fatta) in HK: mi spieghi ogni passaggio"
> [!abstract] Cosa si valuta
> Sai dire come è giustificato ogni passo (**assioma** o **modus ponens**) e trovare l'**istanza di sostituzione** che produce ciascuna formula.

> [!success]- Traccia di risposta
> Per ogni riga:
> 1. **È un assioma?** Allora indica quale schema (A1–A11) e **quali formule sostituisci** alle metavariabili. Esempio: nella dimostrazione di T1, la riga 1 è A2 con $\beta:=\alpha\to\alpha$, $\gamma:=\alpha$.
> 2. **È ottenuta con R1?** Indica le due righe usate: una della forma $\delta$, l'altra $\delta\to\varphi$.
> 3. Se cita un teorema già dimostrato (es. T1), dillo: la sequenza diventa una dimostrazione vera inserendo la dimostrazione di T1.
>
> Allenati su T1 e T2 in [[01 - Il calcolo HK (sintassi)]], e sul Lemma 2(i).

### 2. "Costruisca una derivazione di $A\to(B\to C)$ da $B\to(A\to C)$ in ND"
> [!abstract] Cosa si valuta
> Sai costruire la derivazione e spiegare il procedimento (PL 126–135).

> [!success]- Soluzione
> L'assunzione $B\to(A\to C)$ resta **aperta** (è la premessa da cui si deriva).
> $$\dfrac{\dfrac{\dfrac{[A]^2\qquad\dfrac{[B]^1\quad B\to(A\to C)}{A\to C}\,{\scriptstyle\to E}}{C}\,{\scriptstyle\to E}}{B\to C}\,{\scriptstyle\to I,1}}{A\to(B\to C)}\,{\scriptstyle\to I,2}$$
> **Spiegazione**: devo arrivare a un'implicazione, quindi assumo l'antecedente $A$ (indice 2) e cerco $B\to C$. Di nuovo un'implicazione: assumo $B$ (indice 1) e cerco $C$. Da $B$ e dalla premessa, con $\to E$, ottengo $A\to C$; con $A$, ancora $\to E$, ottengo $C$. Infine chiudo $B$ ($\to I,1$) e poi $A$ ($\to I,2$).
> Dunque $B\to(A\to C)\vdash_{ND}A\to(B\to C)$. È l'esercizio aggiuntivo 2 senza l'ultima $\to I$ ([[17 - Esercizi aggiuntivi ND e PK|es. 2]]).

### 3. "Costruisca una dimostrazione di $\neg(A\land B)\Rightarrow\neg A\lor\neg B$ in PK"
> [!abstract] Cosa si valuta
> Sai costruire la dimostrazione e spiegare il procedimento (PL 138–145).

> [!success]- Soluzione (= De Morgan 2 del libro, senza l'ultimo $\to d$)
> 1. $A\Rightarrow A$ → is → $B,A\Rightarrow A$ → ss → $A,B\Rightarrow A$
> 2. $B\Rightarrow B$ → is → $A,B\Rightarrow B$
> 3. $A,B\Rightarrow A\land B$ — $\land d$
> 4. $B,A\Rightarrow A\land B$ — ss
> 5. $\neg(A\land B),B,A\Rightarrow$ — $\neg s$
> 6. $B,A,\neg(A\land B)\Rightarrow$ — ss\*
> 7. $A,\neg(A\land B)\Rightarrow\neg B$ — $\neg d$
> 8. $\neg(A\land B)\Rightarrow\neg B,\neg A$ — $\neg d$
> 9. $\neg(A\land B)\Rightarrow\neg A,\neg B$ — sd
> 10. $\neg(A\land B)\Rightarrow\neg A\lor\neg B$ — $\lor d$
>
> **Spiegazione "dal basso"**: a destra c'è una disgiunzione, e la virgola a destra vale "o", quindi con $\lor d$ basta ottenere $\neg A,\neg B$. Le negazioni a destra si spostano a sinistra ($\neg d$): basta $A,B,\neg(A\land B)\Rightarrow$. La negazione a sinistra si sposta a destra ($\neg s$): basta $A,B\Rightarrow A\land B$, che si chiude con $\land d$ e identici. Vedi [[15 - Calcolo dei sequenti Pk#3.12.1 Dimostrazioni in Pk|nota 15]].

### 4. "Quali sono le regole del calcolo PK?"
> [!abstract] Cosa si valuta
> Sai elencare le regole, spiegarne l'interpretazione intuitiva e usare bene i termini **formula principale**, **formula ausiliaria**, **contesto** (PL 140–142).

> [!success]- Traccia
> - **Assioma**: $p\Rightarrow p$ ($p$ variabile; gli identici per formule qualsiasi sono derivabili, [[16 - Dispensa Calcoli#1. Eliminazione dell'identità|Teor. 1]]).
> - **Strutturali**: scambio, contrazione, indebolimento (a sinistra e a destra), **cesura**. Agiscono sulla struttura della sequenza.
> - **Operazionali**: per ogni connettivo una regola **a sinistra** e una **a destra**. Sono solo regole di **introduzione**, a differenza di ND.
> - Terminologia: **contesti** = $\Gamma,\Delta,\Pi$; **principale** = la formula nuova nella conclusione; **ausiliarie** = le formule delle premesse da cui nasce la principale.
> - Interpretazioni: $\land s$ (a sinistra la virgola è "e"), $\lor d$ (a destra la virgola è "o"), $\to d$ (teorema di deduzione), $\neg$ (sposta la formula dall'altra parte).
> - Regole **forti** (operazionali + cesura) e **deboli** (le altre).
>
> Tabelle complete in [[15 - Calcolo dei sequenti Pk#Regole di Pk|nota 15]].

### 5. "Come si interpreta una sequenza in PK?"
> [!abstract] Cosa si valuta
> Sai spiegare l'interpretazione, sottolineare la **diversa lettura della virgola** a destra e a sinistra e usare **antecedente** e **conseguente** (PL 139–140).

> [!success]- Traccia
> $A_1,\dots,A_n\Rightarrow B_1,\dots,B_m$: dalla **congiunzione** delle formule dell'**antecedente** si deriva la **disgiunzione** delle formule del **conseguente**.
> - Virgola a sinistra = **"e"**; virgola a destra = **"o"**.
> - Formula associata: $\bigwedge\Gamma\to\bigvee\Delta$; la sequenza è **valida** se la formula associata è una tautologia (Def. 12.204).
> - Casi limite: $\Rightarrow A$ è valida sse $A$ è una tautologia; $\Gamma\Rightarrow$ (conseguente vuoto) dice che $\Gamma$ è contraddittorio; il sequente vuoto $\Rightarrow$ esprime l'assurdo.
> - $\Gamma$ e $\Delta$ sono **successioni** (ordine e ripetizioni contano), per questo esistono scambio e contrazione.

### 6. "Cos'è una derivazione in forma normale in ND?"
> [!abstract] Cosa si valuta
> Sai definirla, dare esempi di derivazioni normali e non normali e spiegarne l'importanza (PL 135–138).

> [!success]- Traccia
> - **Formula massimale**: conclusione di un'**introduzione** e, subito dopo, premessa **maggiore** di un'**eliminazione** dello stesso connettivo (deviazione inutile).
> - **Forma normale**: nessuna formula massimale.
> - Esempio **non normale** (Es. 11.195): $\to I$ costruisce $C\to A\lor B$ e subito $\to E$ la elimina. Versione **normale**: da $[A]^1$, $\lor I(a)$, poi due $\to I$.
> - **Teorema 11.196 (Prawitz, 1965)**: ogni derivazione si trasforma **in modo effettivo** in una in forma normale, con stessa conclusione e stesse assunzioni.
> - **Importanza**: niente ridondanze né loop, ricerca delle prove più controllabile, proprietà della sottoformula; tra le conseguenze, la consistenza del sistema.
>
> Vedi [[14 - Deduzione naturale ND#Ricerca delle prove e forma normale (pp. 135–138)|nota 14]].

### 7. "Dimostri l'equivalenza tra ND e PK"
> [!abstract] Cosa si valuta
> Sai gestire la **struttura induttiva** della dimostrazione e trasformare derivazioni ND in dimostrazioni PK e viceversa (Dispensa Calcoli).

> [!success]- Traccia
> Teorema 5 della dispensa: (1) $Y\vdash_{ND}\alpha$ ⇒ $\vdash_{PK}\mathbf{Y}\Rightarrow\alpha$; (2) $\vdash_{PK}\Sigma\Rightarrow\alpha$ ⇒ $\lvert\Sigma\rvert\vdash_{ND}\alpha$.
> - Prima definisci **successione**, **supporto**, **$Y$-successione** (Def. 2–4) e spiega a cosa servono.
> - Induzione sul **numero di vertici**.
> - Casi: $\land I$, $\land E(a)$ (con **cut**), $\to I$; e $\land s$, $\to d$.
>
> Tutto in [[16 - Dispensa Calcoli#3. Teorema 5 — Equivalenza tra PK e ND|nota 16]], con la sezione "come esporlo all'orale".

---

## 🧩 Altre domande probabili sui moduli 2–3

*(Non nell'elenco ufficiale, ma coerenti con il programma.)*

> [!question]- Enunci e dimostri il teorema di correttezza di HK
> [[03 - Teorema di correttezza]]

> [!question]- Enunci e dimostri il teorema di completezza di HK; cos'è il lemma di Lindenbaum?
> [[04 - Consistenza e insiemi consistenti massimali]], [[05 - Valutazione canonica e completezza]], schema in [[06 - Mappa della dimostrazione e checklist]]

> [!question]- Cos'è un insieme consistente? E consistente massimale?
> [[04 - Consistenza e insiemi consistenti massimali#Consistenza|Consistenza]] e [[04 - Consistenza e insiemi consistenti massimali#Insiemi consistenti massimali|massimalità]]

> [!question]- Differenza tra interpretazione e valutazione Booleana; cos'è la conseguenza logica?
> [[12 - Tavole di verità, valutazioni e conseguenza logica#2.7.2 Interpretazioni e valutazioni Booleane|nota 12]]

> [!question]- Cos'è il principio di induzione e perché è valido?
> [[10 - Nozioni preliminari e induzione#2. Il principio d'induzione matematica (§1.4, pp. 42–43)|nota 10]]

> [!question]- Perché si dice che $\alpha\Rightarrow\alpha$ non serve come assioma in PK?
> Teorema 1 (eliminazione dell'identità): [[16 - Dispensa Calcoli#1. Eliminazione dell'identità|nota 16]]

---

## Modulo 1 — *Ragionare nel quotidiano* (RNQ)

*Domande fuori dalle parti 2–3: risposte brevi, da integrare con il libro RNQ.*

> [!question]- Fare la tavola di verità di $\neg A\to(B\lor\neg A)$
> | $A$ | $B$ | $\neg A$ | $B\lor\neg A$ | $\neg A\to(B\lor\neg A)$ |
> |:-:|:-:|:-:|:-:|:-:|
> | 1 | 1 | 0 | 1 | **1** |
> | 1 | 0 | 0 | 0 | **1** |
> | 0 | 1 | 1 | 1 | **1** |
> | 0 | 0 | 1 | 1 | **1** |
>
> **Processo**: elenco le $2^2$ attribuzioni (procedura delle tavole, [[12 - Tavole di verità, valutazioni e conseguenza logica#Procedura delle tavole di verità|nota 12]]); calcolo prima $\neg A$, poi $B\lor\neg A$, infine il connettivo principale $\to$.
> **Risultato**: tautologia. Nelle righe 1–2 l'antecedente è falso; nelle righe 3–4 il conseguente contiene $\neg A$, che è vero.

> [!question]- "Se Parigi è in Italia, allora Parigi è in Europa. Quindi, non si dà il caso che Parigi sia in Italia e non sia in Europa": è formalmente valido?
> **Schema**: $P\to Q \;\therefore\; \neg(P\land\neg Q)$.
> Cerco una riga con premessa vera e conclusione falsa. La conclusione è falsa solo se $P\land\neg Q$ è vera, cioè $P=1$ e $Q=0$; ma lì la premessa $P\to Q$ vale 0. Nessun controesempio: lo schema è **valido**, quindi l'argomento è formalmente valido, anche se la premessa "Parigi è in Italia" è falsa.

> [!question]- "Se Anna esce senza l'ombrello, si bagna la testa. Anna non esce senza l'ombrello. Quindi non si bagna la testa." È una fallacia?
> Sì: è la **negazione dell'antecedente**. Schema $P\to Q,\ \neg P\ \therefore\ \neg Q$. Controesempio: $P=0$, $Q=1$ (le premesse sono vere, la conclusione falsa). Anna potrebbe bagnarsi per altre ragioni.

> [!question]- Altre domande RNQ
> - Cos'è un argomento valido? (RNQ 44)
> - Argomento invalido con premesse e conclusione vere (RNQ 47)
> - Un argomento valido può avere premesse false? (RNQ 46) → **sì**: la validità dice solo che *se* le premesse sono vere, lo è anche la conclusione. Esempio: l'argomento su Parigi qui sopra.
> - Argomento in senso stretto vs spiegazione (RNQ 37)
> - Problemi filosofici dell'implicazione classica (RNQ 19): verofunzionalità, **paradossi dell'implicazione materiale** (il falso implica tutto; il vero è implicato da tutto) → cfr. *ex absurdo quodlibet* in [[12 - Tavole di verità, valutazioni e conseguenza logica#2.7.1 Tavole di verità|nota 12]].
