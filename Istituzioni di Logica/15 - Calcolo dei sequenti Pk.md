---
tags: [logica, istituzioni-di-logica, modulo2, sequenti, Pk, calcoli-sintattici]
fonte: "Percorsi di logica", §3.12 fino a 3.12.1 (pp. 139–145) + dispensa "Calcoli", Teorema 1
---
# Il calcolo dei Sequenti Pk (§3.12)
← [[14 - Deduzione naturale ND]] | → [[16 - Dispensa Calcoli]]

> [!warning] Programma
> §3.12 **fino al sottoparagrafo 3.12.1** (incluso), da integrare con la dispensa "Calcoli", **Teorema 1** (eliminazione dell'identità) → [[16 - Dispensa Calcoli]].
> Il §3.13 è sostituito dal **Teorema 5** della stessa dispensa (equivalenza ND–PK).

## Sequenti

- Ideato da **Gentzen** (1934, assistente di Hilbert a Göttingen) come estensione dei suoi sistemi di deduzione naturale. È uno degli algoritmi dimostrativi più **eleganti e agevoli**.
- Come i tableaux, Pk è un metodo **algoritmico** per la logica degli enunciati, **consistente e completo** rispetto alla semantica.
- $\Gamma,\Delta,\Pi,\Sigma$ indicano **sequenze finite** di formule (non insiemi: una formula può occorrere più volte, l'ordine conta → [[10 - Nozioni preliminari e induzione#1. Notazioni insiemistiche di base 🧩|insiemi vs sequenze]]).

> [!info] Sequente
> Un **sequente** è un'espressione $\Gamma\Rightarrow\Delta$ con $\Gamma,\Delta$ sequenze finite, eventualmente vuote.
> - $\Gamma$ = **antecedente**, $\Delta$ = **conseguente**.
> - Se entrambe sono vuote: $\Rightarrow$, il **sequente vuoto**.
> - Si omettono le parentesi esterne: $A_1,\dots,A_n\Rightarrow B_1,\dots,B_m$.
>
> **Lettura**: dalla **congiunzione** $\bigwedge_{i=1}^n A_i$ delle formule in $\Gamma$ si deriva la **disgiunzione** $\bigvee_{i=1}^m B_i$ delle formule in $\Delta$.

> [!info] Derivazione in Pk
> - Una **derivazione** $\mathcal{D}$ è un **albero diadico finito** $\mathcal{T}$ in cui ogni nodo è un sequente.
> - La **radice** è il sequente "in fondo" (livello più basso): è ciò che si dimostra.
> - Le **foglie** sono assiomi o, eventualmente, assunzioni.
> - Ogni nodo che non è assioma o assunzione dipende dai suoi (al più due) successori tramite una regola.
> - Se tutte le foglie sono assiomi: **dimostrazione**, e $\Gamma\Rightarrow\Delta$ è **dimostrabile**.
> - $S\vdash_{Pk} s$: esiste una derivazione di $s$ con foglie assiomi o sequenti in $S$. Se $s$ è dimostrabile: $\vdash_{Pk} s$.
> - Rappresentazione: albero "**capovolto**", radice in basso e foglie in alto.

---

## Regole di Pk

> [!abstract] Definizione 12.199 — Assioma
> $$p\Rightarrow p\qquad\text{con } p \text{ variabile proposizionale.}$$
> Per una formula qualsiasi $A$, il sequente $A\Rightarrow A$ è **derivabile** (Es. 12.202, sotto); nelle prove lo si usa come se fosse un assioma.

> [!abstract] Regole strutturali
> | Nome | Sinistra | Destra |
> |---|---|---|
> | **Scambio** | $\dfrac{\Gamma,A,B,\Pi\Rightarrow\Delta}{\Gamma,B,A,\Pi\Rightarrow\Delta}\,{\scriptstyle ss}$ | $\dfrac{\Gamma\Rightarrow\Delta,A,B,\Pi}{\Gamma\Rightarrow\Delta,B,A,\Pi}\,{\scriptstyle sd}$ |
> | **Contrazione** | $\dfrac{A,A,\Gamma\Rightarrow\Delta}{A,\Gamma\Rightarrow\Delta}\,{\scriptstyle cs}$ | $\dfrac{\Gamma\Rightarrow\Delta,A,A}{\Gamma\Rightarrow\Delta,A}\,{\scriptstyle cd}$ |
> | **Indebolimento** | $\dfrac{\Gamma\Rightarrow\Delta}{A,\Gamma\Rightarrow\Delta}\,{\scriptstyle is}$ | $\dfrac{\Gamma\Rightarrow\Delta}{\Gamma\Rightarrow\Delta,A}\,{\scriptstyle id}$ |
>
> **Cesura (cut)**:
> $$\dfrac{\Gamma\Rightarrow\Delta,A\qquad A,\Gamma\Rightarrow\Delta}{\Gamma\Rightarrow\Delta}\,{\scriptstyle cut}$$
> Si dicono "strutturali" perché agiscono sulla **struttura** dei sequenti e (solo apparentemente) non sui connettivi.

> [!abstract] Regole operazionali (introduzione dei connettivi)
> | | Sinistra | Destra |
> |---|---|---|
> | $\neg$ | $\dfrac{\Gamma\Rightarrow\Delta,A}{\neg A,\Gamma\Rightarrow\Delta}\,{\scriptstyle\neg s}$ | $\dfrac{A,\Gamma\Rightarrow\Delta}{\Gamma\Rightarrow\Delta,\neg A}\,{\scriptstyle\neg d}$ |
> | $\land$ | $\dfrac{A,B,\Gamma\Rightarrow\Delta}{A\land B,\Gamma\Rightarrow\Delta}\,{\scriptstyle\land s}$ | $\dfrac{\Gamma\Rightarrow\Delta,A\qquad\Gamma\Rightarrow\Delta,B}{\Gamma\Rightarrow\Delta,A\land B}\,{\scriptstyle\land d}$ |
> | $\lor$ | $\dfrac{A,\Gamma\Rightarrow\Delta\qquad B,\Gamma\Rightarrow\Delta}{A\lor B,\Gamma\Rightarrow\Delta}\,{\scriptstyle\lor s}$ | $\dfrac{\Gamma\Rightarrow\Delta,A,B}{\Gamma\Rightarrow\Delta,A\lor B}\,{\scriptstyle\lor d}$ |
> | $\to$ | $\dfrac{\Gamma\Rightarrow\Delta,A\qquad B,\Gamma\Rightarrow\Delta}{A\to B,\Gamma\Rightarrow\Delta}\,{\scriptstyle\to s}$ | $\dfrac{A,\Gamma\Rightarrow\Delta,B}{\Gamma\Rightarrow\Delta,A\to B}\,{\scriptstyle\to d}$ |

> [!tip] Come ricordarle
> - A **sinistra** una formula conta "in congiunzione", a **destra** "in disgiunzione". Quindi $\land s$ e $\lor d$ hanno **una** premessa, $\land d$ e $\lor s$ **due**.
> - $\neg$ **sposta** la formula dall'altro lato.
> - $\to d$: $A\to B$ a destra equivale ad avere $A$ a sinistra e $B$ a destra (teorema di deduzione!).
> - $\to s$: $A\to B$ a sinistra: o "manca" $A$ (va a destra) o vale $B$ (sta a sinistra).
> - Attenzione alla **posizione**: la formula principale sta **in testa** a sinistra e **in coda** a destra. Per portarla lì servono gli scambi (ss, sd).

> [!info] Terminologia
> - **Contesti**: le occorrenze di $\Gamma,\Delta,\Pi$.
> - **Formule principali**: nel sequente **conclusione**, quelle che non sono contesti.
> - **Formule ausiliarie**: nelle **premesse**, quelle che non sono contesti.
> - **Regole forti**: le operazionali e la cesura. **Deboli**: tutte le altre (scambio, contrazione, indebolimento).
> - Anche queste regole sono **schemi**.

> [!note] Osservazione 12.200 — Differenza con ND
> In ND ogni connettivo ha regole di introduzione **e** di eliminazione; in Pk ci sono **solo regole di introduzione** (a sinistra o a destra). Quindi, **tranne la cesura**, ogni applicazione di una regola di Pk produce sempre un **incremento** e mai una perdita di informazione (cfr. Teor. 12.203).

---

## 3.12.1 Dimostrazioni in Pk

*Le derivazioni sono scritte dall'alto (foglie) verso il basso (radice); ogni riga indica la regola applicata.*

> [!example] Ex falso quodlibet: $\vdash A\land\neg A\Rightarrow$
> 1. $A\Rightarrow A$
> 2. $\neg A, A\Rightarrow$ — $\neg s$
> 3. $A,\neg A\Rightarrow$ — ss
> 4. $A\land\neg A\Rightarrow$ — $\land s$

> [!example] A fortiori: $\vdash\ \Rightarrow A\to(B\to A)$
> 1. $A\Rightarrow A$
> 2. $B, A\Rightarrow A$ — is
> 3. $A\Rightarrow B\to A$ — $\to d$
> 4. $\Rightarrow A\to(B\to A)$ — $\to d$

> [!example] Modus ponens: $\vdash\ \Rightarrow(A\to B)\land A\to B$
> 1. $A\Rightarrow A$  e  $B\Rightarrow B$
> 2. $A\to B, A\Rightarrow B$ — $\to s$ (vedi nota)
> 3. $(A\to B)\land A\Rightarrow B$ — $\land s$
> 4. $\Rightarrow(A\to B)\land A\to B$ — $\to d$
>
> > [!note] Nota sul passo 2
> > Il libro applica $\to s$ direttamente a $A\Rightarrow A$ e $B\Rightarrow B$. A rigore, con contesto $\Gamma = A$, $\Delta = B$, le premesse sarebbero $A\Rightarrow B, A$ e $B, A\Rightarrow B$, che si ottengono dagli identici con indebolimenti e scambi:
> > - $A\Rightarrow A$ → id → $A\Rightarrow A,B$ → sd → $A\Rightarrow B,A$
> > - $B\Rightarrow B$ → is → $A,B\Rightarrow B$ → ss → $B,A\Rightarrow B$

> [!example] De Morgan 1: $\vdash\ \Rightarrow\neg A\land\neg B\to\neg(A\lor B)$
> **Ramo sinistro**
> 1. $B\Rightarrow B$
> 2. $\neg B, B\Rightarrow$ — $\neg s$
> 3. $B,\neg B\Rightarrow$ — ss
> 4. $\neg A, B,\neg B\Rightarrow$ — is
> 5. $B,\neg A,\neg B\Rightarrow$ — ss
>
> **Ramo destro**
> 1. $A\Rightarrow A$
> 2. $\neg A, A\Rightarrow$ — $\neg s$
> 3. $A,\neg A\Rightarrow$ — ss
> 4. $\neg B, A,\neg A\Rightarrow$ — is
> 5. $A,\neg B,\neg A\Rightarrow$ — ss
> 6. $A,\neg A,\neg B\Rightarrow$ — ss
>
> **Unione e conclusione**
> 7. $A\lor B,\neg A,\neg B\Rightarrow$ — $\lor s$ (da destro 6 e sinistro 5)
> 8. $\neg A, A\lor B,\neg B\Rightarrow$ — ss
> 9. $\neg A,\neg B, A\lor B\Rightarrow$ — ss
> 10. $\neg A\land\neg B, A\lor B\Rightarrow$ — $\land s$
> 11. $A\lor B,\neg A\land\neg B\Rightarrow$ — ss
> 12. $\neg A\land\neg B\Rightarrow\neg(A\lor B)$ — $\neg d$
> 13. $\Rightarrow\neg A\land\neg B\to\neg(A\lor B)$ — $\to d$

> [!example] Idempotenza della congiunzione: $\vdash\ \Rightarrow A\land A\to A$
> 1. $A\Rightarrow A$
> 2. $A, A\Rightarrow A$ — is
> 3. $A\land A\Rightarrow A$ — $\land s$
> 4. $\Rightarrow A\land A\to A$ — $\to d$

> [!example] De Morgan 2: $\vdash\ \Rightarrow\neg(A\land B)\to\neg A\lor\neg B$
> 1. $A\Rightarrow A$ → is → $B, A\Rightarrow A$ → ss → $A, B\Rightarrow A$
> 2. $B\Rightarrow B$ → is → $A, B\Rightarrow B$
> 3. $A, B\Rightarrow A\land B$ — $\land d$ (da 1 e 2)
> 4. $B, A\Rightarrow A\land B$ — ss
> 5. $\neg(A\land B), B, A\Rightarrow$ — $\neg s$
> 6. $B,\neg(A\land B), A\Rightarrow$ — ss
> 7. $B, A,\neg(A\land B)\Rightarrow$ — ss
> 8. $A,\neg(A\land B)\Rightarrow\neg B$ — $\neg d$
> 9. $\neg(A\land B)\Rightarrow\neg B,\neg A$ — $\neg d$  *(il libro scrive direttamente $\neg A,\neg B$: serve uno scambio sd)*
> 10. $\neg(A\land B)\Rightarrow\neg A,\neg B$ — sd
> 11. $\neg(A\land B)\Rightarrow\neg A\lor\neg B$ — $\lor d$
> 12. $\Rightarrow\neg(A\land B)\to\neg A\lor\neg B$ — $\to d$

---

## 🧩 Esercizi

> [!question]- Esercizio 12.202 — Per ogni $A$ e ogni $\Gamma$: $\vdash_{Pk}\Gamma, A\Rightarrow A$
> *È il **Teorema 1** della dispensa "Calcoli" (eliminazione dell'identità), con la stessa dimostrazione: vedi [[16 - Dispensa Calcoli#1. Eliminazione dell'identità|nota 16]].*
>
> **Suggerimento del libro**: provare $\vdash A\Rightarrow A$ per induzione su $\#(A)$; poi $\Gamma, A\Rightarrow A$ con indebolimenti (is) ripetuti.
>
> **Base** $A = p$: è l'assioma $p\Rightarrow p$.
> **Passo** (ipotesi: $B\Rightarrow B$ e $C\Rightarrow C$ sono derivabili):
> - $A = \neg B$: $B\Rightarrow B$ → $\neg s$ → $\neg B, B\Rightarrow$ → ss → $B,\neg B\Rightarrow$ → $\neg d$ → $\neg B\Rightarrow\neg B$.
> - $A = B\land C$: $B\Rightarrow B$ → is, ss → $B, C\Rightarrow B$; $C\Rightarrow C$ → is → $B, C\Rightarrow C$; $\land d$ → $B, C\Rightarrow B\land C$; $\land s$ → $B\land C\Rightarrow B\land C$.
> - $A = B\lor C$: $B\Rightarrow B$ → id → $B\Rightarrow B, C$; $C\Rightarrow C$ → id, sd → $C\Rightarrow B, C$; $\lor s$ → $B\lor C\Rightarrow B, C$; $\lor d$ → $B\lor C\Rightarrow B\lor C$.
> - $A = B\to C$: $B\Rightarrow B$ → id, sd → $B\Rightarrow C, B$; $C\Rightarrow C$ → is, ss → $C, B\Rightarrow C$; $\to s$ (con $\Gamma = B$, $\Delta = C$) → $B\to C, B\Rightarrow C$; ss → $B, B\to C\Rightarrow C$; $\to d$ → $B\to C\Rightarrow B\to C$. ∎

**Esercizio 12.201** — Dimostrare con il calcolo dei sequenti (selezione di soluzioni; "is\*/ss\*" = indebolimenti e scambi ripetuti):

> [!question]- (1) $\Rightarrow(A\to B)\lor(B\to A)$
> 1. $A\Rightarrow A$
> 2. $B, A\Rightarrow A$ — is
> 3. $B, A\Rightarrow A, B$ — id
> 4. $B, A\Rightarrow B, A$ — sd
> 5. $A\Rightarrow B, B\to A$ — $\to d$ ($B$ principale)
> 6. $A\Rightarrow B\to A, B$ — sd
> 7. $\Rightarrow B\to A, A\to B$ — $\to d$ ($A$ principale)
> 8. $\Rightarrow A\to B, B\to A$ — sd
> 9. $\Rightarrow(A\to B)\lor(B\to A)$ — $\lor d$
>
> Notevole: è una tautologia **classica**, anche se "intuitivamente" strana.

> [!question]- (2) $\Rightarrow(A\to B)\to((B\to C)\to(A\to C))$
> **Obiettivo intermedio**: $A, B\to C, A\to B\Rightarrow C$, poi tre $\to d$.
> 1. $B\Rightarrow B$ → id, sd → $B\Rightarrow C, B$ → is, ss → $B, A\Rightarrow C, B$
> 2. $C\Rightarrow C$ → is\* → $C, B, A\Rightarrow C$
> 3. $B\to C, B, A\Rightarrow C$ — $\to s$ (da 1 e 2; $\Gamma = B, A$, $\Delta = C$)
> 4. $B, A, B\to C\Rightarrow C$ — ss\*
> 5. $A\Rightarrow A$ → is\*, id, sd → $A, B\to C\Rightarrow C, A$
> 6. $A\to B, A, B\to C\Rightarrow C$ — $\to s$ (da 5 e 4; $\Gamma = A, B\to C$, $\Delta = C$)
> 7. $A, B\to C, A\to B\Rightarrow C$ — ss\*
> 8. $B\to C, A\to B\Rightarrow A\to C$ — $\to d$
> 9. $A\to B\Rightarrow(B\to C)\to(A\to C)$ — $\to d$
> 10. $\Rightarrow(A\to B)\to((B\to C)\to(A\to C))$ — $\to d$

> [!question]- (6) $\Rightarrow(A\land B)\land C\to A\land(B\land C)$
> 1. $A, B, C\Rightarrow B$ e $A, B, C\Rightarrow C$ (identici + is\*, ss\*)
> 2. $A, B, C\Rightarrow B\land C$ — $\land d$
> 3. $A, B, C\Rightarrow A$ (identico + is\*, ss\*)
> 4. $A, B, C\Rightarrow A\land(B\land C)$ — $\land d$ (da 3 e 2)
> 5. $A\land B, C\Rightarrow A\land(B\land C)$ — $\land s$
> 6. $(A\land B)\land C\Rightarrow A\land(B\land C)$ — $\land s$
> 7. $\Rightarrow(A\land B)\land C\to A\land(B\land C)$ — $\to d$

> [!question]- (10) $\Rightarrow(A\to B)\to(\neg B\to\neg A)$
> 1. $A\to B, A\Rightarrow B$ (come nel *modus ponens*)
> 2. $\neg B, A\to B, A\Rightarrow$ — $\neg s$
> 3. $A,\neg B, A\to B\Rightarrow$ — ss\*
> 4. $\neg B, A\to B\Rightarrow\neg A$ — $\neg d$
> 5. $A\to B\Rightarrow\neg B\to\neg A$ — $\to d$
> 6. $\Rightarrow(A\to B)\to(\neg B\to\neg A)$ — $\to d$

> [!question]- (11) $\Rightarrow A\to A\land A$
> 1. $A\Rightarrow A$ e $A\Rightarrow A$
> 2. $A\Rightarrow A\land A$ — $\land d$
> 3. $\Rightarrow A\to A\land A$ — $\to d$

> [!question]- (12) $\Rightarrow\neg\neg A\leftrightarrow A$, cioè $\Rightarrow(\neg\neg A\to A)\land(A\to\neg\neg A)$
> **Prima metà**: $A\Rightarrow A$ → $\neg d$ → $\Rightarrow A,\neg A$ → $\neg s$ → $\neg\neg A\Rightarrow A$ → $\to d$ → $\Rightarrow\neg\neg A\to A$.
> **Seconda metà**: $A\Rightarrow A$ → $\neg s$ → $\neg A, A\Rightarrow$ → ss → $A,\neg A\Rightarrow$ → $\neg d$ → $A\Rightarrow\neg\neg A$ → $\to d$ → $\Rightarrow A\to\neg\neg A$.
> **Unione**: $\land d$ → $\Rightarrow(\neg\neg A\to A)\land(A\to\neg\neg A)$.

> [!question]- (13) $\Rightarrow A\land(A\to B)\to B$
> 1. $A\to B, A\Rightarrow B$ (come nel *modus ponens*)
> 2. $A, A\to B\Rightarrow B$ — ss
> 3. $A\land(A\to B)\Rightarrow B$ — $\land s$
> 4. $\Rightarrow A\land(A\to B)\to B$ — $\to d$

Altri esercizi svolti (in ND e in PK): [[17 - Esercizi aggiuntivi ND e PK]]. Gli altri item (3, 4, 5, 7, 8, 9, 14, 15) si fanno con la stessa tecnica: si parte dal sequente finale e si "smontano" i connettivi, risalendo fino agli identici.

> [!tip] Strategia per le dimostrazioni in Pk
> 1. Parti dal sequente da dimostrare (in basso) e chiediti qual è l'ultima regola operazionale.
> 2. "Smonta" il connettivo principale: le premesse contengono solo **sottoformule** (proprietà della sottoformula).
> 3. Ripeti finché arrivi a sequenti del tipo $\dots, A, \dots\Rightarrow\dots, A,\dots$, che chiudi con identici + indebolimenti + scambi.
> 4. Usa scambi e contrazioni per mettere le formule nella posizione richiesta dalle regole.
> 5. Non serve mai la cesura.

---

## ⚠️ Inizio §3.12.2 (p. 145) — presente nel PDF, forse oltre il programma

> [!abstract] Teorema 12.203 — Proprietà della sottoformula
> Se $\mathcal{T}$ è una dimostrazione **senza cesura**, ogni formula che occorre in $\mathcal{T}$ è **sottoformula** di una formula del sequente finale.
> Motivo: in ogni regola tranne la cesura, le formule delle premesse sono sottoformule di quelle della conclusione. Nelle prove senza cesura le informazioni delle foglie **permangono** nella conclusione.

> [!info] Definizione 12.204 — Sequente valido
> $\Gamma\Rightarrow\Delta$ è **valido** (tautologia) se $\Gamma\models\bigvee\Delta$, cioè se la sua **formula associata** $\bigwedge_{A\in\Gamma} A\to\bigvee_{B\in\Delta} B$ è una tautologia. Una valutazione $v$ **soddisfa** un sequente se rende vera la sua formula associata.

> [!note] Osservazione 12.205
> $\Rightarrow A$ è valido sse $A$ è una tautologia.
