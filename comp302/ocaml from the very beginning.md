```ocaml
let times10 n = 10 * n
fun int -> int 


let bothnonzero a b = match a, b with 
| 0, _ -> false
| _, 0 -> false
| _, _ -> true
fun int -> int -> bool

let rec calcsum n = match n with
| 1 -> 1
| _ -> n + calcsum (n-1)
fun int -> int

let rec calcsumtr n acc = match n with
| 1 -> 1 + acc
| _ -> calcsumtr (n-1) (n + acc)
fun int -> int -> int

let rec power x n = match n with 
| 0 -> 1
| 1 -> x
| _ -> x * (power x (n-1))

let rec powertr x n acc = match n with 
| 0 -> 1
| 1 -> x*acc
| _ -> (powertr x (n-1) (x*acc))


```

### Chapter 4. Making Lists

- the empty list `[]` has neither head or tail.
- every non-empty list (at least one element) has a head and tail. For example, `[5]` has a head `5` and a tail `[]`.
- only 2 operators for lists: cons `::` to add a new head to a list, and append `@` to combine two lists together. 
- when a list has type `'a list` it means that we're not inspecting the individual elements of the list, so they could be of any type. Likewise with `'b list` etc. 

returns a list of only the elements at odd indices in the input list:
```ocaml
let rec odd_elements l = match l with 
| [] -> [] 
| [a] -> [a] 
| a::_::t -> a :: odd_elements t 
```

#### Questions
```ocaml
let rec evens l = match l with
| [] -> []
| [x] -> []
| h::h2::t -> h2 :: evens t

let rec count_true l count = match l with
| [] -> count 
| h::t -> if h = true then count_true t (count+1) else count_true t count

let rec drop_last l n = match l with
| [] -> []
| h::t -> if t = [] then n else drop_last t (n @ [h])


```

#### Two Different Ways of Thinking
Think of each match case as an independent statement, while assuming that the recursion works. If the statements appear to be valid, then you're good to go (ish).

### Chapter 5. Sorting Things
```ocaml
let rec sort l n =
let rec insert x l = match l with
| [] -> [x]
| h::t -> 
	if x <= h 
		then x :: h :: t 
		else h :: insert x t 
in
match l with 
| [] -> []
| h::t -> insert h (sort t)

```

### Chapter 6. Functions upon Functions upon Functions
```ocaml
(* apply input function f to each element of the list *)
let rec map f l = 
	match l with 
	| [] -> [] 
	| h::t -> f h :: map f t
(* type of map: ('a -> 'b ) -> 'a -> 'b *)
```

```ocaml

let rec filter f l = match l with
	| [] -> []
	| h::t -> if (f h) = true then h :: filter f t else filter f t 

```

### Chapter 7. When Things Go Wrong
```ocaml
let rec smallest l = match l with
	| [] -> raise Not_found
	| [x] -> if x >= 0 && x <> 0 then x else raise Not_found
	| h1::h2::t -> if h1 >= h2 && h2 >= 0 then smallest (h2::t) else smallest (h1::t)



```