---
tags: [logica, istituzioni-di-logica, modulo3, completezza, consistenza, lindenbaum]
fonte: Dispensa "Completezza 2.0" (Paoli), §2, Lemmi 4–9
---
# Consistenza e insiemi consistenti massimali
← [[03 - Teorema di correttezza]] | → [[05 - Valutazione canonica e completezza]]

La completezza è più ardua della correttezza: servono nuove definizioni e una catena di lemmi (vedi [[06 - Mappa della dimostrazione e checklist|mappa]]).

---

## Consistenza

> [!info] Definizione — Insieme consistente
> $\Gamma \subseteq FOR(\mathcal{L}_0)$ è **consistente** sse esiste almeno una formula $\alpha$ tale che $\Gamma \nvdash_{HK} \alpha$.
> Cioè: da $\Gamma$ **non si deriva tutto**. $\Gamma$ è **inconsistente** se non è consistente (da $\Gamma$ si deriva ogni formula).

### Lemma 4 — Caratterizzazione della consistenza

> [!abstract] Lemma 4
> $\Gamma$ è consistente **sse** per nessuna formula $\beta$ si ha sia $\Gamma \vdash_{HK} \beta$ sia $\Gamma \vdash_{HK} \neg\beta$.
> (Consistente = da $\Gamma$ non si deriva una contraddizione.)

> [!success]- Dimostrazione
> **($\Rightarrow$)**, per contrapposizione: se esiste $\beta$ con $\Gamma \vdash \beta$ e $\Gamma \vdash \neg\beta$, allora $\Gamma$ è inconsistente.
> Unisco le due derivazioni (dalle stesse assunzioni $\Gamma$) e, presa $\alpha$ **arbitraria**, prolungo:
> | Formula | Giustificazione |
> |---|---|
> | $\vdots$ ($D$) | |
> | $\beta$ | derivata da $\Gamma$ |
> | $\vdots$ | |
> | $\neg\beta$ | derivata da $\Gamma$ |
> | $\beta \to (\neg\beta \to \alpha)$ | A10 |
> | $\neg\beta \to \alpha$ | R1 |
> | $\alpha$ | R1 |
>
> Quindi $\Gamma \vdash \alpha$ per $\alpha$ arbitraria: $\Gamma$ è inconsistente.
>
> **($\Leftarrow$)**, per contrapposizione: se $\Gamma$ è inconsistente, da $\Gamma$ si deriva tutto, in particolare un qualsiasi $\beta$ e $\neg\beta$. Banale. ∎

> [!note]
> Da qui in avanti il Lemma 4 si usa senza menzionarlo: si sceglie la definizione di consistenza più comoda.

### Lemma 5 — Soddisfacibile ⇒ consistente

> [!abstract] Lemma 5
> Se esiste una valutazione $v$ tale che $v(\gamma)=1$ per ogni $\gamma \in \Gamma$, allora $\Gamma$ è consistente.

> [!success]- Dimostrazione
> Per contrapposizione: se $\Gamma$ è inconsistente, nessuna $v$ rende vere tutte le formule di $\Gamma$.
> Sia $\Gamma \vdash \beta$ e $\Gamma \vdash \neg\beta$. Per la **correttezza** ([[03 - Teorema di correttezza|Teorema 3]]), $\Gamma \models \beta$ e $\Gamma \models \neg\beta$.
> Se esistesse $v$ con $v(\gamma)=1$ per ogni $\gamma\in\Gamma$, per definizione di conseguenza logica avremmo $v(\beta)=v(\neg\beta)=1$: impossibile, perché $v(\beta)=1$ implica $v(\neg\beta)=0$. ∎

> [!tip] Il converso del Lemma 5 è il [[05 - Valutazione canonica e completezza#Lemma 10 — Lemma della valutazione canonica|Lemma 10]] — ed è il cuore della completezza.

### Lemma 6 — Inconsistenza e derivabilità

> [!abstract] Lemma 6
> (i) Se $\Gamma \cup \{\neg\alpha\}$ è inconsistente, allora $\Gamma \vdash_{HK} \alpha$.
> (ii) Se $\Gamma \cup \{\alpha\}$ è inconsistente, allora $\Gamma \vdash_{HK} \neg\alpha$.

> [!success]- Dimostrazione di (i)
> Sia $\Gamma \cup \{\neg\alpha\} \vdash \beta$ e $\Gamma \cup \{\neg\alpha\} \vdash \neg\beta$. Per il **Teorema di deduzione**: $\Gamma \vdash \neg\alpha \to \beta$ e $\Gamma \vdash \neg\alpha \to \neg\beta$. Unisco le derivazioni e prolungo:
> | Formula | Giustificazione |
> |---|---|
> | $\neg\alpha \to \beta$ | derivata da $\Gamma$ |
> | $\neg\alpha \to \neg\beta$ | derivata da $\Gamma$ |
> | $(\neg\alpha \to \beta) \to ((\neg\alpha \to \neg\beta) \to \neg\neg\alpha)$ | A9 (con $\alpha := \neg\alpha$) |
> | $(\neg\alpha \to \neg\beta) \to \neg\neg\alpha$ | R1 |
> | $\neg\neg\alpha$ | R1 |
> | $\neg\neg\alpha \to \alpha$ | A11 |
> | $\alpha$ | R1 |
>
> Quindi $\Gamma \vdash \alpha$. ∎

> [!success]- 🧩 Dimostrazione di (ii) (nella dispensa: "simile")
> Sia $\Gamma \cup \{\alpha\} \vdash \beta$ e $\Gamma \cup \{\alpha\} \vdash \neg\beta$. Per il Teorema di deduzione $\Gamma \vdash \alpha \to \beta$ e $\Gamma \vdash \alpha \to \neg\beta$. Prolungo:
> | Formula | Giustificazione |
> |---|---|
> | $\alpha \to \beta$ | derivata da $\Gamma$ |
> | $\alpha \to \neg\beta$ | derivata da $\Gamma$ |
> | $(\alpha \to \beta) \to ((\alpha \to \neg\beta) \to \neg\alpha)$ | A9 |
> | $(\alpha \to \neg\beta) \to \neg\alpha$ | R1 |
> | $\neg\alpha$ | R1 |
>
> Quindi $\Gamma \vdash \neg\alpha$ (qui non serve A11). ∎

> [!tip] Forma contrapposta (usata nel Teorema 11)
> Da (i): se $\Gamma \nvdash \alpha$, allora $\Gamma \cup \{\neg\alpha\}$ è **consistente**.

---

## Insiemi consistenti massimali

> [!info] Definizione — Insieme consistente massimale
> $\Gamma \subseteq FOR(\mathcal{L}_0)$ è **consistente massimale** sse:
> 1. $\Gamma$ è consistente;
> 2. ogni $\Delta$ che include **propriamente** $\Gamma$ ($\Gamma \subset \Delta$) è inconsistente.
>
> Intuizione: a $\Gamma$ "non si può aggiungere niente" senza renderlo inconsistente.

### Lemma 7 — Lemma di Lindenbaum

> [!abstract] Lemma 7 (Lindenbaum)
> Se $\Gamma \subseteq FOR(\mathcal{L}_0)$ è consistente, allora esiste $\Delta$ **consistente massimale** tale che $\Gamma \subseteq \Delta$.

> [!success]- Dimostrazione
> **Costruzione.** $FOR(\mathcal{L}_0)$ è **numerabile**: fisso un'enumerazione $\alpha_1, \alpha_2, \alpha_3, \dots$ di tutte le formule. Definisco per induzione:
> $$\Gamma_1 = \Gamma \qquad \Gamma_{n+1} = \begin{cases} \Gamma_n \cup \{\alpha_n\} & \text{se } \Gamma_n \cup \{\alpha_n\} \text{ è consistente} \\ \Gamma_n & \text{altrimenti} \end{cases} \qquad \Delta = \bigcup_{n} \Gamma_n$$
> Per costruzione: ogni $\Gamma_n$ è consistente; $\Gamma_1 \subseteq \Gamma_2 \subseteq \dots$; e $\Gamma \subseteq \Delta$.
>
> **1) $\Delta$ è consistente.** Per assurdo, sia $\Delta \vdash \beta$ e $\Delta \vdash \neg\beta$. Le derivazioni sono **finite**, quindi usano solo un insieme finito di assunzioni $\{\delta_1, \dots, \delta_m\} \subseteq \Delta$. Ciascun $\delta_i$ è stato inserito a un certo stadio della costruzione; sia $k$ il **più grande** di questi indici. Poiché la successione è crescente, $\{\delta_1,\dots,\delta_m\} \subseteq \Gamma_k$, quindi $\Gamma_k \vdash \beta$ e $\Gamma_k \vdash \neg\beta$: $\Gamma_k$ sarebbe inconsistente. Contraddizione.
>
> **2) $\Delta$ è massimale.** Sia $\Delta \subset \Delta'$. Allora esiste $\psi \in \Delta'$ con $\psi \notin \Delta$. Nell'enumerazione $\psi = \alpha_i$ per qualche $i$. Poiché $\alpha_i \notin \Delta$, al passo $i$ non è stata aggiunta: dunque $\Gamma_i \cup \{\psi\}$ è **inconsistente**. Ma $\Gamma_i \cup \{\psi\} \subseteq \Delta'$, quindi (monotonia) **a maggior ragione** $\Delta'$ è inconsistente. ∎

> [!tip] Punti chiave da citare all'esame
> - serve la **numerabilità** di $FOR(\mathcal{L}_0)$ per l'enumerazione;
> - la consistenza dell'unione usa la **finitezza delle derivazioni** + la catena crescente;
> - la massimalità usa il fatto che **ogni** formula viene "esaminata" una volta.

### Lemma 8 — Chiusura deduttiva

> [!abstract] Lemma 8 (Chiusura deduttiva)
> Se $\Gamma$ è consistente massimale e $\Gamma \vdash_{HK} \alpha$, allora $\alpha \in \Gamma$.

> [!success]- Dimostrazione
> Per assurdo: $\Gamma \vdash \alpha$ ma $\alpha \notin \Gamma$. Allora $\Gamma \subset \Gamma \cup \{\alpha\}$, che per massimalità è **inconsistente**. Per il Lemma 6 (ii), $\Gamma \vdash \neg\alpha$. Insieme a $\Gamma \vdash \alpha$, $\Gamma$ è inconsistente: contraddizione. ∎

### Lemma 9 — Lemma delle clausole omofoniche

> [!abstract] Lemma 9 (Clausole omofoniche)
> Se $\Gamma$ è consistente massimale, per ogni $\alpha, \beta$:
> 1. $\neg\alpha \in \Gamma$ sse $\alpha \notin \Gamma$
> 2. $\alpha \land \beta \in \Gamma$ sse $\alpha \in \Gamma$ **e** $\beta \in \Gamma$
> 3. $\alpha \lor \beta \in \Gamma$ sse $\alpha \in \Gamma$ **o** $\beta \in \Gamma$
> 4. $\alpha \to \beta \in \Gamma$ sse (se $\alpha \in \Gamma$ allora $\beta \in \Gamma$)
>
> "Omofoniche": l'appartenenza a $\Gamma$ si comporta **esattamente come** "$v(\cdot)=1$" nelle clausole delle valutazioni.

> [!success]- Dimostrazione (1) — negazione
> - $\alpha$ e $\neg\alpha$ **non possono** stare entrambe in $\Gamma$: altrimenti $\Gamma \vdash \alpha$ e $\Gamma \vdash \neg\alpha$, e $\Gamma$ sarebbe inconsistente.
> - **Almeno una** delle due ci sta. Considero $\Gamma \cup \{\alpha\}$:
>   - se è inconsistente, per il Lemma 6 (ii) $\Gamma \vdash \neg\alpha$, quindi per il Lemma 8 $\neg\alpha \in \Gamma$;
>   - se è consistente, allora $\alpha \in \Gamma$ (altrimenti sarebbe un'estensione propria consistente, contro la massimalità).
>
> Quindi esattamente una tra $\alpha$ e $\neg\alpha$ sta in $\Gamma$, cioè $\neg\alpha \in \Gamma$ sse $\alpha \notin \Gamma$. ∎

> [!success]- Dimostrazione (2) — congiunzione
> **($\Rightarrow$)** Sia $\alpha\land\beta \in \Gamma$. Per A3, A4: $\vdash \alpha\land\beta\to\alpha$, $\vdash\alpha\land\beta\to\beta$.
> | Formula | Giustificazione |
> |---|---|
> | $\alpha\land\beta\to\alpha$ | A3 |
> | $\alpha\land\beta\to\beta$ | A4 |
> | $\alpha\land\beta$ | $\in \Gamma$ |
> | $\alpha$ | R1 |
> | $\beta$ | R1 |
>
> Quindi $\Gamma\vdash\alpha$, $\Gamma\vdash\beta$ e per il Lemma 8 $\alpha,\beta\in\Gamma$.
>
> **($\Leftarrow$)** Sia $\alpha, \beta \in \Gamma$. Per il [[01 - Il calcolo HK (sintassi)#6. Lemma 2|Lemma 2(i)]] $\vdash \alpha \to (\beta \to \alpha\land\beta)$.
> | Formula | Giustificazione |
> |---|---|
> | $\alpha$ | $\in\Gamma$ |
> | $\beta$ | $\in\Gamma$ |
> | $\alpha \to (\beta \to \alpha\land\beta)$ | Lemma 2(i) |
> | $\beta \to \alpha\land\beta$ | R1 |
> | $\alpha\land\beta$ | R1 |
>
> Per il Lemma 8, $\alpha\land\beta \in \Gamma$. ∎

> [!success]- 🧩 Dimostrazione (3) — disgiunzione (non svolta nella dispensa)
> **($\Leftarrow$)** Se $\alpha \in \Gamma$: da A6 $\alpha \to \alpha\lor\beta$ e R1, $\Gamma \vdash \alpha\lor\beta$; per il Lemma 8 $\alpha\lor\beta\in\Gamma$. Se $\beta\in\Gamma$: analogo con A7.
>
> **($\Rightarrow$)** Sia $\alpha\lor\beta\in\Gamma$. Se $\alpha\in\Gamma$ ho finito. Altrimenti, per (1), $\neg\alpha \in \Gamma$. Mostro che allora $\beta\in\Gamma$:
> - $\Gamma \cup \{\alpha\} \vdash \beta$: infatti da $\alpha$, $\neg\alpha$ (che sta in $\Gamma$) e A10 $\alpha \to (\neg\alpha \to \beta)$ con due R1 ottengo $\beta$. Per il Teorema di deduzione: $\Gamma \vdash \alpha \to \beta$.
> - $\vdash \beta \to \beta$ (T1).
> - A8: $(\alpha\to\beta)\to((\beta\to\beta)\to(\alpha\lor\beta\to\beta))$; due R1 danno $\Gamma \vdash \alpha\lor\beta\to\beta$; con $\alpha\lor\beta\in\Gamma$ e R1, $\Gamma\vdash\beta$.
>
> Per il Lemma 8, $\beta\in\Gamma$. ∎

> [!success]- Dimostrazione (4) — implicazione
> **($\Rightarrow$)** Sia $\alpha\to\beta\in\Gamma$ e $\alpha\in\Gamma$. Per R1 $\Gamma\vdash\beta$, e per il Lemma 8 $\beta\in\Gamma$.
>
> **($\Leftarrow$)** Supponiamo: se $\alpha\in\Gamma$ allora $\beta\in\Gamma$. Cioè $\alpha\notin\Gamma$ **oppure** $\beta\in\Gamma$.
> - **Caso $\alpha\notin\Gamma$**: per (1) $\neg\alpha\in\Gamma$. Si ha $\vdash \neg\alpha \to (\alpha\to\beta)$ *(segue da A10 — vedi nota)*; con R1, $\Gamma\vdash\alpha\to\beta$; per il Lemma 8, $\alpha\to\beta\in\Gamma$.
> - **Caso $\beta\in\Gamma$**: per A1 $\vdash \beta\to(\alpha\to\beta)$; con R1 $\Gamma\vdash\alpha\to\beta$; per il Lemma 8, $\alpha\to\beta\in\Gamma$. ∎
>
> > [!note] Nota sul caso $\alpha \notin \Gamma$
> > La dispensa scrive "in virtù di A10, $\vdash \neg\alpha\to(\alpha\to\beta)$". A rigore A10 è $\alpha\to(\neg\alpha\to\beta)$ (premesse nell'ordine opposto); la forma scambiata si ottiene subito: da $\{\neg\alpha, \alpha\}$ con A10 e due R1 si deriva $\beta$, e due applicazioni del Teorema di deduzione danno $\vdash \neg\alpha\to(\alpha\to\beta)$.
