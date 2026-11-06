Da ora le funzioni stesse sono valori.
le funzioni che consumano o producono altre funzioni sono dette 'higher-order functions'

obiettivi:
* read function types containing several arrows and parentheses;
* remember that arrows associate to the right and applications to the left;
* write anonymous functions with `fun`;
* use partial application;
* define functions that accept or return other functions;
* use composition and iteration to build new computations;
* prove pointwise properties and equalities between functions;
* explain the role of every argument of a higher-order function.

Parliamo di una grammatica scritta come: 
```
    T ::= Bool | Nat | T₁ → T₂
```

(Lean ha molte più cose ma per ora non le usiamo)

Se A e B sono tipi, allora `A -> B` prende A in input e restituisce B.
A: dominio, B: codominio

**creazione di funzioni:** Se un espressione 'e' ha tipo B e 'x' ha tipo A, 
allora `fun x : A => e` ha tipo `A -> B`.

**applicazione di funzioni**: se `f A -> B` e `a : A` allora `f a : B`.
Il tipo dell'argomento (a) deve essere lo stesso del dominio della funzione su cui è applicato.

Le freccie associano a **destra**, quindi 
`Nat -> Nat -> Nat` diventa `Nat -> (Nat -> Nat)` NON `(Nat -> Nat) -> Nat`

L'applicazione di funzioni associa a **sinistra**, quindi:
`addN 5 3` significa `(addN 5) 3`


## Funzioni anonime 

Una funzione non ha bisogno di un nome. 
L'espressione `fun n : Nat => n + 1` è una funzione anonima, detta anche **lambda abstraction**.

es: ` (fun n : Nat => n + 1) 5` = 6

possiamo anche dare nomi a delle funzioni anonime:
`def incr := fun n : Nat => n + 1`

Una f anonima può usare variabili del contesto:
```lean
def incrBy (k : Nat) : Nat → Nat := fun n => n + k
```



## Applicazioni parziali

Un applicazione è parziale quando il suo output è una funzione.

p.e. `def addFive : Nat → Nat := addN 5` restituisce una funzione `Nat -> Nat`.

Una funzione, essendo un valore, può essere utilizzata dai condizionali (if).
p.e:
``` lean
def chooseN (s : Bool) (f g : Nat → Nat) : Nat → Nat :=
  if s then f else g
```

## Composizione di funzioni

Date due funzioni
```
f : Nat → Nat
g : Nat → Nat
```
possiamo costruire una funzione che applica prima `g` e poi applica `f` sul risultato di `g`.

``` lean
def composeN (f g : Nat → Nat) : Nat → Nat :=
  fun n => f (g n)
```

Lean ha un operatore built-in per la composizione: `∘` (scritto '\comp') 
``` lean
example : ((fun n : Nat => n + 1) ∘ (fun n : Nat => n * 2)) 5 = 11 := by rfl
```

## Function extensionality

Per provare che due funzioni sono identiche è sufficiente dimostrare che restituiscano gli stessi valori per ogni input.
`(∀ x, f x = g x) → f = g`

La tattica `funext` fa esattamente questo.

Quando il goal è un’uguaglianza **tra funzioni**, `funext n` fa due cose:

1. introduce un input arbitrario `n`, come fa `intro`;
2. cambia il goal da `f = g` a `f n = g n`.

Guarda [[10. funext]] per un esempio di dimostrazione.

## State transformer

Una funzione può essere vista come un piccolo programma che modifica uno stato.
Prendiamo per esempio il bilancio di una banca, queste funzioni ricordano le transazioni precedenti aggiornando il conto:

``` lean
def deposit (amount : Nat) : Nat → Nat :=
  fun balance => balance + amount

def withdraw (amount : Nat) : Nat → Nat :=
  fun balance => balance - amount

example : deposit 20 100 = 120 := by rfl
example : withdraw 7 100 = 93  := by rfl
```

## Itera N volte

La composizione usa un numero finito e statico di passi.
Con 'itera' possiamo dire di iterare 'k' volte una funzione 'f'.

la funzione `iterateN` prende in input 3 cose:
- `f`: la funzione che dice cosa fa un passo
- `k`: dice quanti passi fare
- `n`: lo stato iniziale
``` lean 
def iterateN (f : Nat → Nat) (k n : Nat) : Nat :=
  match k with
  | 0      => n                     -- applying a function zero times leaves the starting value unchanged
  | k' + 1 => f (iterateN f k' n)
```

