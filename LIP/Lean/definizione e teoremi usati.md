## 1. Booleani

```lean
-- Definizioni [1]
negB (b) := !b                     negIf, negMatch, negMatch₂, negLambda, negLambda₂   (varianti)
andB (a b) := a && b               andMatch
orB (a b) := a || b
impB (a b) := (!a) || b            impIf, impMatch
xorB (a b) := (!a && b) || (a && !b)   xorIf, xorMatch
nandB (a b) := !andB a b           nandMatch
norB (a b) := !(a || b)
majority (a b c)                   -- vera se almeno due true
allThree (a b c) := a && b && c    allThreeMatch
mux2 (s0 a b) := (!s0 && b) || (s0 && a)    mux2β (con if)
mux4 (s0 s1 a0 a1 a2 a3) := mux2 s0 (mux2 s1 a3 a2) (mux2 s1 a1 a0)
mystery := andB true               mystery₂ := fun x => x
applyTwice (f : Bool → Bool) (b) := f (f b)
alarmShouldSound (armed doorOpen motionDetected)

-- Teoremi [1]
✓ neg_is_involutory (b) : negB (negB b) = b
✓ negB_eq_negMatch : ∀ b, negB b = negMatch b
✓ false_and_any_is_false (a) : andB false a = false          (+ versione _forall)
○ andB_commutative_sorry (a b) : andB a b = andB b a          -- sorry (la versione provata è sotto)
✓ andB_commutative (a b) : andB a b = andB b a
✓ and_true_right (b) : andB b true = b                        (+ _cases)
✓ andB_eq_andMatch : ∀ a b, andB a b = andMatch a b
✓ and_associative (a b c) : andB a (andB b c) = andB (andB a b) c
✓ or_false_right (b) : orB b false = b
✓ nor_commutative (a b) : norB a b = norB b a
✓ deMorgan_nand (a b) : !(a && b) = (!a || !b)
✓ mystery_resolved (b) : mystery b = mystery₂ b
✓ (example) allThree a b c = allThreeMatch a b c
```

## 2. Uguaglianza, successore e logica di base su Nat

```lean
-- Definizioni [2]
predN, predN₂ : Nat → Nat          -- 0 ↦ 0 | n+1 ↦ n
isZero : Nat → Bool

-- Teoremi [2]
✓ pred_succ (n) : predN (succ n) = n
✓ succN_injective : ∀ n m, succ n = succ m → n = m
✓ succ_succ_injective (a b) : succ (succ a) = succ (succ b) → a = b      (+ ₂ con apply)
✓ succ_succ_succ_injective (a b) : succ³ a = succ³ b → a = b
✓ succ_ne_zero_example (n) : succ n = 0 → False
✓ zero_ne_succ (n) : 0 = succ n → False
✓ eq_trans : ∀ a b c, a = b → b = c → a = c        (+ eq_trans₂, eq_trans₃, eq_trans_apply)
✓ eq_from_common_left (a b c) (hab : a = b) (hac : a = c) : b = c
✓ eq_from_succ_chain (a b c) (succ a = succ b) (succ b = succ c) : a = c
✓ plus_id : ∀ n m o, n = m → m = o → n + m = m + o
```

## 3. Addizione (`addN`, `doubleN`, `tripleN`)

```lean
-- Definizioni [2]
addN (n m)      -- ricorre su m:  addN n 0 = n ; addN n (m'+1) = succ (addN n m')      (+ addN₂)
doubleN (n)     -- 0 ↦ 0 | n+1 ↦ doubleN n + 2
tripleN (n) := addN (doubleN n) n

-- Teoremi [2]
✓ addN_zero (n) : addN n 0 = n
✓ addN_succ_right (n m) : addN n (m+1) = addN n m + 1
✓ addN_one_right (n) : addN n 1 = succ n
✓ addN_two_steps (n m) : addN n (m+1+1) = addN n m + 1 + 1
✓ succ_addN_zero : ∀ n, succ (addN n 0) = succ n
✓ addN_id : ∀ n m, n = m → n + n = m + m                   (+ addN_id₂)
✓ addN_zero_eq (n m) (h : n = m) : addN n 0 = m
✓ addN_zero_trans : ∀ a b c, a = b → b = c → addN a 0 = c
✓ addN_zero_cancel (n m) (h : addN n 0 = addN m 0) : n = m

-- Teoremi [3]
✓ zero_addN (n) : addN 0 n = n
✓ addN_succ_left (n m) : addN (n+1) m = addN n m + 1
✓ addN_comm (n m) : addN n m = addN m n
✓ addN_assoc (n m k) : addN (addN n m) k = addN n (addN m k)
✓ addN_self_succ (n) : addN (n+1) (n+1) = addN n n + 1 + 1
✓ doubleN_addN (n) : doubleN n = addN n n
```

## 4. Moltiplicazione (`mulN`)

```lean
-- Definizioni [2]
mulN (n m)      -- mulN n 0 = 0 ; mulN n (m'+1) = addN (mulN n m') n

-- Teoremi [2]
✓ mulN_zero_right (n) : mulN n 0 = 0
✓ mulN_succ_right (n m) : mulN n (m+1) = addN (mulN n m) n
✓ mult_n_0_m_0 : ∀ n m, addN (mulN n 0) (mulN m 0) = 0
✓ one_not_double : ∀ n, 1 = mulN 2 n → False

-- Teoremi [3]
✓ mulN_zero_left (n) : mulN 0 n = 0
✓ mulN_one_left (n) : mulN 1 n = n
✓ mulN_succ_left (n m) : mulN (n+1) m = addN (mulN n m) m
✓ mulN_comm (n m) : mulN n m = mulN m n
✓ mulN_addN_distrib_left (a b c) : mulN a (addN b c) = addN (mulN a b) (mulN a c)   -- induzione su c
◐ mulN_addN_distrib_left_by_first_arg (a b c) : (stesso enunciato)   -- induzione su a, opzionale; la catena di rw finisce senza chiudere esplicitamente
✓ mulN_assoc (n m k) : mulN (mulN n m) k = mulN n (mulN m k)

-- Teoremi [4]
✓ repeatAdd_eq_mulN (a b) : repeatAdd b a 0 = mulN a b       (vedi sezione 13)
```

## 5. Altre funzioni aritmetiche

Solo definizioni, nessun teorema [2]. Le versioni con `iterateN` (`pow2`) e `sumBelow` sono nelle sezioni 13 e 14.

```lean
powN (b e)     -- powN b 0 = 1 ; powN b (e'+1) = mulN (powN b e') b
fact           -- 0 ↦ 1 | n'+1 ↦ mulN (n'+1) (fact n')
sumUpTo (n)    -- 0 ↦ 0 | n+1 ↦ (n+1) + sumUpTo n
fibN (n)       -- 0 ↦ 0 | 1 ↦ 1 | n+2 ↦ fibN n + fibN (n+1)
```

## 6. Parità (`isEven`, `isOdd`)

```lean
-- Definizioni [2]
isEven : Nat → Bool     -- 0 ↦ true | 1 ↦ false | n+2 ↦ isEven n
isOdd (n) : Bool        -- 0 ↦ false | 1 ↦ true | n+2 ↦ isOdd n

-- Teoremi [3]
✓ isEven_succ (n) : isEven (n+1) = !(isEven n)
✓ isEven_doubleN (n) : isEven (doubleN n)
```

## 7. Confronti e sottrazione (`eqN`, `leN`, `ltN`, `subN`)

```lean
-- Definizioni [2]
eqN : Nat → Nat → Bool   -- 0,0 true | 0,_+1 false | _+1,0 false | n+1,m+1 ↦ eqN n m
leN : Nat → Nat → Bool   -- 0,_ true | _+1,0 false | n+1,m+1 ↦ leN n m
ltN (n m) : Bool         -- 0,0 false | _+1,0 false | 0,_+1 true | n+1,m+1 ↦ ltN n m
subN : Nat → Nat → Nat   -- 0,_ ↦ 0 | n+1,0 ↦ n+1 | n+1,m+1 ↦ subN n m

-- Teoremi [2]
✓ leN_zero_right (n) : leN n 0 = isZero n
✓ isZero_eq_eqN (n) : isZero n = eqN n 0
✓ leN_eq_eqN_zero (n) : leN n 0 = eqN n 0
✓ not_eqN_of_false (a b) (h : eqN a b = false) : !(eqN a b) = true

-- Teoremi [3]
✓ eqN_refl (a) : eqN a a = true
✓ leN_refl (n) : leN n n = true
○ leN_succ_right_sorry (a b) : leN a b = true → leN a (b+1) = true   -- tentativo "ingenuo", resta sorry di proposito
✓ leN_succ_right (a b) : leN a b = true → leN a (b+1) = true         -- induction a generalizing b
✓ leN_succ_left (a b) : leN (a+1) b = true → leN a b = true          -- induction b generalizing a
✓ leN_antisymm : ∀ n m, leN n m → leN m n → eqN n m
✓ subN_self (n) : subN n n = 0
```

## 8. Espressioni booleane (`BExp`) [3]

```lean
BExp : btrue | bfalse | bifte (c t e : BExp)
evalBExp : BExp → Bool       -- bifte t0 t1 t2 ↦ if evalBExp t0 then evalBExp t1 else evalBExp t2
dualBExp : BExp → BExp       -- btrue↔bfalse ; bifte t0 t1 t2 ↦ bifte (dual t0) (dual t2) (dual t1)

✓ dualBExp_involutive (t) : dualBExp (dualBExp t) = t
✓ evalBExp_dual (t) : evalBExp (dualBExp t) = !(evalBExp t)
```

## 9. `BExpConn` e traduzione in `BExp` [3]

```lean
BExpConn : btrue | bnot t | band t0 t1 | bor t0 t1
✓ evalBExpConn : BExpConn → Bool       -- btrue ↦ true | bnot t ↦ !(eval t) | band ↦ && | bor ↦ ||
✓ bexpconn_to_bexp : BExpConn → BExp   -- bnot a ↦ bifte a bfalse btrue
                                       -- band a b ↦ bifte a b bfalse
                                       -- bor a b ↦ bifte a btrue b
✓ bexpconn_equiv_bexp (t) : evalBExpConn t = evalBExp (bexpconn_to_bexp t)
```

## 10. Numeri binari (`BinNat`) [3]

```lean
BinNat : bz | b0 n (=2n) | b1 n (=2n+1)
bz, six, six', thirteen, twentyfour           -- numerali di esempio
hasLeadingZeros : BinNat → Bool               -- .bz ↦ false | .b0 .bz ↦ true | .b0 b ↦ rec b | .b1 b ↦ rec b
normalize (b) := natToBin (binToNat b)

✓ binToNat : BinNat → Nat                     -- bz ↦ 0 | b0 n ↦ binToNat n * 2 | b1 n ↦ binToNat n * 2 + 1
✓ binIncr : BinNat → BinNat                   -- bz ↦ b1 bz | b0 n ↦ b1 n | b1 n ↦ b0 (binIncr n)
✓ natToBin : Nat → BinNat                     -- zero ↦ bz | succ n ↦ binIncr (natToBin n)
✓ binDouble : BinNat → BinNat                 -- bz ↦ bz | b ↦ b0 b

✓ natToBin_succ (n) : natToBin (n+1) = binIncr (natToBin n)
✓ binToNat_binDouble (b) : binToNat (binDouble b) = 2 * binToNat b
✓ binToNat_binIncr (b) : binToNat (binIncr b) = binToNat b + 1
✓ binToNat_natToBin (n) : binToNat (natToBin n) = n
✓ binIncr_binDouble (b) : binDouble (binIncr b) = binIncr (binIncr (binDouble b))
✓ binDouble_binToNat (n) : binDouble (natToBin n) = natToBin (2 * n)
✓ binDouble_hasLeadingZeros (b) : hasLeadingZeros (binDouble b) = hasLeadingZeros b
✓ normalize_smallest (b) : hasLeadingZeros (normalize b) = false
```

## 11. Funzioni di ordine superiore e composizione [4]

```lean
-- Definizioni
incr := fun n => n + 1
incrBy (k) : Nat → Nat := fun n => n + k
applyTwiceN (f) (n) := f (f n)
applyThreeTimesN (f) (n) := f (f (f n))
addFive := addN 5
chooseN (s : Bool) (f g : Nat → Nat) : Nat → Nat := if s then f else g
sumFun : (Nat → Nat) → (Nat → Nat) → (Nat → Nat) := fun f g n => addN (f n) (g n)
composeN (f g) := fun n => f (g n)          -- g viene applicata per prima
applyThenTest (p : Nat → Bool) (f : Nat → Nat) := fun n => p (f n)
idN : Nat → Nat := fun n => n

-- Teoremi (uguaglianza tra funzioni, tattica funext)
✓ composeN_identity_left (f) : composeN idN f = f
✓ composeN_identity_right (f) : composeN f idN = f
✓ composeN_assoc (f g h) : composeN (composeN f g) h = composeN f (composeN g h)
```

## 12. Trasformatori di stato: `deposit` e `withdraw` [4]

```lean
-- Definizioni
deposit (amount) : Nat → Nat := fun balance => balance + amount
withdraw (amount) : Nat → Nat := fun balance => balance - amount

-- Teoremi
✓ deposit_zero : deposit 0 = idN
✓ deposit_additivity (a b) : composeN (deposit a) (deposit b) = deposit (b + a)
✓ deposit_reversibility : ∀ a, ∃ b, composeN (withdraw b) (deposit a) = idN     -- testimone b = a
```

## 13. Iterazione (`iterateN`) [4]

```lean
-- Definizioni
iterateN (f) (k n) : Nat      -- 0 ↦ n | k'+1 ↦ f (iterateN f k' n)
pow2 (n) : 0 ↦ 1 | n ↦ iterateN (fun x => x*2) (n-1) 2
repeatAdd (k a) : Nat → Nat := iterateN (fun x => addN x a) k

-- Teoremi
✓ applyTwiceN_eq_iterateN2 (f) : applyTwiceN f = iterateN f 2
✓ iterateN_fixed_point (f) (x k) (h : f x = x) : iterateN f k x = x
✓ iterateN_succ_eq_addN (k n) : iterateN Nat.succ k n = addN n k
✓ iterateN_succ_from_zero (k) : iterateN Nat.succ k 0 = k      (+ versione ₂ con have)
✓ repeatAdd_succ_left (a b c) : repeatAdd (a+1) b c = addN (repeatAdd a b c) b
✓ repeatAdd_eq_mulN (a b) : repeatAdd b a 0 = mulN a b
```

## 14. Sommatorie e ricerca limitata [4]

```lean
-- Definizioni
sumBelow (f) (n) : Nat        -- 0 ↦ 0 | n'+1 ↦ sumBelow f n' + f n'
sumOfSquares (n) := sumBelow (fun x => x*x) n
countBelow (p : Nat → Bool) (n) : Nat   -- 0 ↦ 0 | n'+1 ↦ if p n' then countBelow p n' + 1 else countBelow p n'
anyBelow (p) (n) : Bool := countBelow p n > 0
allBelow (p) (n) : Bool := countBelow p n == n
○ checkCommutativeBelow (op : Nat → Nat → Nat) (n) : Bool := sorry

-- Teoremi
✓ sumBelow_one (n) : sumBelow (fun _ => 1) n = n
✓ anyBelow_grows (p n) (h : anyBelow p n = true) : anyBelow p (n+1) = true
○ allBelow_true (n) : allBelow (fun _ => true) n = true
○ allBelow_eq_not_anyBelow_not (p n) : allBelow p n = !(anyBelow (fun i => !(p i)) n)
○ allBelow_of_forall (p n) (h : ∀ i, p i = true) : allBelow p n
○ (example) (∀ a b, eqN (op a b) (op b a) = true) → checkCommutativeBelow op n = true
○ sumOdd_closed (n) : sumBelow (λ n => 2 * n - 1) (n+1) = n * n
○ sumId_closed (n) : 2 * sumBelow (λ n => n) (n+1) = (n+1) * n
```

## 15. Banca come funzione [4]

```lean
-- Definizioni
Bank := Nat → Nat
emptyBank : Bank := fun _ => 0
depositBank (a n) (b : Bank) : Bank := fun x => if x = a then b a + n else b x
○ withdrawBank (a n) (b : Bank) : Bank := sorry    -- nessun effetto se i fondi non bastano

-- Teoremi
○ depositBank_same (a n b) : depositBank a n b a = b a + n
○ depositBank_other (a x n b) (h : x ≠ a) : depositBank a n b x = b x
○ depositBank_additivity (a n1 n2) : (depositBank a n2) ∘ (depositBank a n1) = depositBank a (n1 + n2)
○ withdrawBank_additivity (a n1 n2 b) (h : b a ≥ n1 + n2) :
    withdrawBank a n2 (withdrawBank a n1 b) = withdrawBank a (n1 + n2) b
```

