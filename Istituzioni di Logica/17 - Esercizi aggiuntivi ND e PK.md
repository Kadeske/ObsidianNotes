---
tags: [logica, istituzioni-di-logica, modulo2, esercizi, ND, PK]
fonte: "Esercizi aggiuntivi" (11 esercizi, da svolgere sia in ND sia in PK)
---
# Esercizi aggiuntivi — soluzioni in ND e in PK
← [[16 - Dispensa Calcoli]] | → [[18 - Possibili domande d'esame]]

> [!info] Come usare questa nota
> Ogni esercizio va dimostrato due volte:
> - in **ND**, come $\vdash_{ND}\varphi$ ([[14 - Deduzione naturale ND|regole]]);
> - in **PK**, come $\vdash_{PK}\ \Rightarrow\varphi$ ([[15 - Calcolo dei sequenti Pk|regole]]).
>
> Prova da solo, poi apri la soluzione. All'esame conta soprattutto saper **spiegare il processo**.

> [!tip] Metodo
> **ND** — lavora "a ritroso":
> 1. Se la formula è un'implicazione $A\to B$: **assumi** $A$ (con un indice) e cerca $B$; alla fine $\to I$.
> 2. Se hai un'assunzione disgiuntiva $A\lor B$: **ragiona per casi** con $\lor E$.
> 3. Usa le eliminazioni ($\land E$, $\to E$) sulle assunzioni e le introduzioni ($\land I$, $\lor I$) per costruire la conclusione.
> 4. Una $\to I$ può chiudere un'assunzione **vacuamente** ($\emptyset$) se l'antecedente non serve.
>
> **PK** — parti dal basso:
> 1. Con $\to d$ porta gli antecedenti delle implicazioni **a sinistra** del $\Rightarrow$.
> 2. "Smonta" le formule a sinistra ($\land s$, $\lor s$, $\to s$) e a destra ($\land d$, $\lor d$).
> 3. Chiudi con identici $\alpha\Rightarrow\alpha$ (Teorema 1) + indebolimenti e scambi.

> [!abstract] Lemma MP (usato spesso): $\vdash_{PK}\beta\to\gamma,\beta\Rightarrow\gamma$
> 1. $\beta\Rightarrow\beta$ → id → $\beta\Rightarrow\beta,\gamma$ → sd → $\beta\Rightarrow\gamma,\beta$
> 2. $\gamma\Rightarrow\gamma$ → is → $\beta,\gamma\Rightarrow\gamma$ → ss → $\gamma,\beta\Rightarrow\gamma$
> 3. $\beta\to\gamma,\beta\Rightarrow\gamma$ — $\to s$ (da 1 e 2, con $\Gamma=\beta$, $\Delta=\gamma$)
>
> Notazione: "is\*", "ss\*" = indebolimenti / scambi ripetuti.

---

## 1. $(\alpha\to\beta)\to((\alpha\to\gamma)\to(\beta\to(\alpha\to\gamma)))$

> [!question]- ND
> $$\dfrac{\dfrac{\dfrac{[\alpha\to\gamma]^2}{\beta\to(\alpha\to\gamma)}\,{\scriptstyle\to I,\ \emptyset}}{(\alpha\to\gamma)\to(\beta\to(\alpha\to\gamma))}\,{\scriptstyle\to I,2}}{(\alpha\to\beta)\to((\alpha\to\gamma)\to(\beta\to(\alpha\to\gamma)))}\,{\scriptstyle\to I,\ \emptyset}$$
> Due chiusure vacue: $\beta$ e $\alpha\to\beta$ non servono.

> [!question]- PK
> 1. $\alpha\to\gamma\Rightarrow\alpha\to\gamma$ — Teor. 1
> 2. $\beta,\alpha\to\gamma\Rightarrow\alpha\to\gamma$ — is
> 3. $\alpha\to\gamma\Rightarrow\beta\to(\alpha\to\gamma)$ — $\to d$
> 4. $\Rightarrow(\alpha\to\gamma)\to(\beta\to(\alpha\to\gamma))$ — $\to d$
> 5. $\alpha\to\beta\Rightarrow(\alpha\to\gamma)\to(\beta\to(\alpha\to\gamma))$ — is
> 6. $\Rightarrow(\alpha\to\beta)\to((\alpha\to\gamma)\to(\beta\to(\alpha\to\gamma)))$ — $\to d$

## 2. $(\alpha\to(\beta\to\gamma))\to(\beta\to(\alpha\to\gamma))$ — scambio delle premesse

> [!question]- ND
> $$\dfrac{\dfrac{\dfrac{\dfrac{\dfrac{[\alpha]^1\quad[\alpha\to(\beta\to\gamma)]^3}{\beta\to\gamma}\,{\scriptstyle\to E}\qquad[\beta]^2}{\gamma}\,{\scriptstyle\to E}}{\alpha\to\gamma}\,{\scriptstyle\to I,1}}{\beta\to(\alpha\to\gamma)}\,{\scriptstyle\to I,2}}{(\alpha\to(\beta\to\gamma))\to(\beta\to(\alpha\to\gamma))}\,{\scriptstyle\to I,3}$$

> [!question]- PK
> 1. $\alpha,\beta\Rightarrow\gamma,\alpha$ — da $\alpha\Rightarrow\alpha$: is, ss, id, sd
> 2. $\beta\to\gamma,\beta\Rightarrow\gamma$ — Lemma MP
> 3. $\beta\to\gamma,\alpha,\beta\Rightarrow\gamma$ — is, ss
> 4. $\alpha\to(\beta\to\gamma),\alpha,\beta\Rightarrow\gamma$ — $\to s$ (da 1 e 3; $\Gamma=\alpha,\beta$, $\Delta=\gamma$)
> 5. $\alpha,\beta,\alpha\to(\beta\to\gamma)\Rightarrow\gamma$ — ss\*
> 6. $\beta,\alpha\to(\beta\to\gamma)\Rightarrow\alpha\to\gamma$ — $\to d$
> 7. $\alpha\to(\beta\to\gamma)\Rightarrow\beta\to(\alpha\to\gamma)$ — $\to d$
> 8. $\Rightarrow(\alpha\to(\beta\to\gamma))\to(\beta\to(\alpha\to\gamma))$ — $\to d$

## 3. $((\alpha\to\beta)\to(\alpha\to\gamma))\to(\alpha\to(\beta\to\gamma))$

Abbreviazione: $X := (\alpha\to\beta)\to(\alpha\to\gamma)$.

> [!question]- ND
> $$\dfrac{\dfrac{\dfrac{\dfrac{[\alpha]^1\qquad\dfrac{\dfrac{[\beta]^2}{\alpha\to\beta}\,{\scriptstyle\to I,\ \emptyset}\quad[X]^3}{\alpha\to\gamma}\,{\scriptstyle\to E}}{\gamma}\,{\scriptstyle\to E}}{\beta\to\gamma}\,{\scriptstyle\to I,2}}{\alpha\to(\beta\to\gamma)}\,{\scriptstyle\to I,1}}{X\to(\alpha\to(\beta\to\gamma))}\,{\scriptstyle\to I,3}$$
> Il trucco: da $\beta$ ottieni $\alpha\to\beta$ con una $\to I$ **vacua**.

> [!question]- PK
> 1. $\alpha,\beta,\alpha\Rightarrow\gamma,\beta$ — da $\beta\Rightarrow\beta$: is\*, ss\*, id, sd
> 2. $\beta,\alpha\Rightarrow\gamma,\alpha\to\beta$ — $\to d$
> 3. $\alpha\to\gamma,\alpha\Rightarrow\gamma$ — Lemma MP
> 4. $\alpha\to\gamma,\beta,\alpha\Rightarrow\gamma$ — is, ss
> 5. $X,\beta,\alpha\Rightarrow\gamma$ — $\to s$ (da 2 e 4; $\Gamma=\beta,\alpha$, $\Delta=\gamma$)
> 6. $\beta,\alpha,X\Rightarrow\gamma$ — ss\*
> 7. $\alpha,X\Rightarrow\beta\to\gamma$ — $\to d$
> 8. $X\Rightarrow\alpha\to(\beta\to\gamma)$ — $\to d$
> 9. $\Rightarrow X\to(\alpha\to(\beta\to\gamma))$ — $\to d$

## 4. $(\alpha\to\gamma)\to(\alpha\to(\beta\to\gamma))$

> [!question]- ND
> $$\dfrac{\dfrac{\dfrac{\dfrac{[\alpha]^1\quad[\alpha\to\gamma]^2}{\gamma}\,{\scriptstyle\to E}}{\beta\to\gamma}\,{\scriptstyle\to I,\ \emptyset}}{\alpha\to(\beta\to\gamma)}\,{\scriptstyle\to I,1}}{(\alpha\to\gamma)\to(\alpha\to(\beta\to\gamma))}\,{\scriptstyle\to I,2}$$

> [!question]- PK
> 1. $\alpha\to\gamma,\alpha\Rightarrow\gamma$ — Lemma MP
> 2. $\beta,\alpha\to\gamma,\alpha\Rightarrow\gamma$ — is
> 3. $\beta,\alpha,\alpha\to\gamma\Rightarrow\gamma$ — ss
> 4. $\alpha,\alpha\to\gamma\Rightarrow\beta\to\gamma$ — $\to d$
> 5. $\alpha\to\gamma\Rightarrow\alpha\to(\beta\to\gamma)$ — $\to d$
> 6. $\Rightarrow(\alpha\to\gamma)\to(\alpha\to(\beta\to\gamma))$ — $\to d$

## 5. $((\alpha\to\beta)\to(\beta\to\alpha))\to(\beta\to\alpha)$

Abbreviazione: $X := (\alpha\to\beta)\to(\beta\to\alpha)$.

> [!question]- ND
> $$\dfrac{\dfrac{\dfrac{[\beta]^1\qquad\dfrac{\dfrac{[\beta]^1}{\alpha\to\beta}\,{\scriptstyle\to I,\ \emptyset}\quad[X]^2}{\beta\to\alpha}\,{\scriptstyle\to E}}{\alpha}\,{\scriptstyle\to E}}{\beta\to\alpha}\,{\scriptstyle\to I,1}}{X\to(\beta\to\alpha)}\,{\scriptstyle\to I,2}$$
> L'assunzione $[\beta]^1$ è usata **due volte** e chiusa da un'unica $\to I$.

> [!question]- PK
> 1. $\alpha,\beta\Rightarrow\alpha,\beta$ — da $\beta\Rightarrow\beta$: is, id, sd
> 2. $\beta\Rightarrow\alpha,\alpha\to\beta$ — $\to d$
> 3. $\beta\to\alpha,\beta\Rightarrow\alpha$ — Lemma MP
> 4. $X,\beta\Rightarrow\alpha$ — $\to s$ (da 2 e 3; $\Gamma=\beta$, $\Delta=\alpha$)
> 5. $\beta,X\Rightarrow\alpha$ — ss
> 6. $X\Rightarrow\beta\to\alpha$ — $\to d$
> 7. $\Rightarrow X\to(\beta\to\alpha)$ — $\to d$

## 6. $(\alpha\to\beta)\to(\alpha\land\gamma\to\beta\land\gamma)$

> [!question]- ND
> $$\dfrac{\dfrac{\dfrac{\dfrac{\dfrac{[\alpha\land\gamma]^1}{\alpha}\,{\scriptstyle\land E(a)}\quad[\alpha\to\beta]^2}{\beta}\,{\scriptstyle\to E}\qquad\dfrac{[\alpha\land\gamma]^1}{\gamma}\,{\scriptstyle\land E(b)}}{\beta\land\gamma}\,{\scriptstyle\land I}}{\alpha\land\gamma\to\beta\land\gamma}\,{\scriptstyle\to I,1}}{(\alpha\to\beta)\to(\alpha\land\gamma\to\beta\land\gamma)}\,{\scriptstyle\to I,2}$$

> [!question]- PK
> 1. $\alpha\to\beta,\alpha\Rightarrow\beta$ — Lemma MP
> 2. $\alpha,\gamma,\alpha\to\beta\Rightarrow\beta$ — is, ss\*
> 3. $\alpha,\gamma,\alpha\to\beta\Rightarrow\gamma$ — da $\gamma\Rightarrow\gamma$: is\*, ss\*
> 4. $\alpha,\gamma,\alpha\to\beta\Rightarrow\beta\land\gamma$ — $\land d$ (da 2 e 3)
> 5. $\alpha\land\gamma,\alpha\to\beta\Rightarrow\beta\land\gamma$ — $\land s$
> 6. $\alpha\to\beta\Rightarrow\alpha\land\gamma\to\beta\land\gamma$ — $\to d$
> 7. $\Rightarrow(\alpha\to\beta)\to(\alpha\land\gamma\to\beta\land\gamma)$ — $\to d$

## 7. $(\alpha\to\beta)\to(\alpha\lor\gamma\to\beta\lor\gamma)$

> [!question]- ND
> $$\dfrac{\dfrac{\dfrac{[\alpha\lor\gamma]^3\qquad\dfrac{\dfrac{[\alpha]^1\quad[\alpha\to\beta]^4}{\beta}\,{\scriptstyle\to E}}{\beta\lor\gamma}\,{\scriptstyle\lor I(a)}\qquad\dfrac{[\gamma]^2}{\beta\lor\gamma}\,{\scriptstyle\lor I(b)}}{\beta\lor\gamma}\,{\scriptstyle\lor E,1,2}}{\alpha\lor\gamma\to\beta\lor\gamma}\,{\scriptstyle\to I,3}}{(\alpha\to\beta)\to(\alpha\lor\gamma\to\beta\lor\gamma)}\,{\scriptstyle\to I,4}$$
> Ragionamento per casi su $\alpha\lor\gamma$.

> [!question]- PK
> 1. $\alpha\to\beta,\alpha\Rightarrow\beta$ — Lemma MP
> 2. $\alpha,\alpha\to\beta\Rightarrow\beta,\gamma$ — ss, id
> 3. $\alpha,\alpha\to\beta\Rightarrow\beta\lor\gamma$ — $\lor d$
> 4. $\gamma,\alpha\to\beta\Rightarrow\beta,\gamma$ — da $\gamma\Rightarrow\gamma$: is, ss, id, sd
> 5. $\gamma,\alpha\to\beta\Rightarrow\beta\lor\gamma$ — $\lor d$
> 6. $\alpha\lor\gamma,\alpha\to\beta\Rightarrow\beta\lor\gamma$ — $\lor s$ (da 3 e 5)
> 7. $\alpha\to\beta\Rightarrow\alpha\lor\gamma\to\beta\lor\gamma$ — $\to d$
> 8. $\Rightarrow(\alpha\to\beta)\to(\alpha\lor\gamma\to\beta\lor\gamma)$ — $\to d$

## 8. $(\alpha\to\beta\land\gamma)\to(\alpha\to\beta)\land(\alpha\to\gamma)$

Si legge $(\alpha\to(\beta\land\gamma))\to((\alpha\to\beta)\land(\alpha\to\gamma))$. Abbreviazione: $X := \alpha\to\beta\land\gamma$.

> [!question]- ND
> $$\dfrac{\dfrac{\dfrac{\dfrac{\dfrac{[\alpha]^1\quad[X]^3}{\beta\land\gamma}\,{\scriptstyle\to E}}{\beta}\,{\scriptstyle\land E(a)}}{\alpha\to\beta}\,{\scriptstyle\to I,1}\qquad\dfrac{\dfrac{\dfrac{[\alpha]^2\quad[X]^3}{\beta\land\gamma}\,{\scriptstyle\to E}}{\gamma}\,{\scriptstyle\land E(b)}}{\alpha\to\gamma}\,{\scriptstyle\to I,2}}{(\alpha\to\beta)\land(\alpha\to\gamma)}\,{\scriptstyle\land I}}{X\to(\alpha\to\beta)\land(\alpha\to\gamma)}\,{\scriptstyle\to I,3}$$

> [!question]- PK
> **Ramo $\beta$**
> 1. $\alpha\Rightarrow\beta,\alpha$ — da $\alpha\Rightarrow\alpha$: id, sd
> 2. $\beta\land\gamma,\alpha\Rightarrow\beta$ — da $\beta\Rightarrow\beta$: is\*, ss\* fino a $\beta,\gamma,\alpha\Rightarrow\beta$, poi $\land s$
> 3. $X,\alpha\Rightarrow\beta$ — $\to s$ (da 1 e 2)
> 4. $\alpha,X\Rightarrow\beta$ — ss
> 5. $X\Rightarrow\alpha\to\beta$ — $\to d$
>
> **Ramo $\gamma$** (analogo)
> 6. $X\Rightarrow\alpha\to\gamma$
>
> **Conclusione**
> 7. $X\Rightarrow(\alpha\to\beta)\land(\alpha\to\gamma)$ — $\land d$ (da 5 e 6)
> 8. $\Rightarrow X\to(\alpha\to\beta)\land(\alpha\to\gamma)$ — $\to d$

## 9. $(\alpha\to\gamma)\lor(\beta\to\gamma)\to(\alpha\land\beta\to\gamma)$

Abbreviazione: $D := (\alpha\to\gamma)\lor(\beta\to\gamma)$.

> [!question]- ND
> $$\dfrac{\dfrac{\dfrac{[D]^4\qquad\dfrac{\dfrac{[\alpha\land\beta]^3}{\alpha}\,{\scriptstyle\land E(a)}\quad[\alpha\to\gamma]^1}{\gamma}\,{\scriptstyle\to E}\qquad\dfrac{\dfrac{[\alpha\land\beta]^3}{\beta}\,{\scriptstyle\land E(b)}\quad[\beta\to\gamma]^2}{\gamma}\,{\scriptstyle\to E}}{\gamma}\,{\scriptstyle\lor E,1,2}}{\alpha\land\beta\to\gamma}\,{\scriptstyle\to I,3}}{D\to(\alpha\land\beta\to\gamma)}\,{\scriptstyle\to I,4}$$

> [!question]- PK
> 1. $\alpha\to\gamma,\alpha,\beta\Rightarrow\gamma$ — Lemma MP + is, ss\*
> 2. $\beta\to\gamma,\alpha,\beta\Rightarrow\gamma$ — Lemma MP + is, ss
> 3. $D,\alpha,\beta\Rightarrow\gamma$ — $\lor s$ (da 1 e 2)
> 4. $\alpha,\beta,D\Rightarrow\gamma$ — ss\*
> 5. $\alpha\land\beta,D\Rightarrow\gamma$ — $\land s$
> 6. $D\Rightarrow\alpha\land\beta\to\gamma$ — $\to d$
> 7. $\Rightarrow D\to(\alpha\land\beta\to\gamma)$ — $\to d$

## 10. $(\alpha\to\beta)\lor(\alpha\to\gamma)\to(\alpha\to\beta\lor\gamma)$

Abbreviazione: $D := (\alpha\to\beta)\lor(\alpha\to\gamma)$.

> [!question]- ND
> $$\dfrac{\dfrac{\dfrac{[D]^4\qquad\dfrac{\dfrac{[\alpha]^3\quad[\alpha\to\beta]^1}{\beta}\,{\scriptstyle\to E}}{\beta\lor\gamma}\,{\scriptstyle\lor I(a)}\qquad\dfrac{\dfrac{[\alpha]^3\quad[\alpha\to\gamma]^2}{\gamma}\,{\scriptstyle\to E}}{\beta\lor\gamma}\,{\scriptstyle\lor I(b)}}{\beta\lor\gamma}\,{\scriptstyle\lor E,1,2}}{\alpha\to\beta\lor\gamma}\,{\scriptstyle\to I,3}}{D\to(\alpha\to\beta\lor\gamma)}\,{\scriptstyle\to I,4}$$

> [!question]- PK
> 1. $\alpha\to\beta,\alpha\Rightarrow\beta,\gamma$ — Lemma MP + id
> 2. $\alpha\to\beta,\alpha\Rightarrow\beta\lor\gamma$ — $\lor d$
> 3. $\alpha\to\gamma,\alpha\Rightarrow\beta,\gamma$ — Lemma MP + id, sd
> 4. $\alpha\to\gamma,\alpha\Rightarrow\beta\lor\gamma$ — $\lor d$
> 5. $D,\alpha\Rightarrow\beta\lor\gamma$ — $\lor s$ (da 2 e 4)
> 6. $\alpha,D\Rightarrow\beta\lor\gamma$ — ss
> 7. $D\Rightarrow\alpha\to\beta\lor\gamma$ — $\to d$
> 8. $\Rightarrow D\to(\alpha\to\beta\lor\gamma)$ — $\to d$

## 11. $\alpha\lor(\alpha\land\beta)\to\alpha$ — assorbimento

> [!question]- ND
> $$\dfrac{\dfrac{[\alpha\lor(\alpha\land\beta)]^3\qquad[\alpha]^1\qquad\dfrac{[\alpha\land\beta]^2}{\alpha}\,{\scriptstyle\land E(a)}}{\alpha}\,{\scriptstyle\lor E,1,2}}{\alpha\lor(\alpha\land\beta)\to\alpha}\,{\scriptstyle\to I,3}$$
> Nel primo caso la derivazione di $\alpha$ da $[\alpha]^1$ è la sola assunzione.

> [!question]- PK
> 1. $\alpha\Rightarrow\alpha$ — Teor. 1
> 2. $\alpha,\beta\Rightarrow\alpha$ — is, ss
> 3. $\alpha\land\beta\Rightarrow\alpha$ — $\land s$
> 4. $\alpha\lor(\alpha\land\beta)\Rightarrow\alpha$ — $\lor s$ (da 1 e 3)
> 5. $\Rightarrow\alpha\lor(\alpha\land\beta)\to\alpha$ — $\to d$
