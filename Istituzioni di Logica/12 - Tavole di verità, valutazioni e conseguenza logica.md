---
tags: [logica, istituzioni-di-logica, modulo2, semantica, tavole-di-verità]
fonte: "Percorsi di logica", §2.7 (pp. 65–80) + inizio §2.8
---
# Tavole di verità, interpretazioni e valutazioni (§2.7)
← [[11 - Il linguaggio della logica proposizionale]] | → [[13 - Il calcolo à la Hilbert H (libro)]]

> [!tip] Collegamento
> È la stessa semantica della [[02 - Semantica e conseguenza logica|nota 02]] (dispensa). Lì si chiama "valutazione", qui "valutazione **Booleana**"; lì $\alpha,\beta$, qui $A, B$.

## 2.7.1 Tavole di verità

- Valori di verità: $1$ (vero) e $0$ (falso), **mutuamente inversi**, con $0 < 1$. (Il concetto di valore di verità risale a Frege, 1891–92.)
- **Principio di composizionalità**: il valore di una formula è **funzione** dei valori delle sue sottoformule immediate, e quindi è univocamente determinato dai valori delle sue variabili.
- **Bivalenza**: ogni formula, data un'attribuzione di valori alle variabili, ha **un** valore in $\{1,0\}$.
- **Non contraddizione**: ne ha **uno solo**.

> [!info] Definizione 7.94 — Tavole di verità dei connettivi
> | $A$ | $B$ | $\neg A$ | $A\land B$ | $A\lor B$ | $A\to B$ |
> |:-:|:-:|:-:|:-:|:-:|:-:|
> | 1 | 1 | 0 | 1 | 1 | 1 |
> | 1 | 0 | 0 | 0 | 1 | 0 |
> | 0 | 1 | 1 | 0 | 1 | 1 |
> | 0 | 0 | 1 | 0 | 0 | 1 |

- $\neg A$ **inverte** il valore di $A$.
- $A\land B$ prende il **più piccolo** dei due valori; $A\lor B$ il **più grande**.
- $A\to B$ è falsa **sse** $A$ è vera e $B$ è falsa. Equivalentemente: è vera sse il valore dell'antecedente è $\le$ quello del conseguente.
  - antecedente falso ⇒ implicazione vera: ***ex absurdo quodlibet*** (un enunciato contraddittorio implica qualsiasi enunciato);
  - conseguente vero ⇒ implicazione vera, qualunque sia l'antecedente.

### Procedura delle tavole di verità
Scrivo $A(p_1,\dots,p_n)$ se $A$ contiene **esattamente** le variabili $p_1,\dots,p_n$.
1. **Passo 1**: scrivo su una riga le variabili $p_1,\dots,p_n$ e poi la formula; creo una colonna per ogni variabile e per ogni connettivo.
2. **Passo 2**: nella colonna di $p_i$ ($1\le i\le n$) scrivo, dall'alto in basso e alternandoli, $2^{n-i}$ occorrenze di 1 seguite da $2^{n-i}$ occorrenze di 0, fino a $2^n$ righe.
3. **Passo 3**: ho tutte le attribuzioni possibili; con la Def. 7.94 calcolo i valori delle sottoformule partendo da quelle di **complessità minore**. I valori di $A$ sono nella colonna del **connettivo principale**.

> [!example] Esempio 7.95 — $(p_1\to p_2)\lor(p_2\to p_1)$
> | $p_1$ | $p_2$ | $p_1\to p_2$ | $p_2\to p_1$ | $\lor$ (principale) |
> |:-:|:-:|:-:|:-:|:-:|
> | 1 | 1 | 1 | 1 | **1** |
> | 1 | 0 | 0 | 1 | **1** |
> | 0 | 1 | 1 | 0 | **1** |
> | 0 | 0 | 1 | 1 | **1** |
>
> La colonna del connettivo principale contiene solo 1: la formula è sempre vera.

> [!question]- Esercizio 7.96 — Le attribuzioni a $n$ variabili sono $2^n$
> Associo a ogni attribuzione $f:\{p_1,\dots,p_n\}\to\{0,1\}$ l'insieme $X_f = \{p_i : f(p_i) = 1\}$. È una **biiezione** tra attribuzioni e $\mathcal{P}(\{p_1,\dots,p_n\})$: un'attribuzione è determinata dalle variabili che rende vere, e ogni sottoinsieme individua un'attribuzione. Poiché $\lvert\mathcal{P}(X)\rvert = 2^n$ (Es. 4.59), le attribuzioni sono $2^n$.

### Classificazione delle formule

> [!info] Definizione 7.97
> $A$ è:
> - **tautologia** se nella colonna del connettivo principale c'è **solo 1**;
> - **soddisfacibile** se c'è **almeno un 1**;
> - **falsificabile** se c'è **almeno uno 0**;
> - **contraddizione** se c'è **solo 0**.
>
> $Taut$ = insieme delle tautologie.

Le categorie non si escludono: una tautologia è anche soddisfacibile; una formula "contingente" è sia soddisfacibile che falsificabile.

> [!question]- Esercizio 7.98 — Classificare (soluzioni)
> Metodo rapido: cerco un'attribuzione che renda falsa la formula; se porta a contraddizione, è una tautologia.
> 1. $(p\lor\neg q)\land(q\land\neg q)$ → **contraddizione** ($q\land\neg q$ è sempre 0).
> 2. $(p\to p)\to(q\to q)$ → **tautologia** (il conseguente è sempre 1).
> 3. $p\land(\neg p\lor q)\to q$ → **tautologia**: per renderla falsa serve $q=0$ e $p\land(\neg p\lor q)=1$, cioè $p=1$ e $\neg p\lor q=1$; ma $\neg p=0$ e $q=0$. Assurdo.
> 4. $(p\to\neg q)\to(q\to\neg p)$ → **tautologia**: falsa richiede $q=1$, $\neg p=0$ (cioè $p=1$) e $p\to\neg q=1$; ma $p=1$, $\neg q=0$. Assurdo.
> 5. $(p\to q\land(q\to r))\to(p\to r)$ (si legge $(p\to(q\land(q\to r)))\to(p\to r)$) → **tautologia**: falsa richiede $p=1$, $r=0$ e $p\to q\land(q\to r)=1$, quindi $q=1$ e $q\to r=1$, cioè $r=1$. Assurdo.
> 6. $p\to(q\lor r\to p)$ → **tautologia** (se $p=1$ il conseguente è vero).
> 7. $p\land(q\lor r)\to(p\land q)\lor(p\land r)$ → **tautologia** (distributività).
> 8. $p$ → **soddisfacibile e falsificabile** (né tautologia né contraddizione).

---

## 2.7.2 Interpretazioni e valutazioni Booleane

> [!info] Definizione 7.99
> 1. Un'**interpretazione** è una funzione arbitraria $i: Var\to\{1,0\}$.
> 2. Una **valutazione Booleana** è una funzione $v: For\to\{1,0\}$ tale che:
>    - $v(\neg A) = 1$ sse $v(A) = 0$;
>    - $v(A\land B) = 1$ sse $v(A) = v(B) = 1$;
>    - $v(A\lor B) = 1$ sse $v(A) = 1$ oppure $v(B) = 1$;
>    - $v(A\to B) = 1$ sse $v(A) = 0$ oppure $v(B) = 1$.

L'interpretazione assegna valori **solo alle variabili**; la valutazione a **tutte le formule**, rispettando le clausole.

> [!info] Osservazione 7.100 — Formulazione aritmetica equivalente
> - $v(\neg A) = 1 - v(A)$
> - $v(A\land B) = \min(v(A), v(B))$
> - $v(A\lor B) = \max(v(A), v(B))$
> - $v(A\to B) = \max(1 - v(A), v(B))$
>
> Ad esempio $x\to y = 0$ sse $x=1$ e $y=0$ sse $\max(1-1, 0) = 0$.

> [!question]- Esercizio 7.101 — L'Oss. 7.100 equivale alla Def. 7.99
> Basta confrontare clausola per clausola, per $x,y\in\{0,1\}$:
> - $1-x = 1$ sse $x=0$;
> - $\min(x,y)=1$ sse $x=y=1$;
> - $\max(x,y) = 1$ sse $x=1$ o $y=1$;
> - $\max(1-x, y) = 1$ sse $1-x=1$ o $y=1$, cioè $x=0$ o $y=1$.

> [!question]- Esercizio 7.102 — $v(A\to B) = 1$ sse $v(A)\le v(B)$
> $v(A\to B)=0$ sse $v(A)=1$ e $v(B)=0$ sse $v(A) > v(B)$ (l'unico caso con $x>y$ in $\{0,1\}$ è $x=1, y=0$). Negando: $v(A\to B)=1$ sse $v(A)\le v(B)$.

> [!info] Terminologia
> - $v(A)$ è il **valore di verità** di $A$ in $v$: $A$ è **vera** in $v$ se $v(A)=1$, **falsa** se $v(A)=0$.
> - $v_1, v_2$ **concordano su** $A$ se $v_1(A)=v_2(A)$; **su** $\Gamma$ se concordano su ogni $A\in\Gamma$.
> - $v$ **estende** $i$ se $v(p) = i(p)$ per ogni $p\in Var$.

> [!note] Interpretazioni e valutazioni si corrispondono
> - Ogni interpretazione si estende a **una e una sola** valutazione Booleana. *(La dimostrazione, Lemma 7.103 ed Es. 7.104, è **fuori programma**; serve solo il fatto.)*
> - Ogni valutazione determina un'unica interpretazione, la sua restrizione $v\restriction Var$.
> - Quindi interpretazioni e valutazioni sono in **corrispondenza biunivoca**: spesso non si distinguono.

> [!question]- Esercizio 7.105 — $A$ è tautologia (contraddizione) sse $v(A)=1$ ($0$) per ogni valutazione $v$
> Le righe della tavola di $A$ sono le attribuzioni di valori alle variabili di $A$. Ogni valutazione $v$ produce una riga (la sua restrizione alle variabili di $A$), e ogni riga si estende a una valutazione. Il valore di $A$ in $v$ è quello nella colonna del principale, nella riga corrispondente. Quindi "solo 1 nella colonna" equivale a "$v(A)=1$ per ogni $v$" (analogo per 0).

---

## Conseguenza ed equivalenza logica

> [!info] Definizione 7.106 — Conseguenza logica
> Sia $\Gamma\cup\{B\}\subseteq For$. $B$ è **conseguenza logica** di $\Gamma$ ($\Gamma\models B$) se, **per ogni** valutazione $v$, se $v(A)=1$ per ogni $A\in\Gamma$, allora $v(B)=1$.

- La **logica proposizionale classica** è la relazione $\models$ tra insiemi di formule e formule.
- $\models$ formalizza l'**inferenza valida** del ragionamento deduttivo: la verità delle premesse determina **necessariamente** la verità della conclusione.
- È più "forte" del legame premesse–conclusione negli argomenti **induttivi** (es. "$x_1,\dots,x_n$ hanno $P$, quindi ogni $x$ ha $P$"): lì le premesse rendono la conclusione solo **probabile**.

> [!info] Definizione 7.107 — Equivalenza logica
> $A$ e $B$ sono **logicamente equivalenti** se $A\models B$ e $B\models A$.

> [!info] Convenzione 7.108 — Notazioni
> - $\Gamma, A_1,\dots,A_n\models B$ per $\Gamma\cup\{A_1,\dots,A_n\}\models B$;
> - $\models A$ per $\emptyset\models A$;
> - $A\simeq B$ per "$A\models B$ e $B\models A$".
>
> $A$ è una tautologia sse $\models A$.

> [!caution] Fuori programma
> **Teorema 7.109** (equivalenza tra $\Gamma, A_1..A_n\models A$ e le forme con $\land$ e $\to$) e **Teorema 7.110** ($A\simeq B$ sse $v(A)=v(B)$ per ogni $v$ sse $\models A\leftrightarrow B$). Si usa però la definizione del bicondizionale:
> $$A\leftrightarrow B \;=\; (A\to B)\land(B\to A).$$

> [!question]- Esercizio 7.111 — Associatività, commutatività
> 1. $A\land(B\land C)\simeq(A\land B)\land C$: per ogni $v$, entrambe valgono $\min(v(A),v(B),v(C))$.
> 2. $A\land B\simeq B\land A$: $\min$ è commutativo.
> 3. Generalizzazione a $n$ congiunti per induzione su $n$, usando 1.
>
> Stesso ragionamento per $\lor$ con $\max$. Da qui le notazioni $A\land B\land C$, $\bigwedge_{i=1}^n A_i$, $\bigvee_{i=1}^n A_i$.

### Definibilità dei connettivi

> [!info] Esercizio 7.112 — Definizione
> Un connettivo binario $C$ è **definibile** se esistono connettivi $C_1,\dots,C_n$ e una formula in due variabili $p, q$, contenente solo $C_1,\dots,C_n$, **equivalente** a $pCq$.

> [!example] Esempio del libro: $\land$ definibile con $\{\neg,\lor\}$
> $$v(A\land B)=1 \iff v(A)=v(B)=1 \iff v(\neg A)=v(\neg B)=0 \iff v(\neg A\lor\neg B)=0 \iff v(\neg(\neg A\lor\neg B))=1.$$
> Poiché $v$ è arbitraria: $A\land B\simeq\neg(\neg A\lor\neg B)$.

> [!question]- Esercizio 7.112 — Soluzioni (i)–(v)
> | | Definizione | Verifica (per ogni $v$) |
> |---|---|---|
> | (i) $\lor$ con $\{\neg,\land\}$ | $A\lor B\simeq\neg(\neg A\land\neg B)$ | $=0$ sse $\neg A\land\neg B=1$ sse $A=B=0$ |
> | (ii) $\to$ con $\{\neg,\land\}$ | $A\to B\simeq\neg(A\land\neg B)$ | $=0$ sse $A=1$ e $B=0$ |
> | (iii) $\to$ con $\{\neg,\lor\}$ | $A\to B\simeq\neg A\lor B$ | $=0$ sse $\neg A=0$ e $B=0$ sse $A=1, B=0$ |
> | (iv) $\land$ con $\{\neg,\to\}$ | $A\land B\simeq\neg(A\to\neg B)$ | $=1$ sse $A\to\neg B=0$ sse $A=1$ e $B=1$ |
> | (v) $\lor$ con $\{\neg,\to\}$ | $A\lor B\simeq\neg A\to B$ | $=1$ sse $\neg A=0$ o $B=1$ sse $A=1$ o $B=1$ |
>
> Conseguenza: bastano $\{\neg,\to\}$, ed è il linguaggio del calcolo H del libro ([[13 - Il calcolo à la Hilbert H (libro)|nota 13]]).

> [!caution] Fuori programma
> **Def. 7.113** (insieme di verità / saturato), **Lemma 7.114**, **Es. 7.115**.
> Curiosità: le condizioni (S1)–(S4) sono le stesse del [[04 - Consistenza e insiemi consistenti massimali#Lemma 9 — Lemma delle clausole omofoniche|Lemma 9]] della dispensa.

---

## Inizio §2.8 — Tableaux semantici (p. 80)

Per vedere che $A$ è una tautologia basta mostrare che **non esiste** una valutazione $v$ con $v(A)=0$. Spesso si fa più in fretta **per assurdo**.

> [!example] Esempio 8.116 — $A = p\to(q\to p)$
> 1. Suppongo che il principale ($\to$) valga $0$.
> 2. Un'implicazione è falsa sse antecedente $=1$ e conseguente $=0$: quindi $p=1$ e $q\to p=0$.
> 3. Allora, di nuovo, $q=1$ e $p=0$.
> 4. Ma $p$ ha già valore 1: un atomo deve avere **lo stesso valore in tutte le sue occorrenze**. Contraddizione ⚡.
>
> Non ci sono altri casi: $A$ non è falsificabile, quindi è una tautologia. Questo metodo è meno "dispendioso" delle tavole, ma se un connettivo non è determinato univocamente bisogna considerare più casi (Es. 8.117).
