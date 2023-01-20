An inductive proof is like a recusrive program using pattern matching.

Basically, logic and programming are the same thing.

```ocaml

let rec rev (l: 'a list) : 'a list =

    match l with

    | [] -> []

    |x :: xs -> rev xs @ [x]

  

let rec rev_tr (l : 'a list) (acc : 'a list) : 'a list =

    match l with

    |[] -> acc

    | x::xs -> rev_tr xs (x :: acc)

```

prove that they do the same thing:

Theorem for any l : 'a lsit ,

rev l = rev_tr l []

```

case l = []

    LHS = rev [] = [] = rev_tr [] [] = RHS

case l = x ::xs

    WTS: rev (x::xs) = rev_tr (x::xs) []

    IH: rev xs = rev_tr xs []

    RHS = rev_tr (x::xs) [] = rev_tr xs [x]

UH OHHHH!! Not compatible with IH, and the IH is not useful to us anymore

there is sth from the slide missing here

```

Often with inductive proofs, we need to generalize the theorem.

By proving a more genral theorem, we obtain a stronger IH.

We need not restrict ourselves to an empty accumulator!

we are gonna trace both of them

```

rev [1;2;3]

= rev [2,3]@[1]

= (rev[3]@[2])@[1]

= ((rev[]@[3])@[2])@[1]

~ rev[]@[3;2;1]

  

rev_tr [1;2;3] []

=rev_tr[2;3] [1]

=rev_tr[3] [2;1]

=rev_tr[] [3;2;1]

  

we can see that:

rev l@acc = rev_tr l acc

```

now we try to prove the theorem again, which is a generalized inductoin proofs:

```

Theorem: for all l, acc : 'a list ,

show that : rev l@acc = rev_tr l acc

  

Case l = []

LHS = rev []@acc

    = []@acc        By def rev

    = acc           By def @

    = RHS           By def rev_tr

  

Case l = x::xs

WTS rev (x::xs)@acc = rev_tr (x::xs) acc ∀ acc

IH : for all acc rev xs@acc = rev_tr xs acc

LHS = (rev xs@[x])@acc   By def of rev

    = rev xs@(x::acc)    By associativity and def @

    = rev_tr xs (x::acc) By def of IH with acc:=x::acc

    = rev_tr (x::xs) acc By def ot rev_tr

    = RHS

```

:= means not eqaul by here we are saying we are choosing acc as that, in IH is general so we are instanciating with somehting else.

Now going from this genrealization to the original theorem:

```

Theorem = for all l : 'a list,

show that rev l = rev_tr l []

  

proof:

By previous theorem with acc=[]

rev l@[] = rev_tr l []

rev l = rev_tr l []        By lemma we proved on Tuesday

```

And now we are done since we got the theorem we were looking for by instanciating the generalized theorem with acc = [ ].

@ works with the thing on the left so here we are using the lemma not definition of @ which works on the left one.

  

Exercise:

```

Thm:  sum l  = sum_tr l 0

sum [] = 0

sum (x::xs) = x + sum xs

  

sum_tr [] acc = acc

sum_tr (x::xs) acc = sum_tr xs (x+acc)

```

Writing the trace:

```

sum [1;2;3]

= 1 + sum[2;3]

= 1 + 2 + sum[3]

= 1 + 2 + 3 + sum[]

= (6) + sum[]

  

sum_tr [1;2;3] 0

= sum_tr [2;3] 1

= sum_tr [3] 3

= sum_tr [] 6

```

so the generalized form would be

```

acc + sum l = sum_tr l acc

```

  

```

Theorem :  for all l : int list,

and any acc:int,

acc + sum l = sum_tr l acc

  

proof by induction on l:

case l = []

LHS = acc + sum[]

    = acc + 0          By sum def

    = acc              By arithmetic operations

    = sum [] acc       By sum_tr def

    =RHS

case  l = x::xs

WTS: acc + sum (x::xs) = sum_tr (x::xs) acc

IHS: ∀ acc: acc + sum (xs) = sum_tr xs acc

LHS = acc + sum (x::xs)          

    = acc +(x + sum xs)           By definiton of sum

    = (acc+x) + sum xs            By associativity

    = sum_tr xs (acc+x)           By IH as acc := acc+x

    = sum_tr (x::xs) acc          By sum_tr def

    = RHS

  
  

Now from the generalized formula to the orginal

Thm:  sum l  = sum_tr l 0

By generazlized theorem with acc = 0

0 + sum l = sum_tr l 0

sum l  = sum_tr l 0               By arithmetic operations

```

  

Exercise:

prove the following theorem:

    map f l = map_tr f l (fun x -> x)

    and write a def of map_tr

you have to come up with a generalization:

  

k(map f l )  = map_tr f l  k