---
tags: [logica, istituzioni-di-logica, modulo2, deduzione-naturale, ND, calcoli-sintattici]
fonte: "Percorsi di logica", §3.11, pp. 126–138
---
# Il calcolo della Deduzione Naturale ND (§3.11)
← [[13 - Il calcolo à la Hilbert H (libro)]] | → [[15 - Calcolo dei sequenti Pk]]

> [!warning] Esclusioni del programma
> Saltare: **Def. 11.186** (definizione induttiva formale delle derivazioni), **Teorema 11.194**, **Lemma 11.198** (eventualmente come esercizio) e **tutti gli esercizi che contengono la negazione**.

## Idea

- Introdotto da **Gentzen** e, indipendentemente, da **Jaśkowski** (1934–35). Obiettivo: codificare il **ragionamento ipotetico** usato in matematica.
- **Nessun assioma**, solo regole. Per ogni connettivo (tranne quelli non primitivi e $\neg$):
  - regole di **introduzione** (da quali premesse si inferisce una formula con quel connettivo principale): da formule più semplici a più complesse;
  - regole di **eliminazione** (cosa si deduce da una formula con quel connettivo principale): da più complesse a più semplici.
- Ogni conclusione dipende da **assunzioni**. Alcune regole le lasciano **aperte**, altre permettono di **chiuderle** (scaricarle): la conclusione non dipende più da esse. Un'assunzione chiusa si scrive $[A]^i$, con l'indice $i$ della regola che la chiude.

---

## Le regole di ND

> [!abstract] Regole di inferenza di ND
> **Congiunzione**
> $$\dfrac{A\qquad B}{A\land B}\,{\scriptstyle\land I}\qquad\qquad \dfrac{A\land B}{A}\,{\scriptstyle\land E(a)}\qquad \dfrac{A\land B}{B}\,{\scriptstyle\land E(b)}$$
> **Disgiunzione**
> $$\dfrac{A}{A\lor B}\,{\scriptstyle\lor I(a)}\qquad \dfrac{B}{A\lor B}\,{\scriptstyle\lor I(b)}\qquad\qquad \dfrac{A\lor B\qquad \begin{matrix}[A]^i\\ \vdots\\ C\end{matrix}\qquad \begin{matrix}[B]^j\\ \vdots\\ C\end{matrix}}{C}\,{\scriptstyle\lor E,\ i,j}$$
> **Implicazione**
> $$\dfrac{\begin{matrix}[A]^i\\ \vdots\\ B\end{matrix}}{A\to B}\,{\scriptstyle\to I,\ i}\qquad\qquad \dfrac{A\qquad A\to B}{B}\,{\scriptstyle\to E}$$
> **Negazione / assurdo**
> $$\dfrac{\begin{matrix}[\neg A]^i\\ \vdots\\ B\end{matrix}\qquad \begin{matrix}[\neg A]^i\\ \vdots\\ \neg B\end{matrix}}{A}\,{\scriptstyle RAA,\ i}\qquad\qquad \dfrac{A\qquad \neg A}{B}\,{\scriptstyle EFQ}$$

| Regola | Lettura intuitiva |
|---|---|
| $\land I$ | da una prova di $A$ e una di $B$ ottengo una prova di $A\land B$ |
| $\land E$ | una prova di $A\land B$ è a fortiori una prova di $A$ (di $B$) |
| $\lor I$ | ciò che supporta $A$ (o $B$) supporta a fortiori $A\lor B$ |
| $\lor E$ | **ragionamento per casi**: se da $A$ deduco $C$ e da $B$ deduco $C$, ciò che supporta $A\lor B$ supporta $C$. $C$ non dipende più da $A$ e $B$ (chiuse, indici $i,j$), ma solo dalle assunzioni di $A\lor B$ |
| $\to I$ | se da (una o più occorrenze di) $A$ deduco $B$, deduco $A\to B$ senza dipendere più da $A$ |
| $\to E$ | coincide con il **modus ponens** di H |
| RAA | **riduzione all'assurdo**: assumo $\neg A$, derivo una contraddizione ($B$ e $\neg B$), concludo $A$ chiudendo $\neg A$ |
| EFQ | ***ex falso quodlibet***: da una contraddizione segue qualsiasi cosa |

Come gli assiomi di H, le regole sono **schemi**.

> [!info] Definizione 11.184
> - **Premesse**: formule sopra la linea; **conclusione**: sotto.
> - In una regola di **eliminazione**: premessa **maggiore** = quella che contiene il connettivo eliminato; **minori** = le altre (es. in $\to E$, $A\to B$ è maggiore e $A$ è minore).

> [!info] Derivazione in ND
> Una derivazione di $A$ da $\Gamma$ è un **albero finito** etichettato con formule, la cui **radice** è $A$. Ogni nodo dipende dai predecessori immediati tramite un'istanza di una regola; le **foglie** sono assunzioni (aperte o chiuse). Radice **in basso**, foglie **in alto**.
> Notazione: $\dfrac{\mathcal{D}}{A}$ indica una derivazione $\mathcal{D}$ di $A$.
> *(La Def. 11.186, che lo formalizza induttivamente, è fuori programma.)*

> [!info] Definizione 11.187 — Dimostrazione, derivabilità, teorema
> - **Dimostrazione**: derivazione in cui **tutte** le assunzioni sono chiuse.
> - $\Gamma\vdash_{ND} A$: esiste una derivazione di $A$ le cui assunzioni **aperte** sono in $\Gamma$.
> - Se $\Gamma = \emptyset$: $\vdash_{ND} A$, e $A$ è un **teorema**.
> - Notazione: $\vdash$ per $\vdash_{ND}$; $\Gamma,\Delta,A_1,\dots,A_n\vdash B$ per $\Gamma\cup\Delta\cup\{A_1,\dots,A_n\}\vdash B$.

---

## Esempi

> [!example] Esempio 11.185 — $\vdash (A\land B\to C)\to(B\to(A\to C))$
> $$\dfrac{\dfrac{\dfrac{\dfrac{[A\land B\to C]^3\qquad \dfrac{[A]^1\quad [B]^2}{A\land B}\,{\scriptstyle\land I}}{C}\,{\scriptstyle\to E}}{A\to C}\,{\scriptstyle\to I,1}}{B\to(A\to C)}\,{\scriptstyle\to I,2}}{(A\land B\to C)\to(B\to(A\to C))}\,{\scriptstyle\to I,3}$$
> Ogni $\to I$ chiude **esattamente un'**assunzione.

> [!example] Chiusura vacua — $\vdash A\to(B\to A)$
> $$\dfrac{\dfrac{[A]^1}{B\to A}\,{\scriptstyle\to I,\ \emptyset}}{A\to(B\to A)}\,{\scriptstyle\to I,1}$$
> In $\to I$, $\lor E$ e RAA si può chiudere un numero **arbitrario, anche nullo**, di assunzioni. Qui la prima $\to I$ chiude *vacuamente* $B$: se ho una dimostrazione di $A$ indipendente da $B$, ne ho anche una in cui $A$ dipende (banalmente) da $B$. Simbolo: $\emptyset$.

> [!example] Chiusura multipla — $\vdash (A\to(B\to C))\to((A\to B)\to(A\to C))$ (è Ax2!)
> $$\dfrac{\dfrac{\dfrac{\dfrac{\dfrac{[A]^1\quad [A\to(B\to C)]^4}{B\to C}\,{\scriptstyle\to E}\qquad \dfrac{[A]^1\quad [A\to B]^3}{B}\,{\scriptstyle\to E}}{C}\,{\scriptstyle\to E}}{A\to C}\,{\scriptstyle\to I,1}}{(A\to B)\to(A\to C)}\,{\scriptstyle\to I,3}}{(A\to(B\to C))\to((A\to B)\to(A\to C))}\,{\scriptstyle\to I,4}$$
> La prima $\to I$ chiude **due** occorrenze di $[A]^1$.

> [!info] Regola derivabile
> Una regola $R$ con premesse $A_1,\dots,A_k$, assunzioni chiuse $[B_1]^i,\dots,[B_j]^w$ e conclusione $C$ è **derivabile** dalle regole $R_1,\dots,R_n$ se $C$ si ottiene da $A_1,\dots,A_k$ (scaricando eventualmente $[B_1]^i,\dots$) applicando alcune tra $R_1,\dots,R_n$.

> [!note] Osservazione 11.188 — EFQ è derivabile
> Con RAA e chiusura vacua: date derivazioni $\mathcal{D}$ di $A$ e $\mathcal{D}'$ di $\neg A$,
> $$\dfrac{\dfrac{\mathcal{D}}{A}\qquad\dfrac{\mathcal{D}'}{\neg A}}{B}\,{\scriptstyle RAA,\ \emptyset}$$
> dove l'assunzione chiusa vacuamente è $\neg B$. Quindi EFQ si potrebbe omettere.

> [!abstract] Lemma 11.189 — Doppia negazione
> Sono derivabili in ND:
> $$\dfrac{\neg\neg A}{A}\,{\scriptstyle DN(a)}\qquad\qquad \dfrac{A}{\neg\neg A}\,{\scriptstyle DN(b)}$$

> [!success]- Dimostrazione di DN(b)
> $$\dfrac{A\qquad \dfrac{[\neg\neg A]^1\quad [\neg\neg\neg A]^2}{\neg A}\,{\scriptstyle RAA,1,\emptyset}}{\neg\neg A}\,{\scriptstyle RAA,2,\emptyset}$$
> - **Prima RAA** (indice 1): $\neg\neg A$ e $\neg\neg\neg A = \neg(\neg\neg A)$ sono una contraddizione ($B$ e $\neg B$). Concludo $\neg A$ chiudendo $[\neg\neg A]^1 = [\neg(\neg A)]^1$.
> - **Seconda RAA** (indice 2): $A$ e $\neg A$ sono una contraddizione. Concludo $\neg\neg A$ chiudendo $[\neg\neg\neg A]^2 = [\neg(\neg\neg A)]^2$.
> - Il simbolo $\emptyset$ segnala, come nota il libro, che uno dei due rami della contraddizione non dipende davvero dall'assunzione scaricata: la dipendenza è banale (chiusura vacua).
>
> **DN(a)** si prova in modo analogo (esercizio). Una soluzione diretta:
> $$\dfrac{[\neg A]^1\qquad \neg\neg A}{A}\,{\scriptstyle RAA,1}$$
> assumo $\neg A$; insieme a $\neg\neg A = \neg(\neg A)$ ho una contraddizione, e RAA chiude $[\neg A]^1$. ∎

> [!example] Esempio 11.191 — Terzo escluso: $\vdash A\lor\neg A$
> $$\dfrac{\dfrac{\dfrac{\dfrac{[A]^1}{A\lor\neg A}\,{\scriptstyle\lor I(a)}\qquad [\neg(A\lor\neg A)]^2}{\neg A}\,{\scriptstyle RAA(b),1}}{A\lor\neg A}\,{\scriptstyle\lor I(b)}\qquad [\neg(A\lor\neg A)]^2}{A\lor\neg A}\,{\scriptstyle RAA,2}$$
> RAA(b) è la regola derivabile dell'Es. 11.190: da $[A]^i$ derivo $B$ e $\neg B$, concludo $\neg A$.

> [!example] Esempio 11.192 — Se $\vdash A$ allora $\vdash\neg(A\to\neg A)$
> Sia $\mathcal{D}$ una dimostrazione di $A$ (tutte le assunzioni chiuse):
> $$\dfrac{\dfrac{\mathcal{D}}{A}\qquad \dfrac{\dfrac{\mathcal{D}}{A}\quad [A\to\neg A]^1}{\neg A}\,{\scriptstyle\to E}}{\neg(A\to\neg A)}\,{\scriptstyle RAA(b),1,\emptyset}$$
> Anche qui tutte le assunzioni sono chiuse, quindi $\vdash\neg(A\to\neg A)$.

### 🧩 Esercizio 11.193 — soluzioni degli item senza negazione (3, 5, 7)

> [!question]- (3) $\vdash (A\to(B\to C))\to(A\land B\to C)$
> $$\dfrac{\dfrac{\dfrac{\dfrac{\dfrac{[A\land B]^1}{A}\,{\scriptstyle\land E(a)}\quad [A\to(B\to C)]^2}{B\to C}\,{\scriptstyle\to E}\qquad \dfrac{[A\land B]^1}{B}\,{\scriptstyle\land E(b)}}{C}\,{\scriptstyle\to E}}{A\land B\to C}\,{\scriptstyle\to I,1}}{(A\to(B\to C))\to(A\land B\to C)}\,{\scriptstyle\to I,2}$$

> [!question]- (5) $\Gamma, A_1,\dots,A_n\vdash B$ sse $\Gamma\vdash A_n\to(\dots(A_2\to(A_1\to B))\dots)$
> **($\Rightarrow$)** Data la derivazione $\mathcal{D}$ di $B$ da $\Gamma, A_1,\dots,A_n$, applico $\to I$ chiudendo $A_1$ e ottengo $A_1\to B$; poi $\to I$ chiudendo $A_2$ e ottengo $A_2\to(A_1\to B)$; … dopo $n$ passi ho la formula voluta, con assunzioni aperte solo in $\Gamma$. (Se un $A_i$ non compare, la chiusura è vacua.)
> **($\Leftarrow$)** Data la derivazione da $\Gamma$ di $A_n\to(\dots(A_1\to B)\dots)$, aggiungo le assunzioni $A_n, A_{n-1},\dots,A_1$ e applico $\to E$ $n$ volte: da $A_n$ ottengo $A_{n-1}\to(\dots)$, …, fino a $A_1\to B$ e infine $B$.

> [!question]- (7) $\Gamma, A_1,\dots,A_n\vdash B$ sse $\Gamma,\bigwedge_{i=1}^n A_i\vdash B$
> **($\Rightarrow$)** Nella derivazione $\mathcal{D}$ sostituisco ogni foglia aperta $A_i$ con una derivazione di $A_i$ dalla sola assunzione $\bigwedge_{i=1}^n A_i = ((A_1\land A_2)\land\dots)\land A_n$, ottenuta con applicazioni ripetute di $\land E(a)$ e $\land E(b)$.
> **($\Leftarrow$)** Dalle assunzioni $A_1,\dots,A_n$ costruisco $\bigwedge A_i$ con $n-1$ applicazioni di $\land I$, poi la "attacco" al posto di ogni foglia $\bigwedge A_i$ nella derivazione data.

> [!caution] Teorema 11.194 — fuori programma
> Afferma che $\Gamma\vdash_{ND} A$ sse $\Gamma\models A$: correttezza e completezza di ND. Il libro lo dimostra nel §3.13, che il programma sostituisce con la dispensa "Calcoli".

---

## Ricerca delle prove e forma normale (pp. 135–138)

- Per costruire una derivazione di $A$ da $\Gamma$ conviene procedere **"dal basso verso l'alto"**: si congettura l'ultima regola usata, poi quelle per le premesse, fino alle assunzioni in $\Gamma$.
- Problema: la ricerca può **non terminare** per i **loop** (formule che ricompaiono).

> [!example] Esempio 11.195 — una derivazione con loop
> $$\dfrac{\dfrac{\dfrac{[C]^1\qquad \dfrac{\dfrac{[A]^2}{A\lor B}\,{\scriptstyle\lor I(a)}}{C\to A\lor B}\,{\scriptstyle\to I,\emptyset}}{A\lor B}\,{\scriptstyle\to E}}{C\to A\lor B}\,{\scriptstyle\to I,1}}{A\to(C\to A\lor B)}\,{\scriptstyle\to I,2}$$
> $A\lor B$ e $C\to A\lor B$ compaiono in due passi successivi: $\to E$ viene **subito dopo** $\to I$.

> [!info] Formula massimale, forma normale
> - Un'introduzione seguita **subito** dall'eliminazione dello stesso connettivo produce una formula che è conclusione di un'introduzione e premessa **maggiore** di un'eliminazione: è una **formula massimale**.
> - Una derivazione senza formule massimali è **in forma normale** (normalizzata): niente occorrenze ridondanti.
>
> Versione normale dell'Esempio 11.195:
> $$\dfrac{\dfrac{\dfrac{[A]^1}{A\lor B}\,{\scriptstyle\lor I(a)}}{C\to(A\lor B)}\,{\scriptstyle\to I,\emptyset}}{A\to(C\to(A\lor B))}\,{\scriptstyle\to I,1}$$

> [!abstract] Teorema 11.196 — Forma normale (Prawitz, 1965)
> Ogni derivazione $\mathcal{D}$ di $A$ da $\Gamma$ in ND si trasforma **in modo effettivo** in una derivazione $\mathcal{D}^*$ della stessa $A$, dalle stesse assunzioni $\Gamma$, **senza formule massimali**.

> [!note] Lemma 11.198 — fuori programma, utile come esercizio
> Proprietà di $\vdash$ (con $\Gamma\vdash$ = da $\Gamma$ si deriva qualsiasi formula): $A\vdash A$; monotonia; transitività; $\land$, $\lor$, $\to$ "a sinistra e a destra"; (8) $\Gamma, A\vdash B$ implica $\Gamma\vdash A\to B$; ecc.
> **Prova di (8)** data dal libro: se $\mathcal{D}$ deriva $B$ da $\Gamma, A$ chiudendo $j$ insiemi di assunzioni, allora
> $$\dfrac{\begin{matrix}\Gamma,[A]^{j+1}\\ \mathcal{D}\\ B\end{matrix}}{A\to B}\,{\scriptstyle\to I,\ j+1}$$
> è una derivazione di $A\to B$ da $\Gamma$. È il **teorema di deduzione** per ND, praticamente immediato grazie a $\to I$.

> [!example] Altri esercizi
> 11 esercizi aggiuntivi svolti in ND (e in PK): [[17 - Esercizi aggiuntivi ND e PK]]. Le domande d'esame su ND sono in [[18 - Possibili domande d'esame]].

> [!tip] Perché passare ai sequenti
> In ND la ricerca "a ritroso" delle prove **non è meccanica**: non c'è una strategia rigorosa per tenere traccia delle assunzioni. Il [[15 - Calcolo dei sequenti Pk|calcolo dei sequenti]] supera questi limiti e dà una strategia **effettiva** (corretta e terminante) di ricerca.
