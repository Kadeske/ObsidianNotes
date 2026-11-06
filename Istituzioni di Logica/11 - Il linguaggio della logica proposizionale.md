---
tags: [logica, istituzioni-di-logica, modulo2, sintassi, formule]
fonte: "Percorsi di logica", cap. La logica proposizionale, §2.6, pp. 57–65
---
# Il linguaggio della logica proposizionale (§2.6)
← [[10 - Nozioni preliminari e induzione]] | → [[12 - Tavole di verità, valutazioni e conseguenza logica]]

> [!info] Obiettivo del capitolo (p. 57)
> Introdurre il linguaggio proposizionale, darne una semantica **vero-funzionale** e una procedura effettiva (i *tableaux*) per determinare il valore di verità di una formula.
> Note storiche (Leibniz, De Morgan, Schröder, Boole, Peirce, Peano, Russell–Whitehead, Hilbert–Ackermann): solo contesto.

---

## Alfabeto

> [!info] Alfabeto
> 1. un insieme **numerabile** $Var = \{p_i : i\in\mathbb{N}\}$ di **variabili proposizionali**;
> 2. **connettivi logici**: $\neg$ (not, negazione), $\land$ (and, congiunzione), $\lor$ (or, disgiunzione), $\to$ ("se…, allora…", implicazione/condizionale);
> 3. **simboli ausiliari**: $($ e $)$.

- Quando non c'è rischio di confusione si scrive $p, q, r, \dots$ al posto di $p_0, p_1, p_2, \dots$
- Le lettere maiuscole $A, B, C, \dots$ indicano formule generiche: sono **metavariabili** (non appartengono al linguaggio, servono a parlarne).

## Definizione 6.82 — Formule

> [!info] Definizione 6.82
> L'insieme $For$ delle **formule** è il **più piccolo** insieme che soddisfa:
> 1. $Var \subseteq For$;
> 2. se $A\in For$, allora $(\neg A)\in For$;
> 3. se $A, B\in For$, allora $(A\land B), (A\lor B), (A\to B)\in For$.

- $\neg$ è **unario**; $\land, \lor, \to$ sono **binari**.
- Con le metavariabili si rappresentano **schemi** di formule: $A\land B$ rappresenta una qualsiasi congiunzione di due formule.
- È una [[10 - Nozioni preliminari e induzione#3. Definizioni induttive (pp. 47–48)|definizione induttiva]]. Nella dispensa è la stessa, con $FOR(\mathcal{L}_0)$ (vedi [[01 - Il calcolo HK (sintassi)|nota 01]]).

> [!question]- Esercizio 6.83 — Quali stringhe sono formule?
> 1. $p_0 \lor \lor p_1$ → **no**: due connettivi binari consecutivi, nessuna clausola la genera.
> 2. $\neg A$ → è uno **schema** di formula (una formula per ogni sostituzione di $A$). A rigore, per la Def. 6.82, con le parentesi: $(\neg A)$.
> 3. $(p_0 \land p_1)\to p_1$ → **sì** (a rigore $((p_0\land p_1)\to p_1)$; le parentesi esterne si omettono).
> 4. $\neg)$ → **no**.

## Convenzione 6.84 — Parentesi

> [!warning] Convenzione 6.84
> Per $A, B\in For$:
> - $(\neg A) \star B = \neg A \star B$ e $A\star(\neg B) = A\star\neg B$, per ogni $\star\in\{\land,\lor,\to\}$;
> - $(A\star B)\to C = A\star B\to C$ e $C\to(A\star B) = C\to A\star B$, per ogni $\star\in\{\land,\lor\}$.
>
> In parole: **$\neg$ lega più forte** di $\land$ e $\lor$, che **legano allo stesso modo** tra loro e più di $\to$.

| Scrittura completa | Abbreviata |
|---|---|
| $((\neg p)\land(\neg q))\to(r\lor s)$ | $\neg p\land\neg q\to r\lor s$ |
| $((p)\land(p\lor q))\lor(r\land s)$ | $(p\land(p\lor q))\lor(r\land s)$ |

Nel secondo esempio le parentesi attorno a $p\lor q$ e a $r\land s$ **restano**: $\land$ e $\lor$ hanno la stessa forza.

> [!caution] Fuori programma
> - **Def. 6.85** (grado di complessità $\#(A)$) ed Es. 6.86
> - **Def. 6.87** (sequenza di formazione), Esempio 6.88, Es. 6.89
> - **Lemma 6.90** (decomposizione unica)
>
> Basta sapere l'idea: $\#(A)$ conta i connettivi di $A$ (serve in induzioni come Es. 12.202).

---

## Sottoformule

> [!info] Definizione 6.91 — Sottoformula
> $A$ è una **sottoformula** di $B$ se $A$ occorre in una sequenza di formazione di $B$. Intuitivamente: $A$ è una delle formule usate per **costruire** $B$ (inclusa $B$ stessa).

> [!info] Definizione 6.92 — Sottoformule immediate $SFI(A)$
> 1. se $A = p\in Var$: $SFI(A) = \emptyset$;
> 2. se $A = \neg B$: $SFI(A) = \{B\}$;
> 3. se $A\in\{B\land C,\ B\lor C,\ B\to C\}$: $SFI(A) = \{B, C\}$.
>
> $B$ e $C$ sono la sottoformula immediata **sinistra** e **destra** di $A$.

> [!info] Terminologia
> - **Atomica** (semplice, atomo): formula senza sottoformule immediate, cioè una variabile. Altrimenti **composta**.
> - **Connettivo principale**: il connettivo che ha come argomenti le sottoformule immediate di $A$.
> - **Albero diadico** di $A$: ogni nodo ha **al più due** successori; la radice è $A$; ogni nodo che non è una variabile precede esattamente le proprie sottoformule immediate.

> [!example] Esempio (2.4): albero di $((p\to q)\to\neg r)\land(s\lor t)$
> ```mermaid
> graph TD
>     R["((p → q) → ¬r) ∧ (s ∨ t)"] --> L["(p → q) → ¬r"]
>     R --> D["s ∨ t"]
>     L --> I["p → q"]
>     L --> N["¬r"]
>     I --> P["p"]
>     I --> Q["q"]
>     N --> r["r"]
>     D --> S["s"]
>     D --> T["t"]
> ```
> Connettivo principale: $\land$. Sottoformule immediate: $(p\to q)\to\neg r$ (sinistra) e $s\lor t$ (destra).

> [!question]- Esercizio 6.93 — Sottoformule immediate
> | Formula | Conn. principale | $SFI$ |
> |---|---|---|
> | $\neg(p\lor q)\to\neg r$ | $\to$ | $\{\neg(p\lor q),\ \neg r\}$ |
> | $\neg p\lor(q\to\neg r)$ | $\lor$ | $\{\neg p,\ q\to\neg r\}$ |
> | $\neg\neg\neg p$ | $\neg$ | $\{\neg\neg p\}$ |
> | $(p\to(p\to p))\to((p\to p)\to(p\to p))$ | $\to$ (centrale) | $\{p\to(p\to p),\ (p\to p)\to(p\to p)\}$ |
