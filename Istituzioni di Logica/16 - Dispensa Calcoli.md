---
tags: [logica, istituzioni-di-logica, modulo2, sequenti, ND, PK, equivalenza]
fonte: Dispensa "Calcoli" (Paoli, "Complementi di deduzione naturale e calcolo delle sequenze"), Teor. 1, Def. 2–4, Teor. 5
---
# Dispensa "Calcoli": eliminazione dell'identità ed equivalenza ND–PK
← [[15 - Calcolo dei sequenti Pk]] | → [[17 - Esercizi aggiuntivi ND e PK]]

> [!info] Cosa chiede il programma
> - **Teorema 1** → integra il §3.12 (sequenti)
> - **Definizioni 2, 3, 4 e Teorema 5** → sostituiscono il §3.13 del libro
>
> 🎯 È una **domanda d'esame** esplicita: *"Dimostri l'equivalenza tra ND e PK"* (vedi [[18 - Possibili domande d'esame]]). Si valuta se sai gestire la **struttura induttiva** della dimostrazione e trasformare derivazioni ND in dimostrazioni PK e viceversa.

Notazione della dispensa: formule $\alpha,\beta,\gamma\in FOR(\mathcal{L}_0)$, calcolo **PK** (= Pk del libro).

---

## 1. Eliminazione dell'identità

L'unico assioma di PK è $p\Rightarrow p$ con $p$ **variabile** (Def. 12.199). Il teorema dice che gli "identici" $\alpha\Rightarrow\alpha$ per formule **qualsiasi** non vanno postulati: si dimostrano.

> [!abstract] Teorema 1
> Per ogni $\alpha\in FOR(\mathcal{L}_0)$: $\quad\vdash_{PK}\alpha\Rightarrow\alpha$.

> [!success]- Dimostrazione (induzione sul numero $n$ di connettivi di $\alpha$)
> **Base** ($n=0$): $\alpha$ è una variabile $p$, e $p\Rightarrow p$ è un assioma.
>
> **Passo** ($n = m+1$). Ipotesi induttiva: $\vdash_{PK}\varphi\Rightarrow\varphi$ per ogni $\varphi$ con al massimo $m$ connettivi. Distinguo 4 casi secondo il connettivo principale di $\alpha$.
>
> **$\alpha = \neg\beta$** ($\beta$ ha $m$ connettivi; per IH c'è una dimostrazione $\mathcal{D}$ di $\beta\Rightarrow\beta$):
> $$\dfrac{\dfrac{\dfrac{\mathcal{D}}{\beta\Rightarrow\beta}}{\Rightarrow\beta,\neg\beta}\,{\scriptstyle\neg d}}{\neg\beta\Rightarrow\neg\beta}\,{\scriptstyle sd;\ \neg s}$$
>
> **$\alpha = \beta\land\gamma$** (IH: $\mathcal{D}_1$ di $\beta\Rightarrow\beta$, $\mathcal{D}_2$ di $\gamma\Rightarrow\gamma$):
> $$\dfrac{\dfrac{\dfrac{\beta\Rightarrow\beta}{\beta,\gamma\Rightarrow\beta}\,{\scriptstyle is;\ ss}\qquad\dfrac{\gamma\Rightarrow\gamma}{\beta,\gamma\Rightarrow\gamma}\,{\scriptstyle is}}{\beta,\gamma\Rightarrow\beta\land\gamma}\,{\scriptstyle\land d}}{\beta\land\gamma\Rightarrow\beta\land\gamma}\,{\scriptstyle\land s}$$
>
> **$\alpha = \beta\lor\gamma$**:
> $$\dfrac{\dfrac{\dfrac{\beta\Rightarrow\beta}{\beta\Rightarrow\beta,\gamma}\,{\scriptstyle id}\qquad\dfrac{\gamma\Rightarrow\gamma}{\gamma\Rightarrow\beta,\gamma}\,{\scriptstyle id;\ sd}}{\beta\lor\gamma\Rightarrow\beta,\gamma}\,{\scriptstyle\lor s}}{\beta\lor\gamma\Rightarrow\beta\lor\gamma}\,{\scriptstyle\lor d}$$
>
> **$\alpha = \beta\to\gamma$**:
> $$\dfrac{\dfrac{\dfrac{\beta\Rightarrow\beta}{\beta\Rightarrow\gamma,\beta}\,{\scriptstyle id;\ sd}\qquad\dfrac{\gamma\Rightarrow\gamma}{\gamma,\beta\Rightarrow\gamma}\,{\scriptstyle is;\ ss}}{\beta\to\gamma,\beta\Rightarrow\gamma}\,{\scriptstyle\to s}}{\beta\to\gamma\Rightarrow\beta\to\gamma}\,{\scriptstyle ss;\ \to d}$$ ∎

> [!tip] Conseguenze pratiche
> - Nelle dimostrazioni si può partire da $\alpha\Rightarrow\alpha$ per qualsiasi $\alpha$.
> - Con indebolimenti si ottiene anche $\Gamma,\alpha\Rightarrow\alpha$: è l'Es. 12.202 del libro, svolto in [[15 - Calcolo dei sequenti Pk#🧩 Esercizi|nota 15]] con lo stesso metodo.

---

## 2. Successioni, supporti, $Y$-successioni

Perché servono: in **ND** le assunzioni formano un **insieme**, in **PK** l'antecedente è una **successione** (ordine e ripetizioni contano). Per confrontare i due calcoli bisogna passare dall'una all'altro.

> [!info] Definizione 2 — Successione
> Sia $X\neq\emptyset$. Una **successione di lunghezza $n$** su $X$ è una funzione $\sigma$ con dominio $[1,\dots,n]$ (interi positivi da 1 a $n$) e codominio $X$. Una **successione finita** su $X$ è una successione di lunghezza $n$ per qualche $n$.
>
> *Esempio*: $X$ = lettere dell'alfabeto, $\sigma(1)=\sigma(2)=\sigma(5)=a$, $\sigma(3)=b$, $\sigma(4)=\sigma(6)=c$. Allora $\sigma$ ha lunghezza 6 e si scrive $\langle a,a,b,c,a,c\rangle$.

> [!info] Definizione 3 — Insieme supporto
> Se $\sigma$ è una successione di lunghezza $n$ su $X$, il suo **supporto** è
> $$\lvert\sigma\rvert = \{x\in X : x = \sigma(m) \text{ per qualche } m\le n\},$$
> cioè l'insieme degli elementi che vi compaiono, dimenticando ordine e ripetizioni.
>
> *Esempio*: $\sigma = \langle p,\ p\lor q,\ r,\ p\to q,\ p\lor q,\ p\lor q,\ r\rangle$ ha supporto $\lvert\sigma\rvert = \{p,\ p\to q,\ p\lor q,\ r\}$.

> [!info] Definizione 4 — $Y$-successione
> Sia $Y = \{a_1,\dots,a_n\}$ un sottoinsieme **finito** di $X$. Una **$Y$-successione** è una successione $\mathbf{Y}$ di lunghezza $n$ su $X$ tale che, per qualche **permutazione** $\pi$ di $[1,\dots,n]$,
> $$\mathbf{Y} = \langle a_{\pi(1)},\dots,a_{\pi(n)}\rangle.$$
> In pratica: gli elementi di $Y$ elencati **ciascuno una sola volta**, in un ordine qualsiasi.
>
> *Esempio*: $Y = \{p,\ p\to q,\ p\lor q,\ r\}$; $\ \mathbf{Y} = \langle p\to q,\ p,\ r,\ p\lor q\rangle$ è una $Y$-successione.

| Passaggio | Strumento |
|---|---|
| successione → insieme | **supporto** $\lvert\sigma\rvert$ |
| insieme finito → successione | una qualsiasi **$Y$-successione** |

---

## 3. Teorema 5 — Equivalenza tra PK e ND

> [!abstract] Teorema 5
> 1. Sia $Y\subseteq FOR(\mathcal{L}_0)$ finito e $\alpha\in FOR(\mathcal{L}_0)$. **Se $Y\vdash_{ND}\alpha$, allora $\vdash_{PK}\mathbf{Y}\Rightarrow\alpha$ per ogni $Y$-successione $\mathbf{Y}$.**
> 2. Sia $\Sigma$ una successione finita su $FOR(\mathcal{L}_0)$ e $\alpha\in FOR(\mathcal{L}_0)$. **Se $\vdash_{PK}\Sigma\Rightarrow\alpha$, allora $\lvert\Sigma\rvert\vdash_{ND}\alpha$.**
>
> Riguarda sequenze con **una sola formula nel conseguente**, che corrispondono alle derivazioni ND (una conclusione).

> [!tip] Schema comune delle due dimostrazioni
> - **Induzione sul numero $n$ di vertici** dell'albero (derivazione ND nella (1), dimostrazione PK nella (2)).
> - Il minimo è $n=1$, non 0: ogni albero ha almeno un vertice.
> - **Base**: l'unico vertice è insieme radice e foglia.
> - **Passo**: si guarda l'**ultima regola** applicata. Le sottoderivazioni hanno al massimo $m$ vertici, quindi vale l'ipotesi induttiva; poi si "traduce" la regola nell'altro calcolo.

### Parte (1): da ND a PK

> [!success]- Dimostrazione di (1)
> Sia $Y\vdash_{ND}\alpha$: esiste una derivazione ad albero $\mathcal{D}$ di $\alpha$ con assunzioni aperte in $Y$. Induzione sul numero $n$ di vertici di $\mathcal{D}$.
>
> **Base** ($n=1$). L'unico vertice è radice, quindi etichettato da $\alpha$, e foglia, quindi $\alpha$ è un'assunzione aperta in $Y$. Per il **Teorema 1** $\vdash_{PK}\alpha\Rightarrow\alpha$; con **is** e **ss** ricostruisco nell'antecedente una qualsiasi $Y$-successione: $\vdash_{PK}\mathbf{Y}\Rightarrow\alpha$.
>
> **Passo** ($n=m+1$). IH: se c'è una derivazione ND di $\varphi$ da $Z$ con al massimo $m$ vertici, allora $\vdash_{PK}\mathbf{Z}\Rightarrow\varphi$ per ogni $Z$-successione $\mathbf{Z}$. Se $\alpha$ è un'assunzione, si procede come nella base. Altrimenti è conclusione di una regola di ND: un caso per regola. La dispensa svolge $\land I$, $\land E(a)$, $\to I$.
>
> **Caso $\land I$**: $\alpha = \beta\land\gamma$ dalle premesse $\beta$ (sottoderivazione $\mathcal{D}_1$) e $\gamma$ ($\mathcal{D}_2$).
> - $\mathcal{D}_1,\mathcal{D}_2$ hanno al massimo $m$ vertici e assunzioni aperte $\subseteq Y$.
> - Per IH (ed eventualmente is, ss): $\vdash_{PK}\mathbf{Y}\Rightarrow\beta$ e $\vdash_{PK}\mathbf{Y}\Rightarrow\gamma$.
> - Con $\land d$: $\vdash_{PK}\mathbf{Y}\Rightarrow\beta\land\gamma$.
>
> **Caso $\land E(a)$**: $\alpha$ ottenuta dalla premessa $\alpha\land\beta$ (sottoderivazione $\mathcal{D}_1$, $m$ vertici, **stesse** assunzioni aperte di $\mathcal{D}$).
> - Per IH: $\vdash_{PK}\mathbf{Y}\Rightarrow\alpha\land\beta$.
> - Costruisco:
> $$\dfrac{\mathbf{Y}\Rightarrow\alpha\land\beta\qquad\dfrac{\alpha\Rightarrow\alpha}{\alpha\land\beta\Rightarrow\alpha}\,{\scriptstyle is;\ ss;\ \land s}}{\mathbf{Y}\Rightarrow\alpha}\,{\scriptstyle cut}$$
> - Qui serve la **cesura**: in PK non ci sono regole di eliminazione, e la cut fa da "ponte".
>
> **Caso $\to I$**: $\alpha = \beta\to\gamma$, da una derivazione $\mathcal{D}_1$ di $\gamma$ dall'assunzione $\beta$ (per semplicità tutte le occorrenze di $\beta$ sono chiuse da questa $\to I$).
> - $\mathcal{D}_1$ ha $m$ vertici e assunzioni aperte = quelle di $\mathcal{D}$ **più** $\beta$.
> - Per IH: $\vdash_{PK}\beta,\mathbf{Y}\Rightarrow\gamma$.
> - Con $\to d$: $\vdash_{PK}\mathbf{Y}\Rightarrow\beta\to\gamma$. ∎

> [!success]- 🧩 Altri casi della parte (1), non svolti nella dispensa
> **$\to E$**: $\alpha$ da $\beta$ e $\beta\to\alpha$. Per IH: $\mathbf{Y}\Rightarrow\beta$ e $\mathbf{Y}\Rightarrow\beta\to\alpha$.
> 1. $\mathbf{Y}\Rightarrow\alpha,\beta$ — id, sd (da $\mathbf{Y}\Rightarrow\beta$)
> 2. $\alpha,\mathbf{Y}\Rightarrow\alpha$ — Teor. 1 + is\*
> 3. $\beta\to\alpha,\mathbf{Y}\Rightarrow\alpha$ — $\to s$ (da 1 e 2)
> 4. $\mathbf{Y}\Rightarrow\alpha,\beta\to\alpha$ — id, sd (da $\mathbf{Y}\Rightarrow\beta\to\alpha$)
> 5. $\mathbf{Y}\Rightarrow\alpha$ — cut (da 4 e 3)
>
> **$\lor I(a)$**: $\alpha = \beta\lor\gamma$ da $\beta$. Per IH $\mathbf{Y}\Rightarrow\beta$; con id: $\mathbf{Y}\Rightarrow\beta,\gamma$; con $\lor d$: $\mathbf{Y}\Rightarrow\beta\lor\gamma$. ($\lor I(b)$ analogo, con sd.)
>
> **$\land E(b)$**: come $\land E(a)$, usando $\beta\Rightarrow\beta$ → is → $\alpha,\beta\Rightarrow\beta$ → $\land s$.
>
> **$\lor E$**: $\alpha$ da $\beta\lor\gamma$ e da derivazioni di $\alpha$ da $[\beta]$ e da $[\gamma]$. Per IH: $\mathbf{Y}\Rightarrow\beta\lor\gamma$, $\beta,\mathbf{Y}\Rightarrow\alpha$, $\gamma,\mathbf{Y}\Rightarrow\alpha$.
> 1. $\beta\lor\gamma,\mathbf{Y}\Rightarrow\alpha$ — $\lor s$
> 2. $\mathbf{Y}\Rightarrow\alpha,\beta\lor\gamma$ — id, sd
> 3. $\mathbf{Y}\Rightarrow\alpha$ — cut
>
> **EFQ / RAA** si trattano con le regole del $\neg$, sfruttando che a destra si possono avere più formule.

### Parte (2): da PK a ND

> [!success]- Dimostrazione di (2)
> Sia $\vdash_{PK}\Sigma\Rightarrow\alpha$: esiste una dimostrazione ad albero $\mathcal{D}$. Induzione sul numero $n$ di vertici.
>
> **Base** ($n=1$). L'unico vertice è radice, cioè $\Sigma\Rightarrow\alpha$, e foglia, cioè un **assioma**. Quindi $\Sigma$ consiste solo di $\alpha$, e $\lvert\Sigma\rvert = \{\alpha\}$. In ND c'è certamente una derivazione di $\alpha$ dall'assunzione aperta $\alpha$ (la derivazione di un solo nodo). Quindi $\lvert\Sigma\rvert\vdash_{ND}\alpha$.
>
> **Passo** ($n=m+1$). IH: se c'è una dimostrazione PK di $\Delta\Rightarrow\varphi$ con al massimo $m$ vertici, allora $\lvert\Delta\rvert\vdash_{ND}\varphi$. La radice $\Sigma\Rightarrow\alpha$ è conclusione di una regola di PK: un caso per regola. La dispensa svolge $\land s$ e $\to d$.
>
> **Caso $\land s$**: $\Sigma = \beta\land\gamma,\Pi$, ottenuta dalla premessa $\beta,\gamma,\Pi\Rightarrow\alpha$ (sottodimostrazione $\mathcal{D}_1$, $m$ vertici).
> - Per IH: $\lvert\beta,\gamma,\Pi\rvert\vdash_{ND}\alpha$, cioè una derivazione $\mathcal{D}^*$ di $\alpha$ dalle assunzioni aperte $\beta,\gamma,\Pi$.
> - Sostituisco ogni foglia $\beta$ con $\dfrac{\beta\land\gamma}{\beta}{\scriptstyle\land E(a)}$ e ogni foglia $\gamma$ con $\dfrac{\beta\land\gamma}{\gamma}{\scriptstyle\land E(b)}$, poi proseguo con $\mathcal{D}^*$ fino ad $\alpha$.
> - Ottengo una derivazione di $\alpha$ dalle assunzioni aperte $\beta\land\gamma,\Pi$: $\lvert\Sigma\rvert\vdash_{ND}\alpha$.
>
> **Caso $\to d$**: $\alpha = \beta\to\gamma$, la radice $\Sigma\Rightarrow\beta\to\gamma$ ottenuta dalla premessa $\beta,\Sigma\Rightarrow\gamma$.
> - Per IH: $\lvert\beta,\Sigma\rvert\vdash_{ND}\gamma$, cioè una derivazione $\mathcal{D}^*$ di $\gamma$ dalle assunzioni aperte $\beta,\Sigma$.
> - Applico $\to I$, chiudendo **tutte** le occorrenze aperte di $\beta$:
> $$\dfrac{\begin{matrix}[\beta]\\ \mathcal{D}^*\\ \gamma\end{matrix}}{\beta\to\gamma}\,{\scriptstyle\to I}$$
> - Quindi $\lvert\Sigma\rvert\vdash_{ND}\beta\to\gamma$. ∎

> [!success]- 🧩 Altri casi della parte (2), non svolti nella dispensa
> **Regole strutturali a sinistra** (is, ss, cs): il supporto dell'antecedente o **resta uguale** (ss, cs) o **cresce** (is). La stessa derivazione ND fornita dall'IH va bene, perché le sue assunzioni aperte sono ancora tra quelle ammesse.
>
> **$\land d$** (con conseguente $\beta\land\gamma$, premesse $\Sigma\Rightarrow\beta$ e $\Sigma\Rightarrow\gamma$): per IH due derivazioni ND di $\beta$ e $\gamma$ da $\lvert\Sigma\rvert$; si uniscono con $\land I$.
>
> **$\to s$** ($\Sigma = \beta\to\gamma,\Pi$, premesse $\Pi\Rightarrow\beta$ e $\gamma,\Pi\Rightarrow\alpha$): per IH $\lvert\Pi\rvert\vdash\beta$ e $\lvert\gamma,\Pi\rvert\vdash\alpha$. Nella seconda derivazione sostituisco ogni foglia $\gamma$ con
> $$\dfrac{\begin{matrix}\lvert\Pi\rvert\\ \vdots\\ \beta\end{matrix}\qquad\beta\to\gamma}{\gamma}\,{\scriptstyle\to E}$$
> ottenendo $\lvert\beta\to\gamma,\Pi\rvert\vdash_{ND}\alpha$.
>
> **cut** ($\Sigma\Rightarrow\varphi$ e $\varphi,\Sigma\Rightarrow\alpha$): per IH $\lvert\Sigma\rvert\vdash\varphi$ e $\lvert\varphi,\Sigma\rvert\vdash\alpha$. Sostituisco le foglie $\varphi$ della seconda con la prima derivazione. La cut corrisponde alla **composizione** di derivazioni.
>
> Le regole che producono conseguenti con più formule ($\lor d$, $\neg d$, id, …) richiedono una versione generalizzata dell'enunciato (conseguente letto come disgiunzione) e non sono trattate dalla dispensa.

> [!tip] Come esporlo all'orale
> 1. Enuncia il teorema e spiega **perché** servono supporti e $Y$-successioni (insiemi vs successioni).
> 2. Dichiara l'induzione: sul **numero di vertici**, minimo 1.
> 3. Base: usa il **Teorema 1** nella (1), l'assioma nella (2).
> 4. Svolgi i casi della dispensa: $\land I$, $\land E(a)$ (con la **cut**!), $\to I$ nella (1); $\land s$, $\to d$ nella (2).
> 5. Fai notare le corrispondenze: **introduzione ND ↔ regola destra PK**; **eliminazione ND ↔ regola sinistra PK (+ cut)**; **$\to I$ ↔ $\to d$**.
