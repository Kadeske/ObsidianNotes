---
tags: [logica, istituzioni-di-logica, modulo3, semantica]
fonte: Dispensa "Completezza 2.0" (Paoli), §1.2
---
# Semantica logica
← [[01 - Il calcolo HK (sintassi)]] | → [[03 - Teorema di correttezza]]

Ogni riga della tavola di verità di $\alpha$ corrisponde a una possibile assegnazione di valori di verità agli enunciati atomici di $\alpha$. Il concetto di **valutazione** formalizza questa idea.

> [!info] Definizione — Valutazione
> Una **valutazione** (di $\mathcal{L}_0$) è una funzione $v : FOR(\mathcal{L}_0) \to \{0,1\}$ tale che:
> - $v(\neg\alpha) = 1$ sse $v(\alpha) = 0$
> - $v(\alpha \land \beta) = 1$ sse $v(\alpha) = 1$ **e** $v(\beta) = 1$
> - $v(\alpha \lor \beta) = 1$ sse $v(\alpha) = 1$ **o** $v(\beta) = 1$
> - $v(\alpha \to \beta) = 1$ sse $v(\alpha) = 0$ **o** $v(\beta) = 1$

> [!note] Osservazione
> Una valutazione è **determinata dai valori sulle variabili**: assegnato un valore a ogni $p_i$, le clausole sopra estendono in modo unico la funzione a tutte le formule (per induzione sulla costruzione delle formule). Questo fatto è usato nel [[05 - Valutazione canonica e completezza#Lemma 10 — Lemma della valutazione canonica|Lemma 10]].

| $v(\alpha)$ | $v(\beta)$ | $\neg\alpha$ | $\alpha\land\beta$ | $\alpha\lor\beta$ | $\alpha\to\beta$ |
|:-:|:-:|:-:|:-:|:-:|:-:|
| 1 | 1 | 0 | 1 | 1 | 1 |
| 1 | 0 | 0 | 0 | 1 | 0 |
| 0 | 1 | 1 | 0 | 1 | 1 |
| 0 | 0 | 1 | 0 | 0 | 1 |

---

> [!info] Definizione — Conseguenza logica
> $\alpha$ è **conseguenza logica** di $\Gamma$, scritto $\Gamma \models_{HK} \alpha$, quando:
> **per ogni** valutazione $v$, se $v(\gamma) = 1$ per ogni $\gamma \in \Gamma$, allora $v(\alpha) = 1$.
>
> Equivalentemente: **non esiste** alcuna $v$ con $v(\gamma)=1$ per ogni $\gamma \in \Gamma$ e $v(\alpha) = 0$.

Intuizione: $\alpha$ non può essere falsa quando tutte le formule di $\Gamma$ sono vere (cfr. la definizione di **schema di argomento valido** del primo modulo).

> [!info] Tautologie
> - Se $\alpha$ è una **tautologia**, non esiste $v$ con $v(\alpha)=0$: quindi una tautologia è conseguenza logica dell'**insieme vuoto**. Si scrive $\models_{HK} \alpha$.
> - Una tautologia è conseguenza logica di **qualsiasi** insieme di formule $\Gamma$.

> [!tip] Sintassi vs semantica — tienile ben separate
> | | Sintattico | Semantico |
> |---|---|---|
> | Relazione | $\Gamma \vdash_{HK} \alpha$ (derivabilità) | $\Gamma \models_{HK} \alpha$ (conseguenza logica) |
> | Caso $\Gamma = \emptyset$ | $\vdash \alpha$: $\alpha$ è un **teorema** | $\models \alpha$: $\alpha$ è una **tautologia** |
> | Si verifica con | derivazioni (successioni finite) | valutazioni / tavole di verità |
> | "Coerenza" | **consistenza** | **soddisfacibilità** (esiste $v$ che rende vere tutte) |
