
## Proofs basics

`rfl` 'spacchetta' le definizioni date performando i calcoli che ritiene rilevanti. Se alla fine le due definizioni si riducono alla stessa espressione, la dimostrazione ha risultato positivo.

`intro x` istanzia una variabile quantificata universalmente. ($\forall$x)

Nota che in `intro a` al posto di 'a' avrei potuto scegliere un nome qualsiasi: non è legato al nome dato nella definizione del teorema.

`simp [f]` o anche `simp` è una tattica che applica una serie di semplificazioni attraverso delle regole standard.

##### Esempi:
``` lean
-- date la funzioni:
def andB (a b : Bool) : Bool :=
  a && b

def norB (a b : Bool) : Bool :=
  !(a||b)


-- intro e rfl
theorem false_and_any_is_false_forall :
  ∀ (a: Bool), andB false a = false := by
  intro a   --- instantiate the universally quantified variable
  rfl

-- simp
theorem and_true_right (b : Bool) : andB b true = b := by
  simp [andB] --- unfold andB and apply known simplification rules for &&
  
-- Metodo alternativo tramite 'case analysis'
theorem and_true_right_cases (b : Bool) : andB b true = b := by
  cases b with
  | false => rfl
  | true  => rfl
  
  
  
-- è possibile farlo più corto?
theorem nor_commutative (a b : Bool) :
    norB a b = norB b a := by
    simp[norB]
    cases a with
    | false => cases b with 
        | true => rfl
        | false => rfl
    | true => cases b with 
        | true => rfl
        | false => rfl
        
-- <;> applica l'operazione finale (in questo caso rfl) a tutti i sottogol generati
theorem nor_commutative2(a b : Bool) : norB a b = norB b a := by 
  simp [norB] 
  cases a <;> cases b <;> rfl

```


## Proofs by semplification

Solitamente ci pensa rfl (?)

Il termine `rfl` è l'abbreviazione del costruttore:  `Eq.refl a : a = a` ed è un tipo induttivo.

## Proofs by rewriting

Dei teoremi noti e le definizioni possono essere utilizzate nella dimostrazione di altri teoremi.
La tattica `rw [h]`, dove h è un'equazione $a=b$ sostituisce a con b nell'espressione target.

esempi:
``` lean 
theorem addN_id : ∀ n m : Nat,
  n = m →
  n + n = m + m := by
  intros n m        --- choose names for universally quantified variables
  intros h          --- move the antecedent of the implication into the hypotheses
  rw [h]            --- rewrite h in the goal using the hypothesis h (left-to-right)

theorem addN_id₂ : ∀ n m : Nat,
  n = m →
  n + n = m + m := by
  intros n m        --- choose names for universally quantified variables
  intros h          --- move the antecedent of the implication into the hypotheses
  rw [<-h]          --- rewrite h in the goal using the hypothesis h (right-to-left)


```


## Proofs by case analysis

In una dimostrazione in cui appare un valore generico `n`, lean non può computare nessun risultato in quando `n` è 'sconosciuto'. Supponendo che `n : Nat` e Nat definito induttivamente come:
``` lean
namespace myNat -- evita di dare errore dato che Nat è un nome già esistente

inductive Nat where 
 | zero : Nat
 | succ : Nat ->  Nat
```

Allora per `n` esistono 2 casi: `n = 0` o `n = k + 1` (con k un numero naturale).

Lean crea quindi un goal per ciascun caso di `n` i quali andranno dimostrati separatamente.

esempio:
``` lean
--date le funzioni:
def leN : Nat → Nat → Bool
  | 0,     _     => true
  | _ + 1, 0     => false
  | n + 1, m + 1 => leN n m
def isZero : Nat → Bool
| 0 => true
| _ => false

-- dimostra che (n<=0) è vero se n = 0

theorem leN_zero_right (n : Nat) : leN n 0 = isZero n := by
  cases n with     -- divide nei vari casi e li dimostra uno ad uno
  | zero   => rfl
  | succ _ => rfl

```

In realtà la tattica `cases` può essere usata anche sulle ipotesi, p.e.:
```lean
/-
Let us prove that `Nat.succ` is injective: two natural numbers with the same
successor are equal.
-/
theorem succN_injective : ∀ n m : Nat, Nat.succ n = Nat.succ m → n = m := by
  intros n m h
  cases h
  rfl
```

L'uguaglianza è in realtà un tipo induttivo con costruttore: `Eq.refl a : a = a`, il termine `rfl` è l'abbreviazione dei questo costruttore.
Quindi quando scriviamo `cases h` lean considera l'unico costruttore possibile per dimostrare l'uguaglianza, ovvero il caso in cui `h` è una prova riflessiva.

## Proofs by exact

*---> Ricorda che le ipotesi sono esse stesse delle prove.*

Consiste nel chiudere la dimostrazione con il termine `exact`.

Prendiamo come esempio la dimostrazione della proprietà transitiva:
``` lean
theorem eq_trans : ∀ a b c : Nat, (a = b) → (b = c) → (a = c) := by
  intros a b c      --- choose names for quantified variables
  intros h1         --- move assumption `a = b` to context
  intros h2         --- move assumption `b = c` to context
  rw [h1]           --- rewrites `h1` in the goal
  exact h2          --- applies `h2` to close the goal
  
  --si può scrivere anche in modo diverso per omettere i quantificatori:
theorem eq_trans₂ (a b c : Nat) (h1 : a = b) (h2 : b = c) : a = c := by
  rw [h1]
  exact h2
  
```

I vari `intros` mettono le ipotesi (a = b) e (b = c) nel contesto: suppone siano vere. Quindi rimane da dimostrare (a = c).
`rw [h1]` dice di prendere in considerazione il fatto che a = b e di riscriverla nel goal.
Il goal passa quindi da $a = c$ a $b = c$. Ora il goal è uguale ad h2.
`exact h2` riesce a concludere perchè ciò che resta da dimostrare ($b = c$) è esattamente l'ipotesi iniziale h2.

In generale `exact t` chiude la dimostrazione se t è una prova dell'obiettivo da dimostrare.
`t` è spesso un ipotesi o un teorema applicato ad un argomento.


Un altro esempio di come utilizzare `exact` con `rw`:
``` lean
theorem eq_trans₃ (a b c : Nat) (h1 : a = b) (h2 : b = c) : a = c := by
  rw [<- h1] at h2
  exact h2
```

`rw [<- h1] at h2` dice: 'rigira h1' ovvero da a=b diventa b=a e riscrivilo in h2 (quindi sostituisci b con a nel termine h2). h2 diventa $a = c$, ovvero il termine da dimostrare.


## Proofs with 'have'
 
La tattica `have` permette di dimostrare fatti intermedi e dargli un nome.
E' utile in quanto è più semplice dividere una dimostrazione in più parti.

esempio:
``` lean 
example (a b c : Nat) (hab : a = b) (hbc : b = c) :
    Nat.succ a = Nat.succ c := by
  have hac : a = c := by    -- prima dimostra il fatto intermedio `a = c`
    rw [hbc] at hab         -- dopo la riscrittura hab diventa 'a = c'
    exact hab               -- fa notare che l'ipotesi corrisponde esattamente ad hab
  cases hac                 -- usa il caso intermedio `hac`.
  rfl
```

La forma generale è:
```
 have name : proposition := by
      proof
```

Lean imposta temporaneamente il goal alla preposizione data da `have`.
Quando questa proposizione viene dimostrata il goal torna come prima.


## Proofs by backward reasoning

Nel secondo metodo dell'esempio si introduce una nuova tattica: `apply`.

`apply t`, dove `t : P -> Q` (p: premessa, q: conclusione/goal), rimpiazza il goal da Q a P.
Si basa sul fatto che P sia una condizione sufficiente per Q, quindi **per dimostrare Q è sufficiente dimostrare P**.
Altri dettagli di `apply t`:
- se t ha più premesse (`t : P1 -> P2 -> Q`) 'apply' creerà un goal per ciascuna premessa, in ordine
- se t non ha premesse `apply t` chiuderà la dimostrazione, agisce come `exact t`
- il goal deve coincidere con la conclusione di t, altrimenti lean darà errore
- solitamente lean capisce da solo gli argomenti da passare a t (nell'esempio successivo saranno comunque esplicitati con 'a' e 'b') 

Partiamo da un esempio:
``` lean 
--utilizziamo il teorema già dimostrato succN_injective
theorem succN_injective : ∀ n m : Nat, Nat.succ n = Nat.succ m → n = m := by
  intros n m h
  cases h
  rfl
/-Nota che non è un equazione ma un implicazione dove:
	- 'Nat.succ n = Nat.succ n' è la premessa
	- 'n = m' è la conclusione
Possiamo usare questo teorema per ragionare 'al contrario': per dimostrare la conclusione è sufficente dimostrare la premessa.

Lean offre due metodi per farlo:
-/

-- Metodo 1: rw con un equazione condizionale

theorem succ_succ_injective (a b : Nat) : a.succ.succ = b.succ.succ → a = b := by
  intro h
  rw [<- succN_injective a b]
  rw [<- succN_injective a.succ b.succ]
  exact h
/-
Dopo intro h abbiamo:
	 h : a.succ.succ = b.succ.succ
     ⊢ a = b

Il teorema succN_injective ha 'a=b' come conclusione, quindi 'rw' la può usare.

Dopo 'rw [<- succN_injective a b]':
'<-' dice di partire da destra verso sinistra quindi b diventa a (in origine è a=b), il goal diventa 'a = a' e viene chiuso in automatico da rw attraverso rfl.

La premessa del teorema diventa il nuovo goal da dimostrare:
	⊢ a.succ = b.succ

Il seconto 'rw' fa la stessa cosa ma ad un 'livello superiore' e diventa:
    ⊢ a.succ.succ = b.succ.succ
che è esattamente l'ipotesi iniziale 'h'.
Quindi chiude con 'exact h'
-/

-- Metodo 2: 'apply'

theorem succ_succ_injective₂ (a b : Nat) : a.succ.succ = b.succ.succ → a = b := by
  intro h
  apply succN_injective a b
  apply succN_injective a.succ b.succ
  exact h

/-
dopo intro h:
        h : a.succ.succ = b.succ.succ
        ⊢ a = b
dopo apply succN_injective a b:
        ⊢ a.succ = b.succ              -- la premessa

    after apply succN_injective a.succ b.succ:
        ⊢ a.succ.succ = b.succ.succ    -- ancora la premessa

L'ultimo goal è esattamente `h`, quindi `exact h` chiude la dimostrazione.
-/


```