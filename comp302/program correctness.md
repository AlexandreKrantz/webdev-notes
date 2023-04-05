- Validation = ensuring software does what it's supposed to do.
	- Formal verification proves the correctness of a program with mathematics.
- Equalities are one of the fundamental ways we think about correctness of functional programs. The absence of mutable state makes it possible to reason straightforwardly about whether two expressions are equal.
	- A function with a given input should always be exactly equal to what we expect.
- If it is the case that for all inputs two functions produce the same output, we will consider the functions to be equal.
	- `if (forall x, f x = g x), then f = g.`
	- This definition is known as "the Axiom of Extensionality".

Example: equational reasoning proof
```ocaml
let twice f x = f (f x)
let compose f g x = f (g x)
let ( << ) = compose

WTS: ((f << g) << h) x =  (f << (g << h)) x

LHS = ((f << g) << h) x
= (f << g) (h x)
= f (g (h x))

RHS = (f << (g << h)) x
= f ((g << h) x)
= f (g (h x))

LHS = RHS
QED
```

Induction on natural numbers:
- For program P(n) that we want to prove on all natural numbers: 
	- Base case: prove P(n) true for n = 0
	- Inductive step: prove P(n) for n=k+1, where we assume that P(k) is true.

Programs as specifications: write an obviously correct implementation that is lacking in some desired property, such as efficiency, then prove that a better implementation is equal to the original.
```ocaml
(* simple, not tail-recursive *)
let rec fact n =
  if n = 0 then 1 else n * fact (n - 1)

(* tail-recursive *)
let rec facti acc n =
  if n = 0 then acc else facti (acc * n) (n - 1)

let fact_tr n = facti 1 n
```
	- Proof worked out on paper

Termination:
- So far we've been proving partial correctness: if a program terminates, then its output is correct.
- Total correctness: program definitely terminates + output is correct.
- Termination is very difficult to prove, certainly it's difficult for a computer to do on its own. 
- Non-rigorously: a recusive function terminates if the recursive calls are on smaller inputs and the base cases are all terminating. 
	- We want no infinitely descending chains. ie, the natural numbers would work but the integers would not. 
		- That's why we may want to use a custom `type nat = Z | S of nat` instead of just plain `int`. 

Induction with `type nat = Z | S of nat ` : 
```
forall properties P,
  if P(Z),
  and if forall k, P(k) implies P(S k),
  then forall n, P(n)
```

Induction with lists:
```ocaml
type    nat  = Z  | S      of nat
type 'a list = [] | ( :: ) of 'a * 'a list
```
	- list is setup in a similar way to nat, so the induction works similarly
```
forall properties P,
  if P([]),
  and if forall h t, P(t) implies P(h :: t),
  then forall lst, P(lst)
```

An inductive proof for lists therefore has the following structure:
```
Proof: by induction on lst.
P(lst) = ...

Base case: lst = []
Show: P([])

Inductive case: lst = h :: t
IH: P(t)
Show: P(h :: t)
```


```ocaml
let rec ( @ ) ( l1 : ’a list ) ( l2 : ’a list ) : ’a list = 
	match l1 with 
	| [] -> l2 
	| x :: xs -> x :: ( x @ l2 )

let rec rev ( l : ’a list ) : ’a list = 
	match l with 
	| [] -> [] 
	| x :: xs -> rev xs @ [ x ]
```
