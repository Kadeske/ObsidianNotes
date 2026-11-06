---
tags: [logica, istituzioni-di-logica, modulo2, induzione, insiemi]
fonte: "Percorsi di logica", cap. Nozioni preliminari, pp. 42–43, 47–48
---
# Nozioni preliminari: insiemi e induzione
← [[00 - Indice]] | → [[11 - Il linguaggio della logica proposizionale]]

> [!info] Cosa chiede il programma
> Notazioni insiemistiche di base + **definizioni e dimostrazioni per induzione** (pp. 42–43; ultime 4 righe di p. 47 e prime 2 di p. 48).

---

## 1. Notazioni insiemistiche di base 🧩

*(Il programma chiede solo di conoscerle: riepilogo sintetico.)*

| Simbolo | Significato |
|---|---|
| $a \in X$ | $a$ appartiene a $X$ |
| $X \subseteq Y$ | inclusione: ogni elemento di $X$ sta in $Y$ |
| $X \subset Y$ | inclusione **propria**: $X\subseteq Y$ e $X \neq Y$ |
| $\emptyset$ | insieme vuoto |
| $X\cup Y$, $X \cap Y$, $X\setminus Y$ | unione, intersezione, differenza |
| $\mathcal{P}(X)$ | insieme potenza (tutti i sottoinsiemi di $X$) |
| $X \times Y$ | prodotto cartesiano: coppie ordinate $(x,y)$ |
| $\lvert X \rvert$ | cardinalità di $X$ |
| $\mathbb{N}$ | numeri naturali $\{0,1,2,\dots\}$ |

> [!note] Insiemi vs sequenze (p. 42)
> - Negli **insiemi** ordine e ripetizioni non contano: $\{a,b\}$, $\{b,a\}$, $\{a,b,a,b,b\}$ sono lo stesso insieme.
> - Una **sequenza** a valori in $X$ è un'enumerazione di elementi di $X$ in cui **sono permesse ripetizioni e l'ordine conta**: $(a,b)$, $(b,a)$, $(a,b,a,b,b)$ sono a due a due distinte.
> - Una sequenza **senza ripetizioni** non ammette ripetizioni, ma l'ordine conta ancora.
>
> Questa distinzione torna nel [[15 - Calcolo dei sequenti Pk|calcolo dei sequenti]], dove $\Gamma, \Delta$ sono *sequenze* e non insiemi.

> [!note] Numerabile
> Un insieme è **numerabile** se è in biiezione con $\mathbb{N}$ (si può enumerare come $x_0, x_1, x_2, \dots$). Esempio (Es. 3.53): $\mathbb{N}\times\mathbb{N}$ è numerabile, tramite $f(m,n) = 2^m(2n+1) - 1$.

---

## 2. Il principio d'induzione matematica (§1.4, pp. 42–43)

Intuitivamente l'induzione è una strategia per provare che una proprietà $P$ vale per **tutti** i numeri naturali, cioè che l'insieme dei naturali che soddisfano $P$ coincide con $\mathbb{N}$.

> [!abstract] Principio d'induzione
> Sia $P$ una proprietà e $x_0, x_1, \dots, x_n, \dots$ una successione di oggetti ordinata secondo gli indici ($x_i \le x_j$ sse $i \le j$). Se valgono
> - **(B)** $x_0$ soddisfa $P$;
> - **(P)** per ogni $n\in\mathbb{N}$, se $P(x_n)$ allora $P(x_{n+1})$;
>
> allora vale
> - **(C)** per ogni $n\in\mathbb{N}$, $P(x_n)$.

| Nome | Parte |
|---|---|
| **base** | (B) |
| **passo induttivo** (o solo *passo*) | (P) |
| **ipotesi induttiva** | l'antecedente di (P): "$P(x_n)$" |
| **conclusione** | (C) |

> [!success]- Dimostrazione della validità del principio
> Supponiamo (B) e (P) veri ma (C) falso. Allora esiste un $x_i > x_0$ che non ha la proprietà $P$. Considero
> $$S = \{x_k : x_k \ge x_0 \text{ e } P(x_k) \text{ è falso}\}.$$
> $S \ne \emptyset$ perché $x_i \in S$. Poiché $\mathbb{N}$ è **ben fondato** (ogni sottoinsieme non vuoto ha un minimo), $S$ ha un elemento minimo $x_j$.
> - Per (B), $x_j > x_0$ (quindi $x_{j-1}$ esiste).
> - Poiché $x_j$ è il **minimo** di $S$, $x_{j-1}\notin S$, cioè $P(x_{j-1})$ è vero.
> - Ma allora, per (P), anche $P(x_j)$ è vero: contraddizione con $x_j\in S$.
>
> Quindi (C) deve essere vero. ∎

> [!example]- Esempio classico: $1 + 2 + \dots + n = \frac{n(n+1)}{2}$ (cfr. p. 46)
> **Base** ($n=1$): $1 = \frac{1\cdot 2}{2}$. ✓
> **Passo**: suppongo $1+\dots+n = \frac{n(n+1)}{2}$ (ipotesi induttiva). Allora
> $$1+\dots+n+(n+1) = \frac{n(n+1)}{2} + (n+1) = \Big(\frac n2 + 1\Big)(n+1) = \frac{(n+1)(n+2)}{2} = \frac{(n+1)((n+1)+1)}{2}.$$ ∎

> [!tip] 🧩 Induzione completa (forte)
> Variante usata nel libro (Teor. 4.58, p. 47) e nella [[03 - Teorema di correttezza|dimostrazione di correttezza]]:
> **se** per ogni $n$, da "$P(m)$ per ogni $m<n$" segue $P(n)$, **allora** $P(n)$ per ogni $n$.
> L'ipotesi induttiva riguarda **tutti** i casi precedenti, non solo l'ultimo. È equivalente all'induzione ordinaria.

---

## 3. Definizioni induttive (pp. 47–48)

L'idea dell'induzione si usa anche per **definire** oggetti matematici, ad esempio insiemi.

> [!info] Definizione induttiva di un insieme $X$
> Si specificano:
> 1. un insieme di oggetti **primitivi** $X_0$;
> 2. per ogni $n\in\mathbb{N}$, come ottenere $X_{n+1}$ da $X_n$ applicando delle **regole di costruzione** agli elementi già ottenuti;
>
> e si pone $X = \bigcup_{n} X_n$. 🧩 *(La p. 48 non è nel PDF: questa è la formulazione standard.)* Equivalentemente: $X$ è il **più piccolo** insieme che contiene i primitivi ed è chiuso rispetto alle regole.

> [!example] Esempi nel corso
> - L'insieme delle formule $For$ ([[11 - Il linguaggio della logica proposizionale#Definizione 6.82 — Formule|Def. 6.82]]): primitivi = variabili; regole = applicazione dei connettivi.
> - $FOR(\mathcal{L}_0)$ della [[01 - Il calcolo HK (sintassi)|dispensa]] ("nient'altro è una formula" = clausola di minimalità).
> - Le derivazioni (successioni o alberi costruiti applicando regole).

> [!tip] Dimostrazioni per induzione su oggetti definiti induttivamente
> Per provare che **ogni** formula ha una proprietà $P$:
> - **base**: $P$ vale per le variabili;
> - **passo**: se $P$ vale per $A$ e $B$, vale per $\neg A$, $A\land B$, $A\lor B$, $A\to B$.
>
> In pratica è un'induzione sul **numero di connettivi** (grado di complessità). Così è fatto il [[05 - Valutazione canonica e completezza#Lemma 10 — Lemma della valutazione canonica|Lemma 10]]; la correttezza invece è per induzione sulla **lunghezza della derivazione**.
