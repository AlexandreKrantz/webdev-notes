### Basics
#### Backgroud Info
- OCaml is a garbage-collected language. It will free memory for data structures when they are no longer needed.
- Note that Microsoft's F# language is basically just a variant of OCaml. 
- The main thing that sets OCaml apart is that its functions are like **mathematical functions**.
	- That is, mathematical functions are _stateless_: they do not maintain any extra information or _state_ that persists between usages of the function. 
	- Also, functions are _first-class_: you can use them as input to other functions, and produce functions as output.
- _Imperative_ programming languages such as C and Java involve _mutable_ state that changes throughout execution. The **reality of mutability** is that whereas machines are good at complicated manipulation of state, humans are not good at understanding it.

#### Functions
- `let` creates new definitions that you can't change. A definition can be a fixed value *or a functionI*.
- functions are defined the same as other values but they have parameters
- call a function with a space: `f a` is `f` applied to `a`
  - If you have multiple arguments, separate them with spaces
  - Surround an argument with `()` if it is an expression
- The `rec` keyword is necessary to make a definition recursive. It allows the function to be called from within itself. Otherwise it'll look for a previously defined function with the same name.
- The basic types are int, float, char, boolean, and string. 
- A function `average` which takes two floats and returns a float has type: `average : float -> float -> float `
- `^` is the string concatenation operator
- Variable/function syntax: `let NAME = EXPRESSION in EXPRESSION`
  - `in` essentially makes it so that the definition is visible in the second expression.
- As a general rule, evaluating an expression does not change it's type.
- Definitions are immutable in OCaml. If a variable name is redefined in the same scope, then it doesn't mutate the original definition. The new definition is applied from that line onwards.
- You can use `'` (prime) character when naming a function.
- **Alternative function syntax:** `fun a -> a` (good for creating anonymous functions)
```ocaml
let f x = 2 * x
let f = fun x -> 2 * x (* creates the same function as prev line*)
```

- Operator functions: **If function is an operator you can put it inbetween the two argument it needs, you can create an operator by using the  curly brackets  in the defintion.**
``` ocaml
let rec (@) (l1 : 'a list) (l2 : 'a list): 'a list =
    match l1 with
    |[]-> l2
    |x :: xs -> x :: (xs @ l2)
```

#### Exceptions
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

Another example:
```ocaml
exception NoSuchKey of key
exception Invariant of string
let rec lookup (t : 'a tree) (k : key) : 'a = 
  match t with 
  | Empty -> raise (NoSuchKey k) 
  | Node -> (left, (k', v), right) -> 
      if k = k' then v else
      if k < k' then lookup left k else 
      if k > k' then lookup right k else 
        raise (invariant "math makes sense") (*this else statement shouldn't ever be reached*)
```


There's also a `try ... with` syntax that looks like `match ... with`:


#### Matching
You can an if statement:
```ocaml
# let rec factorial n =
    if n <= 1 then 1 else n * factorial (n - 1);;
```

Or if you have many options you can use the matching pattern. Putting an undefined variable (eg. `_` ) will match with anything:
```ocaml
# let rec factorial n =
    match n with
    | 0 | 1 -> 1
    | _ -> n * factorial (n - 1);;
val factorial : int -> int = <fun>
```

In fact, we may simplify further with the `function` keyword, which introduces pattern-matching directly:
```ocaml
# let rec factorial = function
    | 0 | 1 -> 1
    | n -> n * factorial (n - 1);;
val factorial : int -> int = <fun>
```

#### Tuples
```ocaml
# let t = (1, "one", '1');;
val t : int * string * char = (1, "one", '1')
```

You can also create records, which are like tuples but with named elements:
```ocaml
# type person =
   {first_name : string;
    surname : string;
    age : int};;
type person = { first_name : string; surname : string; age : int; }
# let frank =
    {first_name = "Frank";
     surname = "Smith";
     age = 40};;
```

#### Lists
- Lists are immutable. There’s no way to change an element of a list from one value to another. Instead, OCaml programmers create new lists out of old lists. [More info here](https://cs3110.github.io/textbook/chapters/data/lists.html#not-mutating-lists)
- A list can be *either* nil `[]` *or* the cons `::` of an element onto another list. Those are the only two ways of forming a list.

- `;` is the list `[]` element separator
- The `::`, or cons operator, adds one element to the front of a list. The `@`, or append operator, combines two lists:
```ocaml
# 1 :: [2; 3];;
- : int list = [1; 2; 3]
# [1] @ [2; 3];;
- : int list = [1; 2; 3]
```

- The pattern `h :: t` is used to deconstruct a list into its head (first element) and tail (the rest of the elements):
```ocaml
# let rec total l =
    match l with
    | [] -> 0
    | h :: t -> h + total t;;
val total : int list -> int = <fun>
# total [1; 3; 5; 3; 1];;
- : int = 13
```
- If you exclude the case where the list is empty you will get a warning. 

Function to find the length of a list:
```ocaml
# let rec length l =
    match l with
    | [] -> 0
    | _ :: t -> 1 + length t;;
val length : 'a list -> int = <fun>
```
- This function operates not just on lists of integers, but on any kind of list. This is indicated by the type, which allows its input to be `'a list` (pronounced alpha list).
	- The head of the list `_` is not inspected, so the function is polymorphic. 

### Types
#### Custom Types
Each type constructor must begin with a capital letter. 
```ocaml
# type colour =
   | Red
   | Blue
   | Green
   | Yellow
   | RGB of int * int * int;;
type colour = Red | Blue | Green | Yellow | RGB of int * int * int
# let l = [Red; Blue; RGB (30, 255, 154)];;
val l : colour list = [Red; Blue; RGB (30, 255, 154)]
```
The RGB just means a tuple of `(int, int, int)`.


Example: custom binary tree
```ocaml
type key = int
type 'a tree = 
	| Empty
	| Node of 'a tree * (key) * 'a tree
let ex : string tree =
	Node (
		Node ( 
		Empty,
		(3, "world"),
		Empty
		)
		(5, "hello"),
		Empty
	)
```

Example: custom list [CHECK IF CORRECT]
```ocaml
type 'a mylist = 
	| Nil
	| Cons of 'a * 'a mylist

let e = Nil (* is this how you use it ?? idk *)
let e0 = Cons(3, e)
let e1 = cons(4, e0)
```

#### Type Inference
- Ocaml infers the type of arguments to functions based on what they do in the function.
	- `let f x y = x + y` => x is an int
	- `let f x y = x +. y` => x is  a float
	- `let f x y = (x, y)` => x is a polymorphic type.  
- If a function is non-terminating, ocaml will not be able to determine the return type of the function. It will say that the function can return anything.
	- Whenever a function has a polymorphic return type, it is non terminating or returns an exception. 

#### Optional Types
- [Super good article!](https://cs3110.github.io/textbook/chapters/data/options.html)
- `Some` and `None` are constructors that let you build optional values, just as `::` and `[]` let you build lists. You can think of an option as a specialized list that can only have zero or one elements.
```ocaml
# let divide x y =
	  if y = 0 then None else Some (x / y);;
val divide : int -> int -> int option = <fun>
```
- Above we can see that using `Some` and `None` makes the return type an `int option` instead of just an `int`. This in unlike other languages where you can return a normal int or a null. Here it is treated as an "option" type no matter what. 

#### Polymorphic types
- Type variables are lowercase identifiers preceded by a single quote (Eg. `'a`). A type variable represents an arbitrary type.
- *Polymorphic functions* don't place any constraints on their inputs. 


### Exception Handling
**[Fantastic Article](https://cs3110.github.io/textbook/chapters/data/exceptions.html?highlight=try)**
- Every expression in ocaml...
	- Has a **type** that can be determined ahead of runtime
	- Computes to a value, diverges (infinite loop), or has an effect. 
- Exceptions are an example of an effect. Other effects are updating memory, printing to screen, etc. 
- **Type checking is a static violation**. You don't need to run the program and can detect the error just from the code text. 
- **Exceptions are dynamic violations** are raised at runtime and can bubble up.

Example: 
```ocaml
let head_of_empty_list = 
	let head (x::t) = x in 
	head [] ;;
```
	- Head takes as input an empty 'a list. The function basically separates out the first element of the list a returns it. But this shouldn't be possible on an empty list so it raises a 'Match_failure' exception. 

- **Backtracking** = General algorithm for finding all (or some) solutions incrementally – abandons partial candidates as soon as it determines that is cannot lead to a successful solution. [todo: learn more abt this]

#### Exception Values
1. Define an exception type: `exception Details of string`
2. Create the exception value. For example: `let e = Failure "something went wrong"` has type `e : exn = Failure "something went wrong"`
3. You can then raise your exception value: `raise e`


**Exception handling:**
```ocaml
try E with
| P -> E'
| ... 
```
	- P is a pattern that is an exception type 
	- E and E' are just arbitrary expressions. 

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

### Modules
- `List` is a module that is included automatically: 
```ocaml
List.map (fun x -> x * 2)
	(List.filter (fun x -> x mod 2 = 0) 1)

```

- Modules are first class citizens (you can use them as funciton inputs and outputs) 
- They resemble interfaces in java
- Modules also have types. `sig` is the signture of modules that have yet to be defined. 

- You can make your own modules to group functions/values together: 
```ocaml
module Hello = struct
  let message = "Hello"
  let hello () = print_endline message
end

let goodbye () = print_endline "Goodbye"

let hello_goodbye () =
  Hello.hello ();
  goodbye ()
```

- You can either define everything in one go (as done in the previous example) or define the module types with a `sig` "interface" paired with a separate module instantiation:
```ocaml
module Hello : sig
 val hello : unit -> unit
end
= 
struct
  let message = "Hello"
  let hello () = print_endline message
end
(* At this point, Hello.message is not accessible anymore. *)

```

- You can use the `ocaml` toplevel to visualize the contents of an existing module, such as `List`:
	- Just do `# #show List;;`
- If you want to add additional stuff to a module later in the program: [ocaml docs](https://ocaml.org/docs/modules#:~:text=for%20each%20library.-,Module%20inclusion,-Let%27s%20say%20we)

- In class example
```ocaml
type ordering = LT | EQ | GT
module type ORDERED = sig
	type t
	val compare : t -> t -> ordering (* function that takes two 't' variables and outputs an ordering. The function compare has to be implemented. *)
end 

module IntLt = struct 
	type t = int
	let compare : t -> t -> ordering = fun s t -> 
end 
```

#### Functors
- A `functor` transforms modules. It's like funtion but for modules instead of values. It replaces the `fun` syntax for anonymous functions. You're also required to specify the *type* of the module.
- **A functor is a module** that is parametrized by another module, just like a function is a value which is parametrized by other values, the arguments.
- [Syntax of functors (ocaml docs)](https://ocaml.org/docs/functors#:~:text=Defining-,functors,-A%20functor%20with)

#### The List Module
- Notice the type of the empty list is `'a list` (its element type is not known).
- The List Module is included in the standard library.
- `List.map f [a1; ...; an]` applies function `f` to `a1, ..., an`, and builds the list `[f a1; ...; f an]` with the results returned by `f`. Not tail-recursive.

**Searching a list:**
- Check if element is in a list with `List.mem a [a1; ... an]`
	- val mem : `'a -> 'a list -> bool`
	- You pass in a value, and it returns true if the value is in the list. 
```ocaml
# List.for_all (fun x -> x mod 2 = 0) [2; 4; 6; 8];;
- : bool = true
# List.exists (fun x -> x mod 2 = 0) [1; 2; 3];;
- : bool = true
```

- The find function finds the element that makes a function true, instead of comparing against an element directly. 
```ocaml
# List.find (fun x -> x mod 2 = 0) [1; 2; 3; 4; 5];;
- : int = 2
# List.find (fun x -> x mod 2 = 0) [1; 3; 5];;
Exception: Not_found.
```

- The filter function again takes a predicate and tests it against each element in the list, but this time returns the list of all elements which test true:
```ocaml
# List.filter (fun x -> x mod 2 = 0) [1; 2; 3; 4; 5];;
- : int list = [2; 4]
```

- `List.fold_left` and `List.fold_right` combine the elements of a list together, using a given function, accumulating an answer which is then returned.
	- The function that we supply should take in two inputs: the accumulation and the current element. 
```ocaml
# List.fold_left;;
- : ('a -> 'b -> 'a) -> 'a -> 'b list -> 'a = <fun>
```
- The function is of type `'a -> 'b -> 'a` where `'a` is the accumulator and `'b` is the type of each element of the list. The next argument is the initial accumulator, which must be of type `'a`, and then finally the input list of type `'b list`. The result is the final value of the accumulator, so it must have type `'a`.
- `List.fold_left` simply means that it moves throught the elements from left to right. 
- `^` is the binary operator to concatenate two strings. 


