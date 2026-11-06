## tags: [istituzioni-di-logica, modulo-2, lezione-1] fonti: Programma 25/26; Completezza 2.0 §1.1–1.2; Possibili domande d'esame 24/25

# Lezione 1 — Induzione, linguaggio proposizionale, semantica

> [!info] Dove siamo nel programma Questa lezione copre la base di tutto il resto dei moduli 2 e 3:
> 
> - **Nozioni preliminari**: definizioni e dimostrazioni per induzione.
> - **Logica proposizionale**: linguaggio, formule, valutazioni, tautologie, conseguenza logica (parte di _Percorsi di logica_ pp. 57–80 e §1.1–1.2 della dispensa _Completezza 2.0_).
> 
> **Piano delle lezioni successive**
> 
> 1. ⬅ _Induzione, linguaggio, semantica_ (questa)
> 2. Il calcolo HK: dimostrazioni, derivazioni, teorema di deduzione
> 3. Deduzione naturale ND e forma normale
> 4. Calcolo delle sequenze PK
> 5. Eliminazione dell'identità in PK + equivalenza ND ⇔ PK
> 6. Correttezza di HK
> 7. Completezza di HK (consistenza, Lindenbaum, valutazione canonica)

> [!tip] Come leggere le stime di probabilità Le stelle sono **mie stime**, basate su due cose: (1) se l'argomento compare nell'elenco ufficiale _Possibili domande d'esame 24/25_; (2) quanto serve per rispondere alle domande che compaiono lì.
> 
> - ★★★ **Alta** — domanda esplicitamente in elenco, o indispensabile per una domanda in elenco
> - ★★ **Media** — non in elenco, ma usata dentro dimostrazioni che lo sono; può uscire come sotto-domanda
> - ★ **Bassa** — utile per capire, difficilmente chiesta da sola

---

## 1. Definizioni e dimostrazioni per induzione

> [!warning] Esame — ★★★ come _strumento_, ★ come domanda a sé Nessuno ti chiederà "che cos'è l'induzione?" in astratto. Ma nell'elenco c'è **"Dimostri l'equivalenza tra ND e PK"**, con il risultato atteso: _"sa gestire la struttura induttiva della dimostrazione"_. E per induzione si dimostrano anche l'eliminazione dell'identità in PK, la correttezza di HK e il lemma della valutazione canonica. Se sbagli la struttura dell'induzione, sbagli tutte queste.

### 1.1 Definizione induttiva

Un insieme è definito induttivamente quando si danno:

1. **clausola di base**: alcuni oggetti appartengono all'insieme;
2. **clausole induttive**: se certi oggetti appartengono all'insieme, ci appartengono anche altri oggetti costruiti a partire da essi;
3. **clausola di chiusura**: _nient'altro_ appartiene all'insieme.

La clausola di chiusura è quella che rende possibile l'induzione: garantisce che ogni elemento sia stato costruito in un numero finito di passi a partire dalla base.

### 1.2 Dimostrazione per induzione

Per dimostrare che **tutti** gli elementi di un insieme definito induttivamente hanno una proprietà $P$:

- **Base**: gli elementi della clausola di base hanno $P$.
- **Passo induttivo**: si assume che $P$ valga per gli oggetti "più semplici" (**ipotesi induttiva**) e si dimostra che vale per quelli costruiti a partire da essi.

Nei testi del corso si induce quasi sempre su un **numero**:

|Dimostrazione|Si induce su…|
|---|---|
|Lemma di coincidenza, valutazione canonica, identità in PK|numero di **connettivi** della formula|
|Correttezza di HK|**lunghezza** della derivazione|
|Equivalenza ND ⇔ PK|numero di **vertici** dell'albero|

> [!important] Il dettaglio che fa la differenza: induzione "forte" Nelle dispense l'ipotesi induttiva è sempre del tipo: _"l'enunciato vale per formule con **al massimo** $m$ connettivi"_, e si dimostra per formule con $m+1$ connettivi. Perché "al massimo"? Se $\alpha = \beta \land \gamma$ ha $m+1$ connettivi, allora $\beta$ e $\gamma$ ne hanno **insieme** $m$, quindi **ciascuna al massimo** $m$ — non necessariamente esattamente $m$. Con un'ipotesi "esattamente $m$" il passo non funzionerebbe. All'orale, dire esplicitamente questo è un ottimo segnale di comprensione.

---

## 2. Il linguaggio $\mathcal{L}_0$ e l'insieme FOR

> [!warning] Esame — ★★ La definizione di FOR non compare come domanda in elenco, ma è la premessa di ogni enunciato. Saperla dire con precisione è richiesto implicitamente ("capacità di definire in modo rigoroso i concetti").

**Simboli di $\mathcal{L}_0$**

- _descrittivi_: un insieme numerabile di variabili ${p_1, p_2, \dots}$
- _logici_: $\neg$ (unario), $\land, \lor, \to$ (binari)
- _ausiliari_: parentesi $($ e $)$

**Definizione (FOR($\mathcal{L}_0$))**

- ogni variabile $p_i$ è una formula;
- se $\alpha, \beta$ sono formule, allora $\neg\alpha$, $\alpha\land\beta$, $\alpha\lor\beta$, $\alpha\to\beta$ sono formule;
- nient'altro è una formula.

**Convenzione di lettura**: $\neg$ lega più di tutti; $\land$ e $\lor$ legano più di $\to$. Quindi $\alpha\land\beta\to\gamma$ si legge $(\alpha\land\beta)\to\gamma$. Per $\to$ annidate userò sempre le parentesi.

**Nozioni collegate**

- **Connettivo principale**: quello applicato per ultimo nella costruzione della formula. È quello su cui si fanno i "casi" nel passo induttivo.
- **Sottoformule**: la formula stessa, più (ricorsivamente) le sottoformule dei suoi componenti immediati.
- **Metavariabili**: $\alpha, \beta, \gamma$ non sono formule del linguaggio, ma variabili del metalinguaggio che stanno per formule qualsiasi. Ti servirà nella lezione 2: gli assiomi di HK sono _schemi_.

> [!note] Perché FOR è numerabile Ogni formula è una stringa finita su un alfabeto numerabile. Il fatto è usato nel **Lemma di Lindenbaum** (modulo 3), che parte da un'enumerazione $\alpha_1, \alpha_2, \dots$ di FOR. ★★ come sotto-domanda: "perché questa enumerazione esiste?"

---

## 3. Semantica: valutazioni

> [!warning] Esame — ★★★ La nozione di valutazione entra nella definizione di conseguenza logica, quindi negli enunciati di correttezza e completezza (modulo 3). La valutazione canonica è _il_ cuore della completezza.

**Definizione (valutazione)**. Una valutazione di $\mathcal{L}_0$ è una funzione $v: \text{FOR}(\mathcal{L}_0) \to {0,1}$ tale che:

- $v(\neg\alpha)=1$ sse $v(\alpha)=0$
- $v(\alpha\land\beta)=1$ sse $v(\alpha)=1$ **e** $v(\beta)=1$
- $v(\alpha\lor\beta)=1$ sse $v(\alpha)=1$ **o** $v(\beta)=1$
- $v(\alpha\to\beta)=1$ sse $v(\alpha)=0$ **o** $v(\beta)=1$

**Due fatti da sapere (e da saper giustificare per induzione)**

1. _Estensione unica._ Ogni assegnazione di valori alle **sole variabili** si estende in uno e un solo modo a una valutazione su tutto FOR. La dispensa lo usa nel lemma della valutazione canonica: si definisce $v$ sulle variabili ($v(p)=1$ sse $p\in\Delta$) e poi _"la possiamo estendere a tutto FOR nel modo consueto"_. ★★
2. _Lemma di coincidenza._ Il valore $v(\alpha)$ dipende solo dai valori delle variabili che compaiono in $\alpha$. È ciò che giustifica le tavole di verità: una riga per ogni assegnazione alle variabili di $\alpha$, cioè $2^n$ righe. ★ come domanda, ma è un ottimo esercizio di induzione (vedi Esercizio 9).

> [!note] Implicazione materiale La quarta clausola rende $\alpha\to\beta$ vera ogni volta che l'antecedente è falso o il conseguente è vero. È la fonte dei "paradossi dell'implicazione materiale" (domanda in elenco, ma del **modulo 1**). Ti basta sapere che $\beta\to(\alpha\to\beta)$ e $\neg\alpha\to(\alpha\to\beta)$ sono tautologie, e che sono proprio gli schemi A1 e A10 di HK usati nella completezza.

---

## 4. Tavole di verità

> [!warning] Esame — ★★★ In elenco c'è **"Fare la tavola di verità di $\neg A\to(B\lor\neg A)$"**, con la nota: _si guarda più al processo che alla colonna finale_. Formalmente rimanda al libro del modulo 1, ma è lo strumento con cui verifichi che gli assiomi di HK sono tautologie (passo base della correttezza).

**Procedura da _raccontare_ all'esame**

1. Individuo le variabili ($n$) → $2^n$ righe.
2. Scompongo la formula nelle sue sottoformule, dalla più semplice alla più complessa (lo stesso ordine della definizione induttiva).
3. Calcolo ogni colonna applicando la clausola del connettivo principale di quella sottoformula.
4. Leggo l'ultima colonna: tutti 1 → **tautologia**; tutti 0 → **contraddizione**; misto → né l'una né l'altra (ma soddisfacibile).

**Esempio svolto: $\neg A\to(B\lor\neg A)$**

|$A$|$B$|$\neg A$|$B\lor\neg A$|$\neg A\to(B\lor\neg A)$|
|:-:|:-:|:-:|:-:|:-:|
|1|1|0|1|**1**|
|1|0|0|0|**1**|
|0|1|1|1|**1**|
|0|0|1|1|**1**|

È una tautologia. Commento da fare a voce: nelle prime due righe l'antecedente è falso, quindi l'implicazione è vera "a vuoto"; nelle ultime due $\neg A$ è vero, quindi lo è anche il disgiunto $B\lor\neg A$.

**Metodo rapido (riduzione all'assurdo)**. Invece di 2ⁿ righe, cerco una valutazione che renda la formula **falsa**. Se arrivo a una contraddizione, è una tautologia. Qui: $v(\neg A\to(B\lor\neg A))=0$ richiede $v(\neg A)=1$ e $v(B\lor\neg A)=0$; ma quest'ultimo richiede $v(\neg A)=0$. Contraddizione. Ti conviene saperli fare entrambi: il metodo rapido è comodissimo per gli assiomi di HK.

---

## 5. Tautologie, soddisfacibilità, conseguenza logica

> [!warning] Esame — ★★★ La definizione di conseguenza logica è il lato semantico del **teorema di adeguatezza** (modulo 3). Aspettati di doverla dire "con tutti i quantificatori al posto giusto", come chiede l'elenco per la definizione di argomento valido.

**Definizioni**

- $\alpha$ è una **tautologia** sse $v(\alpha)=1$ per **ogni** valutazione $v$.
- $\alpha$ è una **contraddizione** sse $v(\alpha)=0$ per ogni $v$.
- $\Gamma$ è **soddisfacibile** sse **esiste** $v$ con $v(\gamma)=1$ per ogni $\gamma\in\Gamma$.
- **Conseguenza logica**: $\Gamma\vDash\alpha$ sse **per ogni** valutazione $v$, **se** $v(\gamma)=1$ per ogni $\gamma\in\Gamma$, **allora** $v(\alpha)=1$. Forma equivalente: **non esiste** $v$ tale che $v(\gamma)=1$ per ogni $\gamma\in\Gamma$ e $v(\alpha)=0$.
- **Equivalenza logica**: $\alpha\equiv\beta$ sse $v(\alpha)=v(\beta)$ per ogni $v$.

> [!important] Ordine dei quantificatori "Per ogni $v$, se $v$ rende vere tutte le premesse allora rende vera $\alpha$" ≠ "se esiste una $v$ che rende vere le premesse, allora $v(\alpha)=1$". La prima è la definizione giusta. Ripetila finché non ti esce automatica.

**Fatti da saper dimostrare** (tutti in pochi passaggi, tutti usati nella completezza):

|Fatto|Uso nel corso|Prob.|
|---|---|:-:|
|$\vDash\alpha$ (cioè $\emptyset\vDash\alpha$) sse $\alpha$ è tautologia|teoremi di HK = tautologie|★★★|
|se $\alpha$ è tautologia, $\Gamma\vDash\alpha$ per ogni $\Gamma$|base della correttezza (assiomi)|★★★|
|se $\alpha\in\Gamma$ allora $\Gamma\vDash\alpha$|base della correttezza (assunzioni)|★★★|
|se $\Gamma\vDash\alpha$ e $\Gamma\vDash\alpha\to\beta$ allora $\Gamma\vDash\beta$|passo induttivo della correttezza (MP)|★★★|
|$\Gamma\vDash\alpha$ sse $\Gamma\cup{\neg\alpha}$ è insoddisfacibile|idea della completezza (Teorema 11)|★★★|
|$\Gamma\cup{\alpha}\vDash\beta$ sse $\Gamma\vDash\alpha\to\beta$|analogo semantico del teorema di deduzione|★★|

---

## 6. Esercizi

> [!example] Esercizio 1 — Formule ★★ Quali di queste stringhe sono formule di $\mathcal{L}_0$? Per quelle che lo sono, indica il connettivo principale. a) $(p_1\land p_2)\to\neg p_3$ b) $p_1\neg\land p_2$ c) $\neg\neg p_1$ d) $p_1\land p_2\to p_3\lor p_4$ e) $(p_1\to)$

> [!success]- Soluzione a) Formula; principale $\to$. b) Non è una formula: $\neg$ è unario e va davanti a una formula, non tra due. c) Formula; principale il $\neg$ più esterno. d) Formula, per la convenzione: $(p_1\land p_2)\to(p_3\lor p_4)$; principale $\to$. e) Non è una formula: $\to$ è binario e manca il secondo argomento. In ogni caso la giustificazione è "ricostruisco la formula a partire dalle variabili usando le clausole di FOR; se non ci riesco, per la clausola di chiusura non è una formula".

> [!example] Esercizio 2 — Sottoformule e connettivi ★★ Elenca le sottoformule di $\neg(p\land q)\to(\neg p\lor\neg q)$ e conta i connettivi.

> [!success]- Soluzione Sottoformule (8): $p$, $q$, $p\land q$, $\neg(p\land q)$, $\neg p$, $\neg q$, $\neg p\lor\neg q$, la formula intera. Connettivi (6): $\neg, \land, \to, \neg, \neg, \lor$. È la legge di De Morgan che dovrai dimostrare in PK (domanda in elenco: $\neg(A\land B)\Rightarrow\neg A\lor\neg B$).

> [!example] Esercizio 3 — Tavola di verità ★★★ Fai la tavola di verità di $(p\to q)\to(\neg q\to\neg p)$ e spiega il procedimento come se fossi all'orale.

> [!success]- Soluzione
> 
> |$p$|$q$|$p\to q$|$\neg q$|$\neg p$|$\neg q\to\neg p$|tot|
> |:-:|:-:|:-:|:-:|:-:|:-:|:-:|
> |1|1|1|0|0|1|1|
> |1|0|0|1|0|0|1|
> |0|1|1|0|1|1|1|
> |0|0|1|1|1|1|1|
> 
> Tautologia (contrapposizione). Metodo rapido: per falsificarla serve $v(p\to q)=1$ e $v(\neg q\to\neg p)=0$, cioè $v(q)=0$ e $v(p)=1$; ma allora $v(p\to q)=0$. Contraddizione.

> [!example] Esercizio 4 — Gli assiomi di HK sono tautologie ★★★ Con il metodo rapido, verifica che sono tautologie: A1: $\alpha\to(\beta\to\alpha)$ A8: $(\alpha\to\gamma)\to((\beta\to\gamma)\to(\alpha\lor\beta\to\gamma))$ A10: $\alpha\to(\neg\alpha\to\beta)$ _(È esattamente l'"esercizio" lasciato aperto nel passo base del Teorema di correttezza.)_

> [!success]- Soluzione **A1**. Falsa sse $v(\alpha)=1$ e $v(\beta\to\alpha)=0$; quest'ultimo richiede $v(\alpha)=0$. Contraddizione. **A8**. Falsa sse $v(\alpha\to\gamma)=1$, $v(\beta\to\gamma)=1$, $v(\alpha\lor\beta\to\gamma)=0$. Dall'ultima: $v(\gamma)=0$ e $v(\alpha\lor\beta)=1$. Con $v(\gamma)=0$, le prime due danno $v(\alpha)=0$ e $v(\beta)=0$, quindi $v(\alpha\lor\beta)=0$. Contraddizione. **A10**. Falsa sse $v(\alpha)=1$ e $v(\neg\alpha\to\beta)=0$; quest'ultimo richiede $v(\neg\alpha)=1$, cioè $v(\alpha)=0$. Contraddizione. Nota: qui $\alpha,\beta,\gamma$ sono metavariabili, ma il ragionamento vale per qualsiasi formula sostituita, perché usa solo le clausole delle valutazioni. Ecco perché basta controllare lo _schema_.

> [!example] Esercizio 5 — Conseguenza logica ★★★ Stabilisci se valgono, giustificando con la definizione: a) ${I\to E}\vDash\neg(I\land\neg E)$ b) ${A\to B,\ \neg A}\vDash\neg B$ c) ${p\to q,\ q}\vDash p$

> [!success]- Soluzione a) **Sì.** Se $v(I\to E)=1$, o $v(I)=0$ (allora $v(I\land\neg E)=0$), o $v(E)=1$ (allora $v(\neg E)=0$ e di nuovo $v(I\land\neg E)=0$). In entrambi i casi $v(\neg(I\land\neg E))=1$. (È l'argomento su Parigi dell'elenco.) b) **No.** Controesempio: $v(A)=0$, $v(B)=1$. Premesse vere, conclusione falsa. È la fallacia della **negazione dell'antecedente** (l'argomento su Anna e l'ombrello). c) **No.** Controesempio: $v(p)=0$, $v(q)=1$. Fallacia dell'**affermazione del conseguente**. Per mostrare che una conseguenza _non_ vale basta **una** valutazione controesempio; per mostrare che vale bisogna ragionare su **tutte**.

> [!example] Esercizio 6 — Fatti di base ★★★ Dimostra: a) se $\alpha$ è una tautologia, allora $\Gamma\vDash\alpha$ per ogni $\Gamma$; b) se $\alpha\in\Gamma$, allora $\Gamma\vDash\alpha$; c) se $\Gamma\vDash\alpha$ e $\Gamma\vDash\alpha\to\beta$, allora $\Gamma\vDash\beta$.

> [!success]- Soluzione a) Sia $v$ qualsiasi con $v(\gamma)=1$ per ogni $\gamma\in\Gamma$. Poiché $\alpha$ è tautologia, $v(\alpha)=1$ comunque. Quindi $\Gamma\vDash\alpha$. b) Sia $v$ con $v(\gamma)=1$ per ogni $\gamma\in\Gamma$; in particolare, essendo $\alpha\in\Gamma$, $v(\alpha)=1$. c) Sia $v$ con $v(\gamma)=1$ per ogni $\gamma\in\Gamma$. Per le ipotesi, $v(\alpha)=1$ e $v(\alpha\to\beta)=1$. Per la clausola di $\to$, da $v(\alpha\to\beta)=1$ segue $v(\alpha)=0$ o $v(\beta)=1$; il primo caso è escluso, quindi $v(\beta)=1$. **Questi tre punti, messi insieme, sono la dimostrazione del Teorema di correttezza di HK** (a e b = base; c = passo induttivo per R1). Nella lezione 6 li ritroverai identici.

> [!example] Esercizio 7 — Conseguenza e insoddisfacibilità ★★★ Dimostra: $\Gamma\vDash\alpha$ sse $\Gamma\cup{\neg\alpha}$ non è soddisfacibile.

> [!success]- Soluzione ($\Rightarrow$) Supponi per assurdo che esista $v$ che rende vere tutte le formule di $\Gamma\cup{\neg\alpha}$. Allora $v$ rende vero tutto $\Gamma$, quindi (da $\Gamma\vDash\alpha$) $v(\alpha)=1$; ma anche $v(\neg\alpha)=1$, cioè $v(\alpha)=0$. Contraddizione. ($\Leftarrow$) Per contrapposizione: se $\Gamma\nvDash\alpha$, esiste $v$ con $v(\gamma)=1$ per ogni $\gamma\in\Gamma$ e $v(\alpha)=0$, cioè $v(\neg\alpha)=1$. Quindi $v$ soddisfa $\Gamma\cup{\neg\alpha}$. Confronta con il **Teorema 11** (completezza): lì si parte da $\Gamma\nvdash\alpha$, si ottiene che $\Gamma\cup{\neg\alpha}$ è _consistente_, e si costruisce una valutazione che lo soddisfa. È lo stesso schema, con "consistente" al posto di "soddisfacibile".

> [!example] Esercizio 8 — Deduzione semantica ★★ Dimostra: $\Gamma\cup{\alpha}\vDash\beta$ sse $\Gamma\vDash\alpha\to\beta$.

> [!success]- Soluzione ($\Rightarrow$) Sia $v$ con $v(\gamma)=1$ per ogni $\gamma\in\Gamma$. Se $v(\alpha)=0$, allora $v(\alpha\to\beta)=1$. Se $v(\alpha)=1$, $v$ rende vero tutto $\Gamma\cup{\alpha}$, quindi $v(\beta)=1$ e di nuovo $v(\alpha\to\beta)=1$. ($\Leftarrow$) Sia $v$ che rende vero tutto $\Gamma\cup{\alpha}$. Da $\Gamma\vDash\alpha\to\beta$: $v(\alpha\to\beta)=1$; con $v(\alpha)=1$ segue $v(\beta)=1$. È la controparte semantica del **Teorema di deduzione** per HK (Theorem 1 della dispensa, enunciato senza dimostrazione).

> [!example] Esercizio 9 — Induzione sui connettivi ★★ (allenamento alla struttura) Dimostra per induzione sul numero di connettivi: se due valutazioni $v, v'$ coincidono su tutte le variabili che compaiono in $\alpha$, allora $v(\alpha)=v'(\alpha)$.

> [!success]- Soluzione **Base** ($n=0$): $\alpha$ è una variabile $p$; per ipotesi $v(p)=v'(p)$. **Ipotesi induttiva**: l'enunciato vale per tutte le formule con **al massimo** $m$ connettivi. **Passo** ($m+1$ connettivi). Casi sul connettivo principale:
> 
> - $\alpha=\neg\beta$: $\beta$ ha $m$ connettivi e le sue variabili sono quelle di $\alpha$, quindi per i.i. $v(\beta)=v'(\beta)$. Allora $v(\neg\beta)=1$ sse $v(\beta)=0$ sse $v'(\beta)=0$ sse $v'(\neg\beta)=1$.
> - $\alpha=\beta\circ\gamma$ con $\circ\in{\land,\lor,\to}$: $\beta$ e $\gamma$ hanno **ciascuna al massimo** $m$ connettivi, e le loro variabili sono tra quelle di $\alpha$. Per i.i. $v(\beta)=v'(\beta)$ e $v(\gamma)=v'(\gamma)$. Poiché il valore di $\beta\circ\gamma$ dipende solo dai valori di $\beta$ e $\gamma$ (clausola di $\circ$), $v(\alpha)=v'(\alpha)$.
> 
> **Struttura identica** a: Lemma 10 (valutazione canonica), Teorema 1 della dispensa _Calcoli_ ($\vdash_{PK}\alpha\Rightarrow\alpha$). Quando studierai quelle, riconoscerai lo stesso scheletro.

> [!example] Esercizio 10 — Induzione, variante ★ Dimostra per induzione che in ogni formula il numero di occorrenze di variabili è uguale al numero di connettivi **binari** più uno.

> [!success]- Soluzione Scrivi $\text{var}(\alpha)$ e $\text{bin}(\alpha)$. **Base**: $\alpha=p$: $\text{var}=1$, $\text{bin}=0$. ✓ **Passo**: $\alpha=\neg\beta$: né le variabili né i connettivi binari cambiano; vale per i.i. $\alpha=\beta\circ\gamma$: $\text{var}(\alpha)=\text{var}(\beta)+\text{var}(\gamma)=(\text{bin}(\beta)+1)+(\text{bin}(\gamma)+1)=(\text{bin}(\beta)+\text{bin}(\gamma)+1)+1=\text{bin}(\alpha)+1$. ✓

---

## 7. Riepilogo per l'esame

|Argomento|Prob.|Perché|Cosa devi saper fare|
|---|:-:|---|---|
|Definizione di conseguenza logica|★★★|lato semantico dell'adeguatezza (mod. 3)|enunciarla con i quantificatori giusti, anche nella forma "non esiste $v$…"|
|Tavole di verità / metodo rapido|★★★|domanda in elenco; assiomi di HK|eseguirle **spiegando** il procedimento|
|Valutazione (def.)|★★★|conseguenza logica, valutazione canonica|dare le quattro clausole|
|Fatti di base su $\vDash$ (Es. 6–7)|★★★|sono i pezzi di correttezza e completezza|dimostrarli in 2–3 righe|
|Struttura delle dimostrazioni per induzione|★★★ (come strumento)|ND⇔PK, identità in PK, correttezza, val. canonica|base, i.i. "al massimo $m$", casi sul connettivo principale|
|Definizione di FOR, connettivo principale|★★|premessa di tutto|darla con la clausola di chiusura|
|Estensione unica di una valutazione|★★|usata nel Lemma 10|sapere cosa dice e dove serve|
|Deduzione semantica (Es. 8)|★★|analogo del teorema di deduzione|dimostrarla|
|Numerabilità di FOR|★★ (sotto-domanda)|Lemma di Lindenbaum|giustificarla in una frase|
|Lemma di coincidenza|★|giustifica le tavole|usarlo come esercizio di induzione|

> [!todo] Prima della lezione 2
> 
> - Rifai gli Esercizi 4, 6 e 9 senza guardare le soluzioni.
> - Verifica col metodo rapido che anche A2, A5, A9 e A11 sono tautologie.
> - Ripeti a voce, senza leggere, la definizione di $\Gamma\vDash\alpha$.