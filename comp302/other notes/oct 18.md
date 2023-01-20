Prove all things; hold fast that which is true.

Test out ideas, and keep it and remember it

  

But we can prove at least some things. In particular, about lists.

Lists are defined inductively:

```ocaml

tyoe 'a list =

| []

|(::) of 'a * 'a list

```

That is:

- [] (nill)

This is an inductive definition! Lists are defined in terms of lists, but always building up ina sound way.

An inductive proof is like a recursive program using pattern mathcing. Think of it like an algorithm.

- Base Case: Prove the property for the case of [] (nil)

            Every recursive algorithm needs a base case

- Step case: Prove the property for the case of x::xs (cons)

        -Assume the property holds for the sublist sx.

        This is the induction hypothesis and corresponds to making a recursive call.

Now for any given list, we can "run" our proof on it to construct a specific proof for that list.

Specific -> if you have a specific list and run your proof on it

  

```ocaml

let rec (@) (l1 : 'a list) (l2 : 'a list): 'a list =

    match l1 with

    |[]-> l2

    |x :: xs -> x :: (xs @ l2)

let rec rev (l : 'a list) : 'a list = match l with

    |[]->[]

    |x :: xs -> rev xs @ [x]

```

**If function is an operator you can put it inbetween the two argument it needs, you can create an operator by using the  curly brackets  in the defintion.**

  

Theorem: For any l1,l2,l3, : 'a list

    l1@(l2@l3) = (l1@l2)@l3 # it is associative, they evaluate to the same value

  

```

Proof:

We choose l1 since append is defined by recursion on the left argument

By Induction on l1  

Case l1 = []

WTS:

[] @ (l2 @ l3) = ( [] @ l2 ) @ l3             # We want to show that LHS and RHS are equal

  

LHS = l2 @ l3          By defintion of @

RHS = l2 @ l3          By defintion of @

we see that LHS = RHS

  

Case l1 = x :: xs

WTS:

(x :: xs) @ (l2 @ l3) = ( (x :: xs) @ l2 ) @ l3             # We want to show that LHS and RHS are equal

  

LHS = x :: ( xs @ (l2 @ l3)          By defintion of @

IH (induction hypothesis): xs @ (l2 @ l3) = ( xs @ l2) @ l3

LHS = x :: ((xs @ l2) @ l3)          By IH

    = ( x :: ( xs @l2)) @ l3         By @ def

    = ((x :: xs) @ l2) @ l3

    = RHS

we see that LHS = RHS

  

We covered all the cases.

```

  

Theorem: Show that for any l : 'a list

         l @ [ ] = l              

```

By induction on l

case l = []

WTS:

[] @ [] = []

LHS = []              By defintion of @

    = RHS

case l = x :: xs

WTS:

(x :: xs) @ [] = (x :: xs)     We want to show LHS=RHS

IH : xs @ [] = xs

LHS = x :: (xs @ [])  By defintion of @

    = x :: xs         By IH

    = RHS

We see that LHS = RHS

We covered all the cases

  
  
  
  

HIS PROOF:

By induction on l

Case l = []

LHS = [] @ []

    = []                BY def @

    = RHS

case l = x :: xs

WTS:

(x::xs) @ [] = x::xs

IH : xs @ [] = xs

LHS = x :: (xs @ [])    By def @

    = x :: xs           By IH

    = RHS

```

  

Theorem:  For any l1,l2 : 'a list

rev (l1 @ l2) = rev l2 @ rev l1

```

By induction on l1

Case l1 = x::xs

WTS:

rev ((x::xs) @ l2) = rev l2 @ rev (x::xs)

IH :

rev (xs @ l2) = rev l2 @ rev xs

LHS = rev (x :: (xs @ l2))              By def @

    = rev (xs @ l2) @ [x]               By def rev

    = (rev l2 @ rev xs) @ [x]           By def IH

    = rev l2 @ (rev xs @ [x])           By lemma, by associativity

    = rev l2 @ rev (x::xs)              By def rev

    = RHS

```

Exercise write the base case for the previous proof