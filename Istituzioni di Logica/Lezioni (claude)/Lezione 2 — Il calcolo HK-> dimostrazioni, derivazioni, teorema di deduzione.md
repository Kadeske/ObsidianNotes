## tags: [istituzioni-di-logica, modulo-2, modulo-3, lezione-2] fonti: Completezza 2.0 §1.1; Programma 25/26; Possibili domande d'esame 24/25

# Lezione 2 — Il calcolo HK: dimostrazioni, derivazioni, teorema di deduzione

> [!info] Dove siamo nel programma
> 
> - Programma: il §3.10 di _Percorsi di logica_ va sostituito con **Completezza 2.0, §1.1**. L'intera dispensa è poi il programma del **modulo 3**.
> - HK è quindi il calcolo su cui poggia tutto il modulo 3: correttezza e completezza si dimostrano **per HK**.
> 
> Prerequisiti dalla Lezione 1: metavariabili, connettivo principale, $\Gamma\vDash\alpha$, induzione sulla lunghezza.

> [!tip] Stime di probabilità ★★★ alta · ★★ media · ★ bassa. Sono mie stime, basate sull'elenco _Possibili domande 24/25_ e su quanto ogni argomento serve nel modulo 3.

---

## 1. Che cos'è un calcolo alla Hilbert

> [!warning] Esame — ★★ Serve per inquadrare le domande su HK. La cosa da saper dire bene è la **differenza con ND e PK**: in HK quasi tutto il lavoro lo fanno gli **assiomi**, e c'è **una sola regola**.

Un calcolo proposizionale alla Hilbert consta di due componenti:

1. un **linguaggio** proposizionale (per HK: $\mathcal{L}_0$, Lezione 1);
2. un insieme di **postulati**, divisi in:
    - **assiomi**: formule assunte come tali;
    - **regole di inferenza**: insiemi di $n+1$ formule ${\alpha_1,\dots,\alpha_n,\alpha_{n+1}}$, con $\alpha_1,\dots,\alpha_n$ _premesse_ e $\alpha_{n+1}$ _conclusione_. Si leggono così: dalle assunzioni $\alpha_1,\dots,\alpha_n$ si può concludere $\alpha_{n+1}$.

**Assiomi e regole sono dati schematicamente.** Dire che $\neg\neg\alpha\to\alpha$ è un assioma significa che è un assioma **ogni** formula ottenuta sostituendo alla metavariabile $\alpha$ una qualsiasi formula di FOR. Ogni schema ha quindi infinite **istanze**.

---

## 2. I postulati di HK

> [!warning] Esame — ★★★ Non serve recitarli a memoria in ordine con il numero. Serve però **riconoscerli** dentro una dimostrazione già fatta (domanda in elenco) e sapere **dove si usano** nella completezza (modulo 3). Consiglio: impara bene A1, A2, A3, A4, A9, A10, A11 e Lemma 2, che sono quelli usati nelle dimostrazioni della dispensa.


| Schema  | Lettura intuitiva                                                      |                                                       |
| ------- | ---------------------------------------------------------------------- | ----------------------------------------------------- |
| **A1**  | $\alpha\to(\beta\to\alpha)$                                            | ciò che è vero resta vero sotto qualsiasi ipotesi     |
| **A2**  | $(\alpha\to(\beta\to\gamma))\to((\alpha\to\beta)\to(\alpha\to\gamma))$ | distribuzione di $\to$ (modus ponens "sotto ipotesi") |
| **A3**  | $\alpha\land\beta\to\alpha$                                            | eliminazione di $\land$ (sinistra)                    |
| **A4**  | $\alpha\land\beta\to\beta$                                             | eliminazione di $\land$ (destra)                      |
| **A5**  | $(\alpha\to\beta)\to((\alpha\to\gamma)\to(\alpha\to\beta\land\gamma))$ | introduzione di $\land$ sotto ipotesi                 |
| **A6**  | $\alpha\to\alpha\lor\beta$                                             | introduzione di $\lor$ (sinistra)                     |
| **A7**  | $\beta\to\alpha\lor\beta$                                              | introduzione di $\lor$ (destra)                       |
| **A8**  | $(\alpha\to\gamma)\to((\beta\to\gamma)\to(\alpha\lor\beta\to\gamma))$  | ragionamento per casi                                 |
| **A9**  | $(\alpha\to\beta)\to((\alpha\to\neg\beta)\to\neg\alpha)$               | riduzione all'assurdo                                 |
| **A10** | $\alpha\to(\neg\alpha\to\beta)$                                        | da una contraddizione segue tutto (_ex falso_)        |
| **A11** | $\neg\neg\alpha\to\alpha$                                              | doppia negazione (è il principio "classico")          |
| **R1**  | $\dfrac{\alpha\qquad\alpha\to\beta}{\beta}$                            | modus ponens: unica regola                            |

Ricorda la convenzione: $\land,\lor$ legano più di $\to$. Quindi A3 è $(\alpha\land\beta)\to\alpha$ e A6 è $\alpha\to(\alpha\lor\beta)$.

> [!note] Perché proprio questi assiomi? Ogni assioma è una **tautologia** (lo hai verificato in parte nella Lezione 1, Es. 4). R1 **preserva la verità**: se $v(\alpha)=1$ e $v(\alpha\to\beta)=1$, allora $v(\beta)=1$. Queste due osservazioni, insieme, sono la dimostrazione della **correttezza** (Lezione 6).

---

## 3. Dimostrazione, teorema, derivazione

> [!warning] Esame — ★★★ Sono le definizioni che dai per prime quando ti presentano una dimostrazione in HK. La **differenza tra dimostrazione e derivazione** è una classica domanda di contorno.

**Dimostrazione in HK.** Successione **finita** di formule $\alpha_1,\dots,\alpha_n$ di FOR tale che, **per ogni** $i\le n$, $\alpha_i$:

- è un **assioma** di HK, oppure
- è ottenuta da formule **precedenti** nella successione applicando R1.

**Teorema di HK.** Formula conclusiva di una qualche dimostrazione. Si scrive $\vdash_{HK}\alpha$. Ogni assioma è un teorema: basta la dimostrazione di un solo passo, l'assioma stesso.

**Derivazione di $\alpha$ da $\Gamma$ in HK.** Successione finita $\alpha_1,\dots,\alpha_n$ con $\alpha_n=\alpha$ tale che, per ogni $i\le n$, $\alpha_i$:

- è un assioma, oppure
- **appartiene a $\Gamma$** (è un'_assunzione_), oppure
- è ottenuta da formule precedenti con R1.

Si scrive $\Gamma\vdash_{HK}\alpha$: "$\alpha$ è derivabile da $\Gamma$".

> [!important] Dimostrazione vs derivazione L'unica differenza è che in una derivazione puoi giustificare un passo anche come **assunzione** presa da $\Gamma$. Le derivazioni formalizzano il ragionamento _ex hypothesi_. Una dimostrazione è una derivazione dall'insieme vuoto: $\vdash_{HK}\alpha$ è lo stesso che $\emptyset\vdash_{HK}\alpha$. Confronta con la Lezione 1: $\vDash\alpha$ era $\emptyset\vDash\alpha$. È lo stesso parallelo, sul lato sintattico.

**Uso di teoremi già dimostrati.** Una volta dimostrato un teorema, lo si può citare come giustificazione di un passo, per esempio "T1". In senso stretto la successione così ottenuta **non è una dimostrazione**, perché quel passo non è né un assioma né ottenuto con R1. Però si trasforma in una dimostrazione vera **inserendo** al suo posto la dimostrazione del teorema citato. ★★: è un'osservazione che la dispensa fa esplicitamente.

---

## 4. Come si legge una dimostrazione già fatta

> [!warning] Esame — ★★★ (domanda esplicita in elenco) _"Le presento la seguente dimostrazione (già fatta) nel calcolo HK: mi spieghi cosa viene fatto in ciascun passaggio."_ Risultato atteso: per ogni passaggio sai dire se è un **assioma** (quale) o un'applicazione di **modus ponens** (a quali righe), e sai indicare **l'istanza di sostituzione** che produce la formula.

**Procedura per ogni riga**

1. È un'implicazione della forma di uno schema? Allora prova a farla combaciare con uno schema: trova cosa si sostituisce ad $\alpha,\beta,\gamma$.
2. Altrimenti cerca due righe precedenti della forma $\delta$ e $\delta\to\text{(riga)}$: è R1.
3. In una derivazione, la riga può anche essere un'assunzione.

### Esempio svolto: il principio di identità T1 $;\vdash_{HK}\alpha\to\alpha$

|#|Formula|Giustificazione|Istanza|
|---|---|---|---|
|1|$(\alpha\to((\alpha\to\alpha)\to\alpha))\to((\alpha\to(\alpha\to\alpha))\to(\alpha\to\alpha))$|A2|$\alpha:=\alpha,\ \beta:=\alpha\to\alpha,\ \gamma:=\alpha$|
|2|$\alpha\to((\alpha\to\alpha)\to\alpha)$|A1|$\alpha:=\alpha,\ \beta:=\alpha\to\alpha$|
|3|$(\alpha\to(\alpha\to\alpha))\to(\alpha\to\alpha)$|R1 da 2 e 1|2 è l'antecedente di 1|
|4|$\alpha\to(\alpha\to\alpha)$|A1|$\alpha:=\alpha,\ \beta:=\alpha$|
|5|$\alpha\to\alpha$|R1 da 4 e 3|4 è l'antecedente di 3|

Come raccontarla a voce: _"L'idea è usare A2 per 'distribuire'. Scelgo l'istanza di A2 in cui $\beta$ è $\alpha\to\alpha$ e $\gamma$ è $\alpha$: l'antecedente di quest'istanza è un'istanza di A1, quindi lo stacco con modus ponens. Ottengo un'implicazione il cui antecedente è di nuovo un'istanza di A1, con $\beta:=\alpha$, e con un secondo modus ponens arrivo ad $\alpha\to\alpha$."_

> [!tip] Il trucco delle istanze Nelle istanze le metavariabili dello schema possono essere sostituite da formule **complesse** ($\beta:=\alpha\to\alpha$) e **anche dalla stessa formula** ($\alpha:=\alpha,\beta:=\alpha$). È proprio qui che gli studenti si bloccano all'orale.

### Esempio svolto: una derivazione

${\alpha\to\beta,\ \beta\to\gamma,\ \alpha}\vdash_{HK}\gamma$

|#|Formula|Giustificazione|
|---|---|---|
|1|$\alpha\to\beta$|assunzione|
|2|$\beta\to\gamma$|assunzione|
|3|$\alpha$|assunzione|
|4|$\beta$|R1 da 3 e 1|
|5|$\gamma$|R1 da 4 e 2|

---

## 5. Il teorema di deduzione

> [!warning] Esame — enunciato e uso ★★★ · dimostrazione ★ La dispensa lo **enuncia senza dimostrarlo**, quindi difficilmente ti chiederanno la dimostrazione. Però è usato nel **Lemma 6** (completezza) e rende brevi quasi tutte le derivazioni. Devi saperlo enunciare con esattezza e saperlo applicare.

**Teorema 1 (di deduzione).** $\Gamma\cup{\alpha}\vdash_{HK}\beta$ **se e solo se** $\Gamma\vdash_{HK}\alpha\to\beta$.

Lettura: per dimostrare un'implicazione $\alpha\to\beta$ basta **assumere** $\alpha$ e derivare $\beta$. È ciò che si fa nella matematica informale. In ND questo diventa direttamente una regola ($\to I$); in HK è un **metateorema**, cioè un risultato _sul_ calcolo e non una regola _del_ calcolo.

Confronto con la Lezione 1, Esercizio 8: $\Gamma\cup{\alpha}\vDash\beta$ sse $\Gamma\vDash\alpha\to\beta$. Il teorema di deduzione ne è il **gemello sintattico**.

> [!note]- Facoltativo: idea della dimostrazione (★, utile per capire A1 e A2) **($\Leftarrow$)** Facile: da una derivazione di $\alpha\to\beta$ da $\Gamma$ aggiungo l'assunzione $\alpha$ e applico R1. (Vedi Esercizio 6.) **($\Rightarrow$)** Per induzione sulla lunghezza della derivazione di $\beta$ da $\Gamma\cup{\alpha}$; si mostra che per **ogni** riga $\delta$ vale $\Gamma\vdash\alpha\to\delta$.
> 
> - $\delta$ è un assioma o sta in $\Gamma$: allora $\delta$, $\delta\to(\alpha\to\delta)$ (A1), $\alpha\to\delta$ (R1).
> - $\delta=\alpha$: allora $\alpha\to\alpha$ è T1.
> - $\delta$ è ottenuta con R1 da $\varepsilon$ e $\varepsilon\to\delta$: per i.i. $\Gamma\vdash\alpha\to\varepsilon$ e $\Gamma\vdash\alpha\to(\varepsilon\to\delta)$. Con A2, $(\alpha\to(\varepsilon\to\delta))\to((\alpha\to\varepsilon)\to(\alpha\to\delta))$, e due R1 danno $\alpha\to\delta$.
> 
> Morale: **A1, A2 e T1 sono esattamente ciò che serve** per il teorema di deduzione. Ecco perché sono i primi due assiomi.

---

## 6. Lemma 2 e mappa degli assiomi nel modulo 3

> [!warning] Esame — ★★★ per il modulo 3 Quando esporrai la completezza ti chiederanno "e qui perché si può fare?". La risposta è quasi sempre "per l'assioma Ax". Questa tabella ti prepara a quelle domande.

**Lemma 2 (i).** $\vdash_{HK}\alpha\to(\beta\to\alpha\land\beta)$. La dispensa lo enuncia; lo dimostri tu nell'Esercizio 4.

|Assioma / risultato|Dove è usato nella dispensa _Completezza 2.0_|
|---|---|
|A1|Lemma 9(4): se $\beta\in\Delta$ allora $\alpha\to\beta\in\Delta$|
|A3, A4|Lemma 9(2): da $\alpha\land\beta\in\Delta$ a $\alpha,\beta\in\Delta$|
|Lemma 2(i)|Lemma 9(2): da $\alpha,\beta\in\Delta$ a $\alpha\land\beta\in\Delta$|
|A9, A11 + Teorema di deduzione|Lemma 6(i): se $\Gamma\cup{\neg\alpha}$ è inconsistente allora $\Gamma\vdash\alpha$|
|A10|Lemma 4 (da $\beta$ e $\neg\beta$ si deriva tutto) e Lemma 9(4)|
|R1|passo induttivo del Teorema 3 (correttezza)|

---

## 7. Esercizi

> [!example] Esercizio 1 — Riconoscere istanze ★★★ Per ciascuna formula, di' se è un'istanza di un assioma di HK; se sì, di quale e con quale sostituzione. a) $(p\to q)\to(r\to(p\to q))$ b) $p\land(q\lor r)\to q\lor r$ c) $p\to(\neg p\to(q\land r))$ d) $p\to(q\to q)$ e) $\neg\neg(p\to q)\to(p\to q)$ f) $p\lor q\to p$

> [!success]- Soluzione a) A1 con $\alpha:=p\to q$, $\beta:=r$. b) A4 con $\alpha:=p$, $\beta:=q\lor r$. c) A10 con $\alpha:=p$, $\beta:=q\land r$. d) **Non** è un'istanza di A1: A1 richiederebbe $p\to(q\to p)$, con lo stesso $p$ in fondo. Non è istanza di nessuno schema. È però un **teorema** (Esercizio 3). e) A11 con $\alpha:=p\to q$. f) Non è istanza di nessuno schema, e **non è nemmeno un teorema**: non è una tautologia ($p=0,q=1$), e per la correttezza i teoremi di HK sono tautologie.

> [!example] Esercizio 2 — Spiega la dimostrazione ★★★ (formato d'esame) Ecco una derivazione di $\alpha\to\gamma$ da ${\alpha\to\beta,\ \beta\to\gamma}$, **senza** giustificazioni. Giustifica ogni passo e indica le istanze.
> 
> 1. $\beta\to\gamma$
> 2. $(\beta\to\gamma)\to(\alpha\to(\beta\to\gamma))$
> 3. $\alpha\to(\beta\to\gamma)$
> 4. $(\alpha\to(\beta\to\gamma))\to((\alpha\to\beta)\to(\alpha\to\gamma))$
> 5. $(\alpha\to\beta)\to(\alpha\to\gamma)$
> 6. $\alpha\to\beta$
> 7. $\alpha\to\gamma$

> [!success]- Soluzione
> 
> 8. Assunzione.
> 9. A1 con $\alpha:=\beta\to\gamma$, $\beta:=\alpha$.
> 10. R1 da 1 e 2.
> 11. A2 con $\alpha:=\alpha$, $\beta:=\beta$, $\gamma:=\gamma$.
> 12. R1 da 3 e 4.
> 13. Assunzione.
> 14. R1 da 6 e 5.
> 
> Commento a voce: _"È il sillogismo ipotetico. Con A1 'metto sotto ipotesi $\alpha$' la seconda premessa, poi A2 mi permette di distribuire l'ipotesi $\alpha$ e di usare la prima premessa."_ Nota che **non** si usa il teorema di deduzione: è una derivazione in senso stretto.

> [!example] Esercizio 3 — Una dimostrazione breve ★★ Dimostra in HK: $\vdash_{HK}\alpha\to(\beta\to\beta)$. Puoi usare T1.

> [!success]- Soluzione
> 
> 15. $\beta\to\beta$ — T1 (con $\alpha:=\beta$)
> 16. $(\beta\to\beta)\to(\alpha\to(\beta\to\beta))$ — A1 con $\alpha:=\beta\to\beta$, $\beta:=\alpha$
> 17. $\alpha\to(\beta\to\beta)$ — R1 da 1 e 2
> 
> Per avere una dimostrazione in senso stretto, sostituisci la riga 1 con i cinque passi di T1 (con $\beta$ al posto di $\alpha$).

> [!example] Esercizio 4 — Lemma 2(i) ★★★ a) Mostra che ${\alpha,\beta}\vdash_{HK}\alpha\land\beta$. _Suggerimento: usa A5 con $\beta$ al posto della sua prima metavariabile._ b) Ricavane $\vdash_{HK}\alpha\to(\beta\to\alpha\land\beta)$.

> [!success]- Soluzione a)
> 
> 18. $\alpha$ — assunzione
> 19. $\alpha\to(\beta\to\alpha)$ — A1
> 20. $\beta\to\alpha$ — R1 da 1 e 2
> 21. $\beta\to\beta$ — T1
> 22. $(\beta\to\alpha)\to((\beta\to\beta)\to(\beta\to\alpha\land\beta))$ — A5 con $\alpha:=\beta$, $\beta:=\alpha$, $\gamma:=\beta$
> 23. $(\beta\to\beta)\to(\beta\to\alpha\land\beta)$ — R1 da 3 e 5
> 24. $\beta\to\alpha\land\beta$ — R1 da 4 e 6
> 25. $\beta$ — assunzione
> 26. $\alpha\land\beta$ — R1 da 8 e 7
> 
> b) Da (a), ${\alpha}\cup{\beta}\vdash\alpha\land\beta$. Per il teorema di deduzione ${\alpha}\vdash\beta\to\alpha\land\beta$. Applicandolo ancora, $\vdash\alpha\to(\beta\to\alpha\land\beta)$.

> [!example] Esercizio 5 — Usare A3 e A4 ★★ Mostra che ${\alpha\land\beta}\vdash_{HK}\beta\land\alpha$.

> [!success]- Soluzione
> 
> 27. $\alpha\land\beta$ — assunzione
> 28. $\alpha\land\beta\to\alpha$ — A3
> 29. $\alpha$ — R1 da 1 e 2
> 30. $\alpha\land\beta\to\beta$ — A4
> 31. $\beta$ — R1 da 1 e 4
> 32. $\beta\to(\alpha\to\beta\land\alpha)$ — Lemma 2(i), con $\alpha:=\beta$, $\beta:=\alpha$
> 33. $\alpha\to\beta\land\alpha$ — R1 da 5 e 6
> 34. $\beta\land\alpha$ — R1 da 3 e 7
> 
> È lo stesso schema del Lemma 9(2) della dispensa: A3 e A4 per "smontare" la congiunzione, Lemma 2 per "rimontarla".

> [!example] Esercizio 6 — Metà facile del teorema di deduzione ★★ Dimostra: se $\Gamma\vdash_{HK}\alpha\to\beta$, allora $\Gamma\cup{\alpha}\vdash_{HK}\beta$.

> [!success]- Soluzione Sia $D$ una derivazione di $\alpha\to\beta$ da $\Gamma$. Le sue righe sono assiomi, elementi di $\Gamma$ o ottenute con R1, quindi $D$ è anche una derivazione da $\Gamma\cup{\alpha}$. Prolungala così:
> 
> - $\alpha$ — assunzione (ora $\alpha\in\Gamma\cup{\alpha}$)
> - $\beta$ — R1 da $\alpha$ e $\alpha\to\beta$
> 
> Questa è una derivazione di $\beta$ da $\Gamma\cup{\alpha}$. La stessa idea ("unisco derivazioni e prolungo") è quella dei Lemmi 4 e 6 della dispensa.

> [!example] Esercizio 7 — Applicare il teorema di deduzione ★★★ Usando l'Esercizio 2, mostra che $\vdash_{HK}(\alpha\to\beta)\to((\beta\to\gamma)\to(\alpha\to\gamma))$.

> [!success]- Soluzione Dall'Es. 2: ${\alpha\to\beta}\cup{\beta\to\gamma}\vdash\alpha\to\gamma$. Teorema di deduzione (con $\Gamma={\alpha\to\beta}$): ${\alpha\to\beta}\vdash(\beta\to\gamma)\to(\alpha\to\gamma)$. Di nuovo (con $\Gamma=\emptyset$): $\vdash(\alpha\to\beta)\to((\beta\to\gamma)\to(\alpha\to\gamma))$. Attenzione all'**ordine**: si "scarica" per prima l'ultima ipotesi che vuoi trovare più all'interno.

> [!example] Esercizio 8 — Un'altra derivazione ★★ Mostra che ${\alpha\to(\beta\to\gamma),\ \beta}\vdash_{HK}\alpha\to\gamma$. Puoi usare il teorema di deduzione.

> [!success]- Soluzione Basta mostrare ${\alpha\to(\beta\to\gamma),\ \beta,\ \alpha}\vdash\gamma$ e poi applicare il teorema di deduzione su $\alpha$:
> 
> 1. $\alpha\to(\beta\to\gamma)$ — assunzione
> 2. $\alpha$ — assunzione
> 3. $\beta\to\gamma$ — R1 da 2 e 1
> 4. $\beta$ — assunzione
> 5. $\gamma$ — R1 da 4 e 3
> 
> Da qui, per deduzione: ${\alpha\to(\beta\to\gamma),\beta}\vdash\alpha\to\gamma$. Applicandolo ancora due volte ottieni $\vdash(\alpha\to(\beta\to\gamma))\to(\beta\to(\alpha\to\gamma))$, cioè la versione HK di un esercizio di ND che è in elenco d'esame.

> [!example] Esercizio 9 — Domande concettuali ★★ a) Perché in HK non ci sono regole specifiche per $\land,\lor,\neg$? b) Perché per controllare che "gli assiomi sono tautologie" basta controllare 11 schemi, se gli assiomi sono infiniti? c) Che differenza c'è tra R1 e il teorema di deduzione?

> [!success]- Soluzione a) In HK il comportamento dei connettivi è codificato **negli assiomi** (A3–A11); l'unica regola, il modus ponens, serve a "staccare" conseguenti. È la scelta opposta a ND, dove ogni connettivo ha regole di introduzione ed eliminazione e non ci sono assiomi. b) Perché il ragionamento con cui si mostra che uno schema è tautologico usa solo le clausole delle valutazioni sui componenti, e vale quindi per **qualsiasi** formula sostituita alle metavariabili (Lezione 1, Es. 4). c) R1 è una **regola del calcolo**: compare nelle derivazioni. Il teorema di deduzione è un **metateorema**: un'affermazione sull'esistenza di derivazioni. Non è mai una riga di una derivazione in HK.

---

## 8. Riepilogo per l'esame

|Argomento|Prob.|Cosa devi saper fare|
|---|:-:|---|
|Spiegare passo per passo una dimostrazione già fatta|★★★|per ogni riga: assioma + istanza, oppure R1 + righe|
|Definizioni di dimostrazione, teorema, derivazione|★★★|darle con "per ogni $i\le n$" e le tre opzioni|
|Differenza dimostrazione / derivazione|★★★|assunzioni da $\Gamma$; dimostrazione = derivazione da $\emptyset$|
|Teorema di deduzione: enunciato e uso|★★★|enunciarlo come "sse"; usarlo per accorciare|
|Lemma 2(i) e uso di A3/A4|★★★ (mod. 3)|ricostruire Lemma 9(2)|
|Mappa "assioma → lemma della completezza"|★★★ (mod. 3)|rispondere a "perché qui si può fare?"|
|Assiomi schematici, istanze|★★|spiegare perché sono infiniti e come si sostituisce|
|Uso di teoremi già dimostrati (T1, T2)|★★|spiegare perché la successione resta trasformabile in dimostrazione|
|Dimostrazione del teorema di deduzione|★|facoltativo: idea con A1, A2, T1|

> [!todo] Prima della lezione 3
> 
> - Rifai l'Esercizio 2 e la dimostrazione di T1 **giustificando a voce** ogni passo.
> - Prova a giustificare da solo i 7 passi di T2 (contrazione) nella dispensa, indicando per ciascuno l'istanza.
> - Memorizza la tabella del §6.