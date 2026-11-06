---
tags:
  - logica
  - istituzioni-di-logica
  - modulo3
  - modulo2
  - HK
  - sintassi
fonte: Dispensa "Completezza 2.0" (Paoli), §1.1
---
# Il calcolo HK — sintassi
← [[00 - Indice]] | → [[02 - Semantica e conseguenza logica]]

> [!tip] Obiettivo generale
> Costruire un calcolo assiomatico **alla Hilbert**, HK, che generi **tutte e sole le tautologie**. Lo si otterrà come corollario di un risultato più forte: **derivabilità in HK = conseguenza logica** ([[05 - Valutazione canonica e completezza#Teorema di adeguatezza|teorema di adeguatezza]]).

Un calcolo proposizionale alla Hilbert ha due componenti:
1. un **linguaggio proposizionale**;
2. un insieme di **postulati** (assiomi + regole di inferenza).

---

## 1. Il linguaggio $\mathcal{L}_0$

> [!info] Definizione — Linguaggio $\mathcal{L}_0$
> - **Simboli descrittivi**: un insieme *numerabile* di variabili $\{p_1, p_2, \dots\}$
> - **Simboli logici**: connettivo unario $\neg$ (negazione); connettivi binari $\land$ (congiunzione), $\lor$ (disgiunzione), $\to$ (implicazione)
> - **Simboli ausiliari**: parentesi $($ e $)$

> [!info] Definizione induttiva — Formule $FOR(\mathcal{L}_0)$
> 1. ogni variabile $p_i$ è una formula;
> 2. se $\alpha, \beta$ sono formule, allora $\neg\alpha$, $\alpha \land \beta$, $\alpha \lor \beta$, $\alpha \to \beta$ sono formule;
> 3. nient'altro è una formula.

> [!warning] Convenzione sulle parentesi
> $\land$ e $\lor$ legano **più fortemente** di $\to$.
> Quindi $\alpha \land \beta \to \gamma$ si legge $(\alpha \land \beta) \to \gamma$, **non** $\alpha \land (\beta \to \gamma)$.

> [!note] Osservazione utile
> $FOR(\mathcal{L}_0)$ è un insieme **numerabile** (le formule sono stringhe finite su un alfabeto numerabile). Questo fatto sarà essenziale nel [[04 - Consistenza e insiemi consistenti massimali#Lemma 7 — Lemma di Lindenbaum|Lemma di Lindenbaum]].

---

## 2. Postulati: assiomi e regole

- **Assiomi**: formule definite come tali, date in forma **schematica**. Dire che $\neg\neg\alpha \to \alpha$ è un assioma significa che *ogni* formula ottenuta sostituendo alla **metavariabile** $\alpha$ una qualsiasi formula di $FOR(\mathcal{L}_0)$ è un assioma.
- **Regole di inferenza**: insiemi di $n+1$ formule $\{\alpha_1, \dots, \alpha_n, \alpha_{n+1}\}$, dove $\alpha_1,\dots,\alpha_n$ sono le **premesse** e $\alpha_{n+1}$ la **conclusione**. Notazione:
$$\frac{\alpha_1, \dots, \alpha_n}{\alpha_{n+1}}$$
  Lettura intuitiva: dalle assunzioni $\alpha_1,\dots,\alpha_n$ si può concludere $\alpha_{n+1}$. Anche le regole sono schematiche.

### Postulati di HK

> [!abstract] Assiomi e regola di HK
> | | Schema | Connettivo |
> |---|---|---|
> | **A1** | $\alpha \to (\beta \to \alpha)$ | $\to$ |
> | **A2** | $(\alpha \to (\beta \to \gamma)) \to ((\alpha \to \beta) \to (\alpha \to \gamma))$ | $\to$ |
> | **A3** | $\alpha \land \beta \to \alpha$ | $\land$ |
> | **A4** | $\alpha \land \beta \to \beta$ | $\land$ |
> | **A5** | $(\alpha \to \beta) \to ((\alpha \to \gamma) \to (\alpha \to \beta \land \gamma))$ | $\land$ |
> | **A6** | $\alpha \to \alpha \lor \beta$ | $\lor$ |
> | **A7** | $\beta \to \alpha \lor \beta$ | $\lor$ |
> | **A8** | $(\alpha \to \gamma) \to ((\beta \to \gamma) \to (\alpha \lor \beta \to \gamma))$ | $\lor$ |
> | **A9** | $(\alpha \to \beta) \to ((\alpha \to \neg\beta) \to \neg\alpha)$ | $\neg$ |
> | **A10** | $\alpha \to (\neg\alpha \to \beta)$ | $\neg$ |
> | **A11** | $\neg\neg\alpha \to \alpha$ | $\neg$ |
>
> **R1 (Modus Ponens)**: $$\frac{\alpha \qquad \alpha \to \beta}{\beta}$$

> [!tip] Come ricordarli
> - A1–A2: implicazione (A1 = "indebolimento", A2 = "distribuzione").
> - A3–A5: congiunzione (eliminazione sx, eliminazione dx, introduzione).
> - A6–A8: disgiunzione (introduzione sx, introduzione dx, eliminazione "per casi").
> - A9: riduzione all'assurdo; A10: *ex falso quodlibet*; A11: doppia negazione (rende la logica **classica**).

---

## 3. Dimostrazioni e teoremi

> [!info] Definizione — Dimostrazione in HK
> Una successione **finita** di formule $\alpha_1, \dots, \alpha_n \in FOR(\mathcal{L}_0)$ tale che, per ogni $i \le n$, $\alpha_i$:
> - è un assioma di HK, **oppure**
> - è ottenuta da formule precedenti nella successione applicando l'(unica) regola R1.

> [!info] Definizione — Teorema di HK
> Un **teorema** (formula dimostrabile) di HK è la formula conclusiva di una qualche dimostrazione in HK. Si scrive $\vdash_{HK} \alpha$.
> In particolare **ogni assioma è un teorema** (dimostrazione banale di lunghezza 1).

### Esempio — T1: principio di identità $\vdash_{HK} \alpha \to \alpha$

> [!example]- Dimostrazione di T1
> | # | Formula | Giustificazione |
> |---|---|---|
> | 1 | $(\alpha \to ((\alpha \to \alpha) \to \alpha)) \to ((\alpha \to (\alpha \to \alpha)) \to (\alpha \to \alpha))$ | A2 con $\beta := \alpha\to\alpha$, $\gamma := \alpha$ |
> | 2 | $\alpha \to ((\alpha \to \alpha) \to \alpha)$ | A1 con $\beta := \alpha \to \alpha$ |
> | 3 | $(\alpha \to (\alpha \to \alpha)) \to (\alpha \to \alpha)$ | R1, 1, 2 |
> | 4 | $\alpha \to (\alpha \to \alpha)$ | A1 con $\beta := \alpha$ |
> | 5 | $\alpha \to \alpha$ | R1, 3, 4 |

> [!note] Uso di teoremi già dimostrati
> Una volta dimostrato un teorema, lo si può citare per accorciare le dimostrazioni. In senso stretto il risultato non è una dimostrazione, ma lo diventa **inserendo** al suo posto la dimostrazione del teorema citato.

### Esempio — T2: legge di assorbimento (contrazione) $\vdash_{HK} (\alpha \to (\alpha \to \beta)) \to (\alpha \to \beta)$

Abbreviamo $\varphi := \alpha \to (\alpha \to \beta)$.

> [!example]- Dimostrazione di T2
> | # | Formula | Giustificazione |
> |---|---|---|
> | 1 | $\varphi \to ((\alpha \to \alpha) \to (\alpha \to \beta))$ | A2 (con $\beta := \alpha$, $\gamma := \beta$) |
> | 2 | $\alpha \to \alpha$ | T1 |
> | 3 | $(\alpha \to \alpha) \to (\varphi \to (\alpha \to \alpha))$ | A1 |
> | 4 | $\varphi \to (\alpha \to \alpha)$ | R1, 2, 3 |
> | 5 | $[\varphi \to ((\alpha \to \alpha) \to (\alpha \to \beta))] \to [(\varphi \to (\alpha \to \alpha)) \to (\varphi \to (\alpha \to \beta))]$ | A2 |
> | 6 | $(\varphi \to (\alpha \to \alpha)) \to (\varphi \to (\alpha \to \beta))$ | R1, 1, 5 |
> | 7 | $\varphi \to (\alpha \to \beta)$ | R1, 4, 6 |

---

## 4. Derivazioni da assunzioni

> [!info] Definizione — Derivazione di $\alpha$ da $\Gamma$
> Una successione **finita** di formule $\alpha_1, \dots, \alpha_n$ (con $\alpha_n = \alpha$) tale che ogni $\alpha_i$ è:
> - un assioma di HK, **oppure**
> - un elemento di $\Gamma$ (un'**assunzione**), **oppure**
> - ottenuta da formule precedenti mediante R1.
>
> Si scrive $\Gamma \vdash_{HK} \alpha$: "$\alpha$ è derivabile in HK da $\Gamma$".

- Unica differenza con le dimostrazioni: i passi possono essere giustificati come assunzioni. Le derivazioni formalizzano il **ragionamento *ex hypothesi***.
- Una dimostrazione è una derivazione dall'**insieme vuoto**: $\vdash_{HK} \alpha$ equivale a $\emptyset \vdash_{HK} \alpha$.

> [!example] Esempio: $\{\alpha \to \beta,\ \beta \to \gamma,\ \alpha\} \vdash_{HK} \gamma$
> | # | Formula | Giustificazione |
> |---|---|---|
> | 1 | $\alpha \to \beta$ | assunzione |
> | 2 | $\beta \to \gamma$ | assunzione |
> | 3 | $\alpha$ | assunzione |
> | 4 | $\beta$ | R1, 1, 3 |
> | 5 | $\gamma$ | R1, 2, 4 |

> [!note] 🧩 Proprietà ovvie ma usate di continuo (implicite nella dispensa)
> 1. **Monotonia**: se $\Gamma \subseteq \Delta$ e $\Gamma \vdash \alpha$, allora $\Delta \vdash \alpha$ (la stessa derivazione va bene).
> 2. **Finitezza**: se $\Gamma \vdash \alpha$, allora esiste $\Gamma_0 \subseteq \Gamma$ **finito** con $\Gamma_0 \vdash \alpha$ (una derivazione è finita, quindi usa solo un numero finito di assunzioni).
> 3. **Segmenti iniziali**: ogni segmento iniziale di una derivazione è a sua volta una derivazione.
> 4. **Teoremi come passi**: se $\vdash \beta$ allora $\beta$ può comparire in qualunque derivazione.
> 5. **Unione di derivazioni**: se $\Gamma \vdash \beta$ e $\Gamma \vdash \gamma$, mettendo una derivazione dopo l'altra otteniamo una derivazione da $\Gamma$ che contiene sia $\beta$ sia $\gamma$.

---

## 5. Teorema di deduzione

> [!abstract] Teorema 1 — Teorema di deduzione
> $$\Gamma \cup \{\alpha\} \vdash_{HK} \beta \quad \text{se e solo se} \quad \Gamma \vdash_{HK} \alpha \to \beta$$
> *(Nella dispensa è enunciato **senza dimostrazione**.)*

> [!success]- 🧩 Approfondimento: dimostrazione (non richiesta dalla dispensa)
> **($\Leftarrow$)** Prendo una derivazione di $\alpha \to \beta$ da $\Gamma$, aggiungo $\alpha$ come assunzione e applico R1: ottengo $\beta$ da $\Gamma \cup \{\alpha\}$.
>
> **($\Rightarrow$)** Sia $\beta_1, \dots, \beta_n = \beta$ una derivazione da $\Gamma \cup \{\alpha\}$. Mostro per induzione (forte) su $i$ che $\Gamma \vdash \alpha \to \beta_i$.
> - $\beta_i$ assioma o $\beta_i \in \Gamma$: allora $\Gamma \vdash \beta_i$; con A1 $\beta_i \to (\alpha \to \beta_i)$ e R1, $\Gamma \vdash \alpha \to \beta_i$.
> - $\beta_i = \alpha$: $\alpha \to \alpha$ è T1.
> - $\beta_i$ ottenuta per R1 da $\beta_j$ e $\beta_j \to \beta_i$ ($j,k<i$): per ipotesi induttiva $\Gamma \vdash \alpha \to \beta_j$ e $\Gamma \vdash \alpha \to (\beta_j \to \beta_i)$. Con A2 $(\alpha \to (\beta_j \to \beta_i)) \to ((\alpha \to \beta_j) \to (\alpha \to \beta_i))$ e due applicazioni di R1: $\Gamma \vdash \alpha \to \beta_i$. ∎

> [!tip] Perché è utile
> Per mostrare $\vdash \alpha \to \beta$ basta derivare $\beta$ **assumendo** $\alpha$: molto più comodo che costruire la dimostrazione assiomatica diretta.

---

## 6. Lemma 2

> [!abstract] Lemma 2 (i)
> $$\vdash_{HK} \alpha \to (\beta \to \alpha \land \beta)$$
> *(Nella dispensa è solo enunciato; viene usato nel [[04 - Consistenza e insiemi consistenti massimali#Lemma 9 — Lemma delle clausole omofoniche|Lemma 9]], caso $\land$.)*

> [!success]- 🧩 Dimostrazione (via teorema di deduzione)
> Mostro $\{\alpha, \beta\} \vdash \alpha \land \beta$:
> | # | Formula | Giustificazione |
> |---|---|---|
> | 1 | $\alpha$ | assunzione |
> | 2 | $\beta$ | assunzione |
> | 3 | $\alpha \to (\beta \to \alpha)$ | A1 |
> | 4 | $\beta \to \alpha$ | R1, 1, 3 |
> | 5 | $\beta \to \beta$ | T1 |
> | 6 | $(\beta \to \alpha) \to ((\beta \to \beta) \to (\beta \to \alpha \land \beta))$ | A5 (con $\alpha := \beta$, $\beta := \alpha$, $\gamma := \beta$) |
> | 7 | $(\beta \to \beta) \to (\beta \to \alpha \land \beta)$ | R1, 4, 6 |
> | 8 | $\beta \to \alpha \land \beta$ | R1, 5, 7 |
> | 9 | $\alpha \land \beta$ | R1, 2, 8 |
>
> Dal Teorema di deduzione due volte: $\{\alpha\} \vdash \beta \to \alpha \land \beta$, quindi $\vdash \alpha \to (\beta \to \alpha \land \beta)$. ∎
