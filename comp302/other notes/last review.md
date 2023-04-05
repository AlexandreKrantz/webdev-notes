## OOP and References
### Basic references
```ocaml
let x = ref 0
let incr () = x := !x + 1 ; !x
```

### Closures
Closures:
```ocaml
let tick_safe = ref 0 in
fun () -> counter := !counter + 1; !counter (* encloses the reference variable so that only the returned function can access it. That way can't be modified in another way*)

counter := 100 (* this would work in the previous example but not in this one *)
```

```ocaml
let (tick_safe, reset) = 
	let counter = ref 0 in
	fun () -> counter := !counter + 1; !counter
	((fun () -> counter := !counter + 1; !counter), fun () -> counter := 0)
```
^ Now we have *multiple* functions that refer to the same private variable. We can reset the counter or increment it. 

This behavior is a bit like OOP classes. 

### For loop
OCaml supports a rather limited form of the familiar `for` loop:

```ocaml
for variable = start_value to end_value do
  expression
done; 
  
for variable = start_value downto end_value do
  expression
done; 
```

### While loop
```ocaml
while boolean-condition do
  expression
done; 
```

### Records
```ocaml

type studentRecord = { name : string ; major : string ; grade : int}

let Alex : studentRecord = { name = "Alex" ; major = "Math" ; grade = 85}

(* normal data retrieval *)
let getName s = s.name

(* pattern matching *)
let getMajor s = match s with
| {name; major; grade} -> major
```

## Exceptions
Create an exception like this:
```ocaml
exception E2 of string
```

An exception is raised like this: 
```ocaml
let f a b = if b = 0 then raise (E2 "division by zero") else a / b;;
```

An exception may be handled with pattern matching:
```ocaml
try f 10 0 with E2 _ -> 0;;
```

When an exception is not handled, it's printed at the toplevel.

#### Backtracking with exceptions
Example: searching through a tree
```ocaml
type key : int 
type 'a tree
	| Empty
	| Node of 'a tree * key * 'a tree

exception NoSuchKey 

let rec keyLookup (t: 'a tree) (k: key): 'a = 
	match t with
	| Empty -> raise NoSuchKey 
	| Node (y, (k',v), u) -> if k'=k then v else 
		try 
			keyLookup l k (* if this succeeds, then the "with" part will not be evaluated.*)
		with 
			| NoSuchKey -> keyLookup r k (* you don't need to put this in a try catch because there's nothing to do after. Let the exception be thrown if the key isn't found.*)
```

## Higher order functions
- DEF. A **higher-order function** takes a function as input or produces a function as output. 

- A function of n inputs can be refactored into a function of 1 input that returns a function of n-1 inputs. This is called "staged computation" // "partial evaluation".

### Greedy Algorithm
- For the example of giving someone money in the least amount of coins, you basically add the largest coin to the total each time that doesn't surpass what you're supposed to give. So if you have $20, $10, and $5 bills, to give $35 you would give $20 first, then $10, then $5. 
- You make the locally optimal choice at each stage with the hope of finding a global optimum (doesn't always work ofc). Usually it is decently fast and easy to implement, but it often doesn't give you the global optimum. 
- [Concept explainer video](https://www.youtube.com/watch?v=HzeK7g8cD0Y)


## Continuations
- recall the definition of tail recursive: A function is tail recursive if it calls itself recursively but **_does not perform any computation after the recursive call returns_**

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
```

## Lazy evaluation
We say that the conditional test `if x then e1 else e2` _eagerly_ evaluates the test `x` but then _lazily_ evaluates `e1` and `e2`, i.e. only when needed.

Not so with function applications. When a function is applied to arguments in OCaml, the arguments are evaluated eagerly, and the function is applied to the resulting values. So if we tried to define the conditional test in a straightforward way as an OCaml function, it would not work.



- The body of a function is never evaluated, so we can suspend a computation by wrapping it in a function. 
```ocaml
fun () -> 3 + 7 
```

We can also tag the suspended computations in a tag to make things more clear:
```ocaml
type 'a susp = Susp of (unit -> 'a)
```
This way we have to use a specific `force` function to actually make the computation happen:
```ocaml
let force (Susp f) = f ()
```
Example of usage:
```ocaml
let x = Susp(fun () -> anotherfunction 23) in force x + force x 
```

### Infinite data
However, we can get infinite streams by _deferring_ the creation of the tail using thunks (suspend functions). Thus we create the tail only when we need it.

```ocaml
type 'a stream = Nil | Cons of 'a * (unit -> 'a stream)

(* an infinite stream of 1's *)
let rec (ones : int stream) = Cons (1, fun () -> ones)
```

Stream operations: 
```ocaml
(* head of a stream *)
let hd (s : 'a stream) : 'a =
  match s with
    Nil -> failwith "hd"
  | Cons (x, _) -> x

(* tail of a stream *)
let tl (s : 'a stream) : 'a stream =
  match s with
    Nil -> failwith "tl"
  | Cons (_, g) -> g () (* get the tail by evaluating the thunk *)

(* n-th element of a stream *)
let rec nth (s : 'a stream) (n : int) : 'a =
  if n = 0 then hd s else nth (tl s) (n - 1)

(* make a stream from a list *)
let from_list (l : 'a list) : 'a stream =
  List.fold_right (fun x s -> Cons (x, fun () -> s)) l Nil

(* make a list from the first n elements of a stream *)
let rec take (s : 'a stream) (n : int) : 'a list =
  if n <= 0 then [] else
  match s with
    Nil -> []
  | _ -> hd s :: take (tl s) (n - 1)

```
