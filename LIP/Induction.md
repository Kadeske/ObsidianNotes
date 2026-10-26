
vecchie definizioni utilizzate:
``` lean session=induction
def addN (n m : Nat) : Nat :=
  match m with
  | 0      => n
  | m' + 1 => Nat.succ (addN n m')

theorem addN_succ_right (n m : Nat) : addN n (m + 1) = (addN n m) + 1 := by rfl


theorem addN_zero (n : Nat) : addN n 0 = n := by
  rfl

theorem succ_addN_zero : ∀ n : Nat, Nat.succ (addN n 0) = Nat.succ n := by
  intros n          --- chooses a name for the quantified variable
  rw [addN_zero]    --- addN n 0 = n

```

La ricorsione (e tutti gli altri metodi visti) non sono sufficienti per alcune cose, es: non posso dimostrare `addN 0 n = n`. dato che n ha due casi: 0 e k+1 (con k : Nat). 

Supponiamo di voler dimostrare una proprietà `P n` per tutti i numeri naturali `n`.
Il principio di induzione dice che è sufficiente dimostrare:
- `P 0`              caso base
- `P n -> P (n+1)`   caso induttivo

Il caso induttivo dice che se la proprietà è valida per un `n` arbitrario, allora è valida anche per il suo successore.

In Lean si sfruttano questi due casi. Viene introdotta anche la 'induction hypotesis' scritta come `ih`.

esempio:
``` lean session=induction
theorem zero_addN (n : Nat) : addN 0 n = n := by
  induction n with
  | zero => rfl               -- caso base: 'addN 0 0 = 0' rfl lo chiude facilmente
  | succ n' ih =>             -- caso induttivo: 'addN 0 (n' + 1) = n' + 1'
      rw [addN_succ_right]    -- addN 0 (n' + 1) -> (addN 0 n') + 1
      rw [ih]

```

Queste dimostrazioni hanno una forte correlazione con la definizione che intendono dimostrare:
```
	PROGRAMMING                  PROVING
    ------------------------------------------------
    match n with ...             cases n with ...
    recursive call on n'         induction hypothesis about n'
    base case                    base case
    recursive case               inductive case
    
La dimostrazione ha la stessa forma della definizione ricorsiva di cui parla
```

### Scelta della variabile induttiva

Una regola utile da ricordare è:
--> Guarda la definizione bloccata, usa l'induzione sull'argomento in cui la definizione esegue la ricorsione.

``` lean session=induction
theorem addN_succ_left (n m : Nat) : addN (n + 1) m = addN n m + 1 := by
  induction m with
  | zero => rfl
  | succ m' ih =>             -- 'addN (n + 1) (m' + 1) = n (m' + 1) + 1'
    rw [addN_succ_right]      -- addN_succ_right: toglie le parentesi da (m' + 1) a SINISTRA
    rw [addN_succ_right]      -- addN_succ_right: toglie le parentesi da (m' + 1) a DESTRA
    rw [ih]
    
-- avrei potuto scrivere anche '| succ m' ih => rw [addN_succ_right,addN_succ_right,ih]'

```


altro esempio: commutatività della funzione addN
``` lean session=induction
theorem addN_comm (n m : Nat) : addN n m = addN m n := by
  induction m with
  | zero =>
    rw [addN_zero]
    rw [zero_addN]
  | succ m' ih =>
    rw [addN_succ_right]
    rw [addN_succ_left]
    rw [ih]
```

## Espressioni booleane (induzione sui booleani)

La forma di una dimostrazione induttiva è data dalla forma della definizione del tipo.
Per `Nat` per esempio la successione è: `0, succ 0, succ (succ 0), ...`.

La sintassi delle 'Boolean expressions' è `t ::= true | false | if t₀ then t₁ else t₂` 
Questo è un 'tree-shaped datatype', l'espressione if contiene alberi più piccoli.

``` lean session=induction
inductive BExp where
  | btrue  : BExp
  | bfalse : BExp
  | bifte  : BExp → BExp → BExp → BExp
  deriving Repr
```

La funzione `evalBExp` valuta un espressione booleana tramite induzione strutturale e segue l'andamento della Big-Step.

Regole per scrivere `t => v` dove t è un espressione booleana e v è un valore:
```
-----------------          ------------------
  btrue ⇒ true               bfalse ⇒ false

  t0 ⇒ true    t1 ⇒ v          t0 ⇒ false    t2 ⇒ v
------------------------      -----------------------
 bifte t0 t1 t2 ⇒ v             bifte t0 t1 t2 ⇒ v
```

Implementata in lean diventa:
``` lean session=induction
def evalBExp : BExp → Bool
  | .btrue          => true
  | .bfalse         => false
  | .bifte t0 t1 t2 => if evalBExp t0 then evalBExp t1 else evalBExp t2

-- esempio di if:
example :
    let ex0 := .bifte (.bifte .bfalse .btrue .bfalse) .btrue .bfalse
    evalBExp ex0 = false := by rfl

```

Un altra funzione strutturalmente ricorsiva. Immaginala come un 'not' con la particolarità che, dato un `if t0 then t1 else t2` lo trasforma in `if t0 then t2 else t1`. (scambia i rami then else tra loro).
``` lean session=induction 
def dualBExp : BExp → BExp
  | .btrue        => .bfalse
  | .bfalse       => .btrue
  | .bifte t0 t1 t2 =>
      .bifte (dualBExp t0) (dualBExp t2) (dualBExp t1)
```

Applicare 2 volte `dualBExp` restituisce l'espressione di origine. Dimostrazione per induzione su t:
``` lean session=induction

/-Nota che bifte ha 3 termini ricorsivi, quindi serviranno 3 ipotesi induttive:
    ih0 : dualBExp (dualBExp t0) = t0
    ih1 : dualBExp (dualBExp t1) = t1
    ih2 : dualBExp (dualBExp t2) = t2
-/

theorem dualBExp_involutive (t : BExp) : dualBExp (dualBExp t) = t := by
  induction t with
  | btrue   => rfl
  | bfalse  => rfl
  | bifte t0 t1 t2 ih0 ih1 ih2 =>
      rw [dualBExp]         -- unfold the outer application of dualBExp
      rw [dualBExp]         -- unfold the remaining application
      rw [ih0, ih1, ih2]    -- applies the three induction hypotheses

```

`