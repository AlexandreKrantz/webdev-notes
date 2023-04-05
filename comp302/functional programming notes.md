### Tail Recursive
Base case should return everything
```ocaml
let rec sum n =
	match n with 
	| 0 -> 0
	| _ -> sum (n-1) + n
sun 5:
5+(sum 4)
5+(4+sum 3)
5+(4+(3 sum 2))
5+(4+(3+(2+sum1)))
5+(4+(3+(2+1)))
let rec sum' n acc =
	match n with
	|0 -> acc
	|_ -> sum (n-1) (n+acc)
sum' 5 0
_
```

### The Call Stack
- When you make recursive calls, you have to store a lot of stuff in memory. If you have a large input (many recursive calls), then you need to store arguments, return address, return value each time.
  - This can result in stack overflow errors
- There's a trick to use constant memory for recursive function calls: [tail calls](https://stackoverflow.com/questions/310974/what-is-tail-call-optimization).
	- Tail recursive = makes recursive call and immediately returns the output of that call. 

### Higher Order Functions
- DEF. A **higher-order function** takes a function as input or produces a function as output. 
```ocaml
let i x f = f x
(* In this case, the input `f` is a function. We see this in the body of the
function `i`, as `f` is applied to `x`, so it must be a function. Since `f`
takes a single input `x`, the type of `f` will have to look like `??? -> ???` --
there must be an arrow in its type. Moreover, the type we assign to `x` must
also be the input type of `f`. *)

```
In this example `f` is an input to the function `i`. We can't tell straight away that it is a function and not just another variable. However, `f` is used like a function within `i` and from this compiler can infer that it is a function. `i : 'a -> ('a -> 'b) -> 'b`

- In combination with polymorphic data structures, they enable us to express *generic algorithms*. 
- A function of n inputs can be refactored into a function of 1 input that returns a function of n-1 inputs. This is called "staged computation" // "partial evaluation".
- Enables us to rewrite any function tail-recursiely, using continuation-passing style.

Alternative function syntax:
```ocaml
let f = fun x -> 2 * x
```

Map function (just like in JS):
```ocaml
let rec map (f : a' -> b') (l : a' list) : 'b list = match l with
  | [] -> []
  | x :: xs -> 
      let y = f x in 
      let ys = map f xs in 
      y :: ys
```

- **Functions are first-class values** (just like integers, but they have the special property of being "callable"). You can pass functions as arguments and have functions as your return values. 

- In the OCaml standard library there are a bunch of standard higher order functions such as `List.map`, `List.filter`, etc. 

#### Currying and Uncurrying
- It was observed by Frege in 1893 that it suffices to restrict attention to functions of a single argument. In other words, you can always simplify a multi-input function to take only one input.

- **Curry function**: takes as input a function `f : ('a * 'b) -> c` and returns `f : 'a -> 'b -> 'c`.
```ocaml
let curry f = (fun x y -> f (x,y))
```

- *Uncurrying* is the reverse process where you have one arguement in a tuple format and then separate them out into multiple arguments:
```ocaml
(* uncurry : ( ’a -> ’b -> ’c) -> (( ’a * ’b) -> ’c) *) 
let uncurry f = (fun (y , x ) -> f y x )
``` 

- Functions written with type `t1 -> t2 -> t3` are called _curried_ functions, and functions with type `t1 * t2 -> t3` are called _uncurried_.

#### Partial Evaluation 
![](Pasted%20image%2020221019120954.png)

Check out the textbook for a great explanation (ctrl f "4.2.4 Example 4: Partial evaluation and staged computation").

#### Code Generation
Textbook "4.2.5 Example 5: Code generation"
???




### Greedy Algorithm
- For the example of giving someone money in the least amount of coins, you basically add the largest coin to the total each time that doesn't surpass what you're supposed to give. So if you have $20, $10, and $5 bills, to give $35 you would give $20 first, then $10, then $5. 
- You make the locally optimal choice at each stage with the hope of finding a global optimum (doesn't always work ofc). Usually it is decently fast and easy to implement, but it often doesn't give you the global optimum. 
- [Concept explainer video](https://www.youtube.com/watch?v=HzeK7g8cD0Y)

### Continuations
- recall the definition of tail recursive: A function is tail recursive if it calls itself recursively but **_does not perform any computation after the recursive call returns_**

#### Continuation Passing Style (CPS)
- Rewriting a function in CPS is one way to make it tail recursive (works for *any* function).
- For a continuation, you save the current **state** of execution into some object, so that you can restore the **state** from this object later. It's not unlike async/await in JS.
You have one function that you want to apply after another:
```ocaml
let f x k = k (x + 1)
let g x = f x (fun r -> 2*r)
```
	- You apply the function that is defined in g after doing the computation that is defined in f. 
	- If you call f directly then it'll just do the x+1 and the k function will default to an identity function. 
	- So essentially you *wait* for the computation of f to be done and then you do the continuation fuction. 

- It is a deep property of ML that every function can be rewritten in tail-recursive form!
	- When you have a non-tail-recursive function, you are typically calling the function recursively and then applying some kind of manipulation to the result before returning it.
	- The idea with continuations is that you can package this extra manipulation as another function that you send as an argument to the recursive function call. That way you can just directly return the result of the recursive function call.
	- Say `n f:’a -> ’b` is *not* tail-recursive. Then there exists a tail-recursive version of the function `f ’: ’a -> ( ’ c -> ’b ) -> ’b`
- So a continuation is like a functional accumulator!
	- When we use this function to compute `f` we give it the initial continuation which is often the identity function `fun r -> r` , indicating no further computation is done on the result.
```ocaml
(* tail recursive append function *)

let rec app' l k c = match l with (* higher order function takes params + continuation function c *)
	| [] -> c k (* base case: return the list k *)
	| h :: t -> app' t k (fun r -> c ( h :: r ) ) (* append the tail and list k, pass in a continuation that appends the head to the result*)

let rec app_tr l k = app' l k (fun r -> r ) (* 1. call the higher order function, pass in the identity function*)

```
	- Although this program is tail recursive, it is not more performant. The deferred operations still need to stored somewhere, they just won't be stored on the runtime stack. The difference is that we (the programmer) are now in control of where they are stored and have direct handle on future computation (whatever that means...). 

#### Control with Continuations
Example: factorial with continuation (`factc`) vs. normal (`fact`)
```ocaml
let rec fact n = if n=0 then 1 else n * fact(n-1) ;; (*normal*)

let rec factc n k = if n=0 then k 1 else factc (n-1) (fun x -> k (x*n)) ;; (*continuation*)
```

Example: summing a binary tree with cps
```ocaml
let rec sum_tree t = match t with (*no cps*)
  | Empty -> 0
  | Tree(t1, x, t2) -> (sum_tree t1) + x + (sum_tree t2) 
  

let rec sum_treecps t k = match t with (*yes cps*)
  | Empty -> k 0
  | Tree(t1, x, t2) -> sum_treecps t1 (fun m -> sum_treecps t2 (fun n -> k(n + m + x)))

```

Success vs. failure continuation:
```ocaml
(* In the discussion about exceptions, we looked at search through binary trees.
Here is a binary tree. *)
type 'a tree =
  | Empty
  | Node of 'a tree * 'a * 'a tree

(* We can implement a `find` function that gives us the first element satisfying
a given predicate in a pre-order left to right traversal. Since the search can
fail, we need to represent this somehow. A nice way to do this is with the
`option` type. *)
let rec find (p : 'a -> bool) (t : 'a tree) : 'a option = match t with
  | Empty -> None (* Search failed! *)
  | Node (l, x, r) ->
    if p x then Some x (* search succeeded! *) else
    (* Otherwise, we need to recursively search the subtrees.
       The search through the left subtree could fail, in which case we must try
       the right subtree. So we need to analyze the option returned by the left
       subtree search and try the right subtree in case that result was a
       failure. *)
    match find p l with
    (* the outcome of the right subtree search becomes the overall outcome since
    we return its value directly. *)
    | None -> find p r 
    (* On the other hand if the left subtree search succeeds, then we have
    succeeded overall and reconstruct the successful result. *)
    | Some x -> Some x

(* We can use the general approach to converting tail-recursion using
continuations and we obtain the following implementation. *)
let rec find1 (p : 'a -> bool) (t : 'a tree) (return : 'a option -> 'r) : 'r = match t with
  | Empty -> return None
  | Node (l, x, r) ->
    if p x then return (Some x) else
    find1 p l (fun result -> match result with
                             | None -> find1 p r return
                             | Some x -> return (Some x))

(* However, there is an important observation we can make about this function
that allows to obtain a _more efficient_ implementation. *)

(* In the initial `find` implementation, there are effectively two different
ways that the function can return.
1. Return by failing, i.e. return None. Note that the failure doesn't contain
   any additional information (None has no fields).
2. Return by succeeding, i.e. return Some x. The success contains the successful
   result `x`.

We can represent each different way of returning by a _separate continuation_!
One continuation, which we will call `fail`, represents returning with failure.
The other, which we will call `succeed`, represents returning a successful result. *)
let rec find2 (p : 'a -> bool) (t : 'a tree) (fail : unit -> 'r) (succeed : 'a -> 'r) : 'r = match t with
(* A call like `find2 p t fail succeed` means:
   "try to find an element of t satisfying p; if you can't, then call `fail`; if
   you can, then call `succeed` with that element." *)

  (* In this case, the search fails, so we call the failure continuation. *)
  | Empty -> fail ()
  | Node (l, x, r) ->
    (* If x satisfies p, then we call the success continuation with the element, as required. *)
    if p x then succeed x else
    (* If it doesn't, then we must recursively analyze the subtrees.
       In case the search through the left subtree fails, we want to search
       through the right subtree. We express this by passing in an adjusted failure continuation.
       In case of success, there's nothing special to do before we can simply
       call the success continuation, we can just pass it along unchanged.
     *)
    find2 p l (fun () -> find2 p r fail succeed) succeed

(* This implementatation is more efficient because it avoids matching on the
result of the recursive call entirely!
Instead, at the point in the program that we detect the failure (Empty case) or
the success (p x = true) we call the appropriate continuation. *)

(* There is also a connection to exceptions that we can make here too.
Calling the failure continuation is like throwing an exception. Using an
adjusted failure continuation (as in the call to find2 p l) is like using a
try-with block to handle the failure. Passing along the failure continuation
unchanged is equivalent to not handling the exception and instead letting a
parent caller handle it. *)
fall2022-06-1.ml

```

Another example:
```ocaml
let rec exact k t s f = 
match k,t with 
| 1, Trie (c, []) -> s [c]
| 1, Trie (c, _ :: _) -> f ()
| k, Trie (c, []) f () 
| k, Trie (c, t :: ts) -> 
	exact (k-1) t (fun r -> s (c :: r)) (fun () -> exact k (Trie(c, ts)) s f)

exact 3 mytrie (fun x -> Some(x)) (fun () -> None)
```

### Eager Evaluation (call-by-value)
- In eager (aka strict) evaluation, the arguments are evaluated before the function call is made.
For example, `f e1 e2` is equivalent
```ocaml
let x = e1 in let y = e2 in f x y
```
Pros: easy to reason about, clear when evaluation happens (on function call)
Cons: may evaluate expressions that are never needed

- In OCaml, all variables are bound to fully evaluated expressions. 
	- For example in `let x = e1 in e2`, the value of `e1` is first evaluated to some value we'll call `v1`. Then `x` is assigned the value `v1` and finally `e2` is evaluated in this environment. 
	- This is what we call "binding by value".
Another example: in `(fun x -> e) e1`, we first evaluate `e1` to a value `v1`. Then assign `x` the value `v1`, then evaluate `e`. Things are always done this way, whether or not the variable `x` is actually used in the function body or not. 

### Lazy (call-by-name)
- Lazy evaluation languages delay the actual evaluation until it is required.
- Variables can be bound to unevaluated expressions, i.e. computation.
	- You would suspend this computation until the value of the variable is required in some operation. 

- In practice, lazy evaluation adopts a refinement of the call-by-name principle, called ***call-by-need*** principle. According to the call-by-need principle, variables are bound to unevaluated expressions, and are evaluated only as often as the value of that variable’s binding is required to complete the computation. So in `let x = horribleComp (345) in x + x`, we only evaluate `x` once for the first argument of the addition, and the second argument is aware that `x` is already evaluated so it just reuses the value. 
	- According to the call-by-need principle, we only evaluate once `horribleComp(345)` to a value `v1`, and then we **memoize** (aka remember) this value and associate the binding for `x` to the value `v1`.
- Def. Memoization = the binding of a variable is evaluated at most once, not at all, if it is never needed, and exactly once, if it is ever needed. Once a computation is evaluated, its value is saved for the future. 
- Lazy evaluation supports **demand-driven computation**. We compute a value if it is actually needed somewhere.

In OCaml, we can model lazy programming with functions and records. We can suspend computation using functions, because the contents of a function body are not evaluated until the function is called. 
![](Pasted%20image%2020221109111123.png)

#### Infinite Data
- Finite data (eg. list) is created with constructors. We take apart this type of data with pattern matching and reason on it with inductions. 
- Infinite data is not defined by constructors... but instead by the observations we can make on it.
![](Pasted%20image%2020221109111425.png)

- While finite objects are intensional, i.e. two objects are the same if they have the same structure, infinite objects are extensional, i.e. they are the same if we can observe them to have the same behavior.
	- Functions are an example of an object with extensional behavior. 
For example, these two functions have the same behavior:
```ocaml
let f x = ( x + 5) * 2 
let g x = 2* x + 10
```
However, we cannot actually pattern match and verify that they are the same. *We can only apply the function to an argument, effectively performing an experiment, and observe the result or behavior of the function

- There is a notion when a program about streams is “good”, i.e. we can continue to make observations about streams. We call such programs **productive**. They may run forever, but at every step we can make an observation. This is unlike looping programs which run forever but are not productive.

Defining infinite structures: 
- `'a str` means a stream of elements of type `'a`
	- the head `hd` is one of the elements
	- the tail `tl` is a suspended stream. a `susp` of an `'a str`
```ocaml
type ’a str = { hd : ’a ; tl : (’a str) susp }
```

Example: infinite stream of 1s
```ocaml
let rec ones = { hd = 1 ; tl = Susp (fun () -> ones ) }
```
The head of a stream of one’s is simply `1`. The tail of a stream of one’s is defined recursively referring back to its definition. We can see here that it is key that the recursive call `ones` is inside the function `fun () -> ones` thereby preventing the evaluation of `ones` eagerly which would result in a looping program.

Example: steam of the natural numbers
```ocaml
(* numsFrom : int -> int str *) 
let rec numsFrom n = 
{ hd = n ; 
tl = Susp ( fun () -> numsFrom ( n +1) ) } 

let nats = numsFrom 0
```