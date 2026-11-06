---
tags: [logica, istituzioni-di-logica, modulo2, hilbert, calcoli-sintattici]
fonte: "Percorsi di logica", cap. Calcoli sintattici, intro + §3.10 (pp. 113–119, 126)
---
# Calcoli sintattici — il calcolo à la Hilbert del libro (§3.10)
← [[12 - Tavole di verità, valutazioni e conseguenza logica]] | → [[14 - Deduzione naturale ND]]

> [!warning] Il programma consiglia di **sostituire** il §3.10 con la dispensa "Completezza 2.0", §1.1
> → studia [[01 - Il calcolo HK (sintassi)]]. Questa nota riassume solo le differenze del libro e la sua dimostrazione del teorema di deduzione, utile come confronto.

## Introduzione al capitolo (pp. 113–114) — contesto
- Obiettivo della logica moderna: isolare principi che codifichino **l'inferenza corretta**.
- **Frege** (*Begriffsschrift*, 1879): 9 assiomi e 3 regole. Poi Peano, Russell–Whitehead (*Principia Mathematica*), Hilbert–Ackermann: è il **calcolo assiomatico** di Hilbert.
- **Gentzen** (1932–35): **deduzione naturale** (ragionamento da assunzioni). Dimostra la **normalizzazione** (con, tra le conseguenze, la consistenza) per ND **intuizionista**; per il caso classico introduce il **calcolo dei sequenti** e il *Hauptsatz* (eliminazione delle cesure).
- **Prawitz** (1965): normalizzazione di ND classica; **normalizzazione forte** (1971).

## Caratteristiche dei calcoli à la Hilbert (p. 115)
- Un sistema deduttivo = un **linguaggio** + un **apparato deduttivo** (assiomi e regole) che permette di **derivare** formule da insiemi di formule in modo **sintattico**, senza riferirsi al significato.
- Hilbert: **poche regole e molti assiomi**. Deduzione naturale e sequenti: **pochi (o nessun) assiomi e molte regole**.
- Pro: eleganza. Contro: grande difficoltà pratica nelle derivazioni, **nessuna euristica** per costruirle meccanicamente.

> [!info] Regola di inferenza / assioma (§3.10.1)
> Una **regola** è un insieme finito di formule $\{A_1,\dots,A_n\}$ ($n>0$): $A_1..A_{n-1}$ sono le premesse, $A_n$ la conclusione. Un **assioma** è una regola con **zero premesse** (una singola formula).

> [!info] Definizione 10.164 — Derivazione in un calcolo S
> Una **derivazione** $\mathcal{D}$ di $A$ da $\Gamma$ in S è una sequenza finita $A_1,\dots,A_n$ con $A_n = A$ tale che, per ogni $i$:
> - $A_i\in\Gamma$; oppure
> - $A_i$ è un assioma di S; oppure
> - esistono $A_{i_1},\dots,A_{i_k}$ precedenti tali che $\dfrac{A_{i_1}\ \dots\ A_{i_k}}{A_i}$ è una regola di S.
>
> $n$ è la **lunghezza** $l(\mathcal{D})$. Se esiste, $A$ è **derivabile** da $\Gamma$: $\Gamma\vdash_S A$ (anche $\Gamma, A_1,\dots,A_n\vdash_S A$). Le assunzioni possono essere vuote.

> [!abstract] Definizione 10.165 — Il calcolo H (linguaggio $\{\neg,\to\}$)
> - **(Ax1)** $A\to(B\to A)$
> - **(Ax2)** $(A\to(B\to C))\to((A\to B)\to(A\to C))$
> - **(Ax3)** $(\neg B\to\neg A)\to(A\to B)$
> - **(MP)** $\dfrac{A\qquad A\to B}{B}$
>
> Sono **schemi**: istanze di Ax1 sono $p\to(q\to p)$, $p\land\neg q\to(p\to p\land\neg q)$, …

> [!tip] Confronto H (libro) vs HK (dispensa)
> | | H (libro) | HK (dispensa) |
> |---|---|---|
> | Connettivi | $\neg, \to$ (gli altri **definiti**, Oss. 10.166 + Es. 7.112) | $\neg,\land,\lor,\to$ primitivi |
> | Assiomi | 3 | 11 (A1, A2 = Ax1, Ax2) |
> | Regola | MP | R1 = MP |
> | Negazione | Ax3 (contrapposizione) | A9, A10, A11 |

> [!example] Esempio 10.167 — $B\vdash_H A\to B$
> | | Formula | |
> |---|---|---|
> | (1) | $B$ | (Ass) |
> | (2) | $B\to(A\to B)$ | (Ax1) |
> | (3) | $A\to B$ | (MP), (1), (2) |

Esercizio 10.168: $\vdash_H A\to A$. Suggerimento: al passo (1) usare l'istanza di Ax2 $(A\to((A\to A)\to A))\to((A\to(A\to A))\to(A\to A))$. È esattamente **T1** della [[01 - Il calcolo HK (sintassi)|nota 01]].

---

## Teorema 10.170 — Teorema di deduzione (dimostrazione del libro)

> [!abstract] Teorema 10.170
> $$\Gamma, A\vdash_H B \iff \Gamma\vdash_H A\to B$$
> È l'analogo **sintattico** del Teorema 7.109: lega la derivabilità alle proprietà di $\to$.

> [!success]- Dimostrazione ($\Rightarrow$), per induzione su $l(\mathcal{D})$
> Sia $\mathcal{D}$ una derivazione di $B$ da $\Gamma, A$.
>
> **Base** $l(\mathcal{D}) = 1$: $B$ è un assioma di H oppure $B\in\Gamma\cup\{A\}$.
> - se $B$ è un assioma o $B\in\Gamma$: per l'Esempio 10.167 ($B\vdash A\to B$) si ottiene $\Gamma\vdash A\to B$;
> - 🧩 se $B = A$: serve $\vdash A\to A$ (Es. 10.168). Il libro lo assorbe nella frase "opportuna applicazione".
>
> **Passo** (induzione completa): suppongo l'enunciato vero per ogni derivazione di lunghezza $k<n$ e lo provo per $l(\mathcal{D}) = n$. L'unico caso non banale: $B$ (l'ultima formula) è ottenuta per **MP** da $A_i = C$ e $A_j = C\to B$ con $i,j<n$. Per ipotesi induttiva esistono derivazioni da $\Gamma$:
> - $\mathcal{D}'$: $A'_1,\dots,A'_m = A\to C$
> - $\mathcal{D}''$: $A''_1,\dots,A''_l = A\to(C\to B)$
>
> Costruisco $\mathcal{D}'''$ da premesse in $\Gamma$:
> | | Formula | Giustificazione |
> |---|---|---|
> | (1)…(m) | $A'_1,\dots, A\to C$ | $\mathcal{D}'$ |
> | (m+1)…(m+l) | $A''_1,\dots, A\to(C\to B)$ | $\mathcal{D}''$ |
> | (m+l+1) | $(A\to(C\to B))\to((A\to C)\to(A\to B))$ | Ax2 |
> | (m+l+2) | $(A\to C)\to(A\to B)$ | MP, (m+l), (m+l+1) |
> | (m+l+3) | $A\to B$ | MP, (m), (m+l+2) |
>
> Questa è la derivazione di $A\to B$ da $\Gamma$ cercata.
>
> **($\Leftarrow$)** lasciata per esercizio: da $\Gamma\vdash A\to B$ aggiungo l'assunzione $A$ e applico MP. ∎

> [!note] Osservazione 10.183 (p. 126)
> Conseguenza della completezza di H: l'interdefinibilità dei connettivi dell'Es. 7.112 si conserva **anche nel calcolo**. Usando le equivalenze (iv)–(v), H dimostra tutte le conseguenze logiche formulate nel linguaggio $\{\neg,\to,\land,\lor\}$.

> [!caution] Correttezza e completezza (§3.10.2)
> Nel libro le pp. 119–125 contengono correttezza (Teor. 10.171) e completezza di H. **Non sono nel PDF e vanno sostituite dalla dispensa**: [[03 - Teorema di correttezza]], [[04 - Consistenza e insiemi consistenti massimali]], [[05 - Valutazione canonica e completezza]].
