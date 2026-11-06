---
tags: [logica, istituzioni-di-logica, modulo3, completezza, teorema]
fonte: Dispensa "Completezza 2.0" (Paoli), §2, Lemma 10, Teorema 11
---
# Valutazione canonica e Teorema di completezza
← [[04 - Consistenza e insiemi consistenti massimali]] | → [[06 - Mappa della dimostrazione e checklist]]

## Lemma 10 — Lemma della valutazione canonica

> [!abstract] Lemma 10 (Valutazione canonica) — converso del [[04 - Consistenza e insiemi consistenti massimali#Lemma 5 — Soddisfacibile ⇒ consistente|Lemma 5]]
> Se $\Gamma$ è consistente, allora esiste una valutazione $v$ tale che $v(\gamma)=1$ per ogni $\gamma\in\Gamma$.
> (Consistente ⇒ soddisfacibile.)

> [!success]- Dimostrazione
> Sia $\Gamma$ consistente. Per **Lindenbaum** (Lemma 7) esiste $\Delta$ consistente massimale con $\Gamma\subseteq\Delta$.
> Definisco, per ogni variabile proposizionale $p$:
> $$v(p) = \begin{cases} 1 & \text{se } p \in \Delta \\ 0 & \text{altrimenti} \end{cases}$$
> Questa funzione determina una valutazione su tutto $FOR(\mathcal{L}_0)$ (la si estende nel modo consueto con le clausole). Si chiama **valutazione canonica** per $\Gamma$.
>
> **Affermazione:** per ogni $\alpha \in FOR(\mathcal{L}_0)$, $\quad v(\alpha)=1 \iff \alpha\in\Delta$.
> Per **induzione sul numero $n$ di connettivi** di $\alpha$.
>
> **Base: $n=0$.** $\alpha$ è una variabile, e $v(\alpha)=1 \iff \alpha\in\Delta$ per definizione di $v$.
>
> **Passo induttivo.** Ipotesi: l'affermazione vale per tutte le formule con al massimo $m$ connettivi. Sia $\alpha$ con $m+1$ connettivi; guardo il connettivo principale.
> - **$\alpha = \neg\beta$** ($\beta$ ha $m$ connettivi):
> $$v(\neg\beta)=1 \iff v(\beta)=0 \overset{\text{IH}}{\iff} \beta\notin\Delta \overset{\text{L9.1}}{\iff} \neg\beta\in\Delta$$
> - 🧩 **$\alpha = \beta\land\gamma$** ($\beta,\gamma$ hanno $\le m$ connettivi):
> $$v(\beta\land\gamma)=1 \iff v(\beta)=1 \text{ e } v(\gamma)=1 \overset{\text{IH}}{\iff} \beta\in\Delta \text{ e } \gamma\in\Delta \overset{\text{L9.2}}{\iff} \beta\land\gamma\in\Delta$$
> - 🧩 **$\alpha = \beta\lor\gamma$**:
> $$v(\beta\lor\gamma)=1 \iff v(\beta)=1 \text{ o } v(\gamma)=1 \overset{\text{IH}}{\iff} \beta\in\Delta \text{ o } \gamma\in\Delta \overset{\text{L9.3}}{\iff} \beta\lor\gamma\in\Delta$$
> - 🧩 **$\alpha = \beta\to\gamma$**:
> $$v(\beta\to\gamma)=1 \iff v(\beta)=0 \text{ o } v(\gamma)=1 \overset{\text{IH}}{\iff} \beta\notin\Delta \text{ o } \gamma\in\Delta \overset{\text{L9.4}}{\iff} \beta\to\gamma\in\Delta$$
>   (l'ultima equivalenza: "$\beta\notin\Delta$ o $\gamma\in\Delta$" è proprio "se $\beta\in\Delta$ allora $\gamma\in\Delta$").
>
> *(La dispensa svolge solo il caso $\neg$ e dice che gli altri "si ragiona nello stesso modo".)*
>
> **Conclusione.** $v(\delta)=1$ per ogni $\delta\in\Delta$, e poiché $\Gamma\subseteq\Delta$, in particolare $v(\gamma)=1$ per ogni $\gamma\in\Gamma$. ∎

> [!tip] Idea
> L'insieme consistente massimale $\Delta$ "**è**" una riga di tavola di verità: le formule vere sono esattamente quelle in $\Delta$. Il Lemma 9 garantisce che l'appartenenza a $\Delta$ rispetti le clausole delle valutazioni.

---

## Teorema 11 — Completezza

> [!abstract] Teorema 11 (Completezza per HK)
> Per ogni insieme di formule $\Gamma$ e ogni formula $\alpha$: $$\text{se } \Gamma\models_{HK}\alpha \text{ allora } \Gamma\vdash_{HK}\alpha.$$

> [!success]- Dimostrazione
> Per **contrapposizione**: mostro che se $\Gamma\nvdash_{HK}\alpha$ allora $\Gamma\not\models_{HK}\alpha$.
> 1. Sia $\Gamma\nvdash\alpha$. Allora $\Gamma\cup\{\neg\alpha\}$ è **consistente** (contrapposta del Lemma 6 (i): se fosse inconsistente avremmo $\Gamma\vdash\alpha$).
> 2. Per il **Lemma 10**, la valutazione canonica $v$ per $\Gamma\cup\{\neg\alpha\}$ dà $v(\gamma)=1$ per ogni $\gamma\in\Gamma\cup\{\neg\alpha\}$.
> 3. Cioè $v(\gamma)=1$ per ogni $\gamma\in\Gamma$ e $v(\neg\alpha)=1$, ossia $v(\alpha)=0$.
> 4. Esiste quindi una valutazione che rende vere tutte le formule di $\Gamma$ e falsa $\alpha$: $\Gamma\not\models\alpha$. ∎

---

## Teorema di adeguatezza

> [!abstract] Teorema di adeguatezza per HK (Teorema 3 + Teorema 11)
> Per ogni $\Gamma$ e ogni $\alpha$: $$\Gamma\vdash_{HK}\alpha \iff \Gamma\models_{HK}\alpha$$

> [!abstract] Corollario — I teoremi di HK sono le tautologie
> Prendendo $\Gamma=\emptyset$: per ogni $\alpha$, $$\vdash_{HK}\alpha \iff \models_{HK}\alpha$$
> Quindi HK è davvero un calcolo che **genera in modo effettivo** esattamente le tautologie definite con le tavole di verità.

> [!note] 🧩 Riformulazione equivalente (utile all'orale)
> Lemma 5 + Lemma 10 dicono: **$\Gamma$ è consistente sse $\Gamma$ è soddisfacibile**. Completezza "forte" e questa equivalenza sono due facce della stessa medaglia.
