---
tags: [logica, istituzioni-di-logica, modulo3, correttezza, teorema]
fonte: Dispensa "Completezza 2.0" (Paoli), §2, Teorema 3
---
# Teorema di adeguatezza — parte 1: correttezza
← [[02 - Semantica e conseguenza logica]] | → [[04 - Consistenza e insiemi consistenti massimali]]

> [!abstract] Obiettivo: Teorema di adeguatezza
> Per ogni $\Gamma$ e ogni $\alpha$: $$\Gamma \vdash_{HK} \alpha \iff \Gamma \models_{HK} \alpha$$
> In particolare, per $\Gamma = \emptyset$: **$\vdash_{HK} \alpha \iff \models_{HK} \alpha$** — i teoremi di HK sono esattamente le tautologie. Si divide in:
> - **Correttezza** ($\Rightarrow$): se $\Gamma \vdash_{HK} \alpha$ allora $\Gamma \models_{HK} \alpha$ — *HK non deriva nulla di sbagliato*
> - **Completezza** ($\Leftarrow$): se $\Gamma \models_{HK} \alpha$ allora $\Gamma \vdash_{HK} \alpha$ — *HK deriva tutto ciò che è valido* → [[05 - Valutazione canonica e completezza]]

---

## Teorema 3 — Correttezza

> [!abstract] Teorema 3 (Correttezza per HK)
> Per ogni insieme di formule $\Gamma$ e ogni formula $\alpha$: $$\text{se } \Gamma \vdash_{HK} \alpha \text{ allora } \Gamma \models_{HK} \alpha.$$

> [!success]- Dimostrazione
> Per ipotesi esiste una derivazione di $\alpha$ da $\Gamma$ in HK. Procedo per **induzione sulla lunghezza $n$** della derivazione (= numero di formule che contiene).
>
> **Base: $n = 1$.** La derivazione è il solo passo $\alpha$, quindi:
> 1. $\alpha \in \Gamma$: sia $v$ con $v(\gamma)=1$ per ogni $\gamma\in\Gamma$; in particolare $v(\alpha)=1$. Dunque $\Gamma \models \alpha$.
> 2. $\alpha$ è un assioma: si verifica (tavole di verità) che **ogni assioma è una tautologia** — vedi tabella sotto. E una tautologia è conseguenza logica di qualunque $\Gamma$.
>
> **Passo induttivo.** Ipotesi induttiva (forte): *per ogni derivazione da $\Gamma$ di lunghezza al massimo $m$, la formula derivata è conseguenza logica di $\Gamma$.*
> Sia $D$ una derivazione di $\alpha$ da $\Gamma$ di lunghezza $m+1$.
> - Se $\alpha \in \Gamma$ o $\alpha$ è un assioma → come nella base.
> - Se $\alpha$ è ottenuta per **R1** da formule precedenti $\delta$ e $\delta \to \alpha$: sia $D'$ la derivazione di lunghezza $m$ ottenuta togliendo $\alpha$ da $D$. $D'$ contiene $\delta$ e $\delta \to \alpha$, e per ipotesi induttiva $\Gamma \models \delta$ e $\Gamma \models \delta \to \alpha$.
>   Sia $v$ una valutazione con $v(\gamma)=1$ per ogni $\gamma\in\Gamma$. Allora $v(\delta) = 1$ e $v(\delta\to\alpha)=1$. Per la clausola di $\to$, $v(\alpha) = 1$. Quindi $\Gamma \models \alpha$. ∎

> [!note] Precisazione utile all'orale
> L'ipotesi induttiva si applica a $\delta$ e a $\delta\to\alpha$ perché ciascuna è l'**ultima formula di un segmento iniziale** di $D'$, e un segmento iniziale di una derivazione è ancora una derivazione (di lunghezza $\le m$). Per questo serve l'induzione "fino a $m$" (forte) e non solo "su $m$".

> [!tip] Idea in una riga
> Gli assiomi sono veri sotto ogni valutazione e **R1 preserva la verità**; quindi tutto ciò che si deriva da premesse vere è vero.

---

## 🧩 Verifica che gli assiomi sono tautologie (lasciata per esercizio)

Metodo rapido: supponi che $v$ renda **falso** l'assioma e arriva a una contraddizione. (Ricorda: $v(\varphi \to \psi) = 0$ sse $v(\varphi)=1$ e $v(\psi)=0$.)

| Assioma | Perché non può essere falso |
|---|---|
| **A1** $\alpha \to (\beta \to \alpha)$ | Falso ⇒ $v(\alpha)=1$ e $v(\beta\to\alpha)=0$ ⇒ $v(\alpha)=0$. Contraddizione. |
| **A2** | Falso ⇒ $v(\alpha\to(\beta\to\gamma))=1$, $v(\alpha\to\beta)=1$, $v(\alpha)=1$, $v(\gamma)=0$ ⇒ $v(\beta)=1$ ⇒ $v(\beta\to\gamma)=0$ ⇒ $v(\alpha\to(\beta\to\gamma))=0$. Contraddizione. |
| **A3**, **A4** | Falso ⇒ $v(\alpha\land\beta)=1$ ma $v(\alpha)=0$ (risp. $v(\beta)=0$). Impossibile. |
| **A5** | Falso ⇒ $v(\alpha\to\beta)=v(\alpha\to\gamma)=1$, $v(\alpha)=1$, $v(\beta\land\gamma)=0$; ma allora $v(\beta)=v(\gamma)=1$. Contraddizione. |
| **A6**, **A7** | Falso ⇒ $v(\alpha)=1$ (risp. $v(\beta)=1$) ma $v(\alpha\lor\beta)=0$. Impossibile. |
| **A8** | Falso ⇒ $v(\alpha\to\gamma)=v(\beta\to\gamma)=1$, $v(\alpha\lor\beta)=1$, $v(\gamma)=0$ ⇒ $v(\alpha)=v(\beta)=0$ ⇒ $v(\alpha\lor\beta)=0$. Contraddizione. |
| **A9** | Falso ⇒ $v(\alpha\to\beta)=v(\alpha\to\neg\beta)=1$, $v(\neg\alpha)=0$ ⇒ $v(\alpha)=1$ ⇒ $v(\beta)=1$ e $v(\neg\beta)=1$. Contraddizione. |
| **A10** $\alpha \to (\neg\alpha \to \beta)$ | Falso ⇒ $v(\alpha)=1$ e $v(\neg\alpha)=1$. Impossibile. |
| **A11** $\neg\neg\alpha \to \alpha$ | Falso ⇒ $v(\neg\neg\alpha)=1$, cioè $v(\alpha)=1$, ma $v(\alpha)=0$. Contraddizione. |
