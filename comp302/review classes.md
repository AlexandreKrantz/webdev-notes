- async-await is actually continuations in disguise??
- CS is the science of abstraction. You go from electronic rocks that can do calculations all the way to things like YouTube with abstraction.
- Programming languages is basically interpreters all the way through to the cpu. If you understand interpreters, you pretty much understand the whole stack.

- OOP: data and functionality are coupled, easy to data new data (types)
- FP: data and functions are decoupled, easy to add new functionality. 
	- if you're working on a project where you going to be adding lots of new functionality, it might be good to use an FP paradigm in a language like python or JS. 
	- generally less bug prone

type inference/checking doesn't do evaluation. So infinite loops won't be detected. 

the type of a `let ... in ...` expression is the type of the stuff that comes after the `in`

#### Dec 1
```ocaml
let x = ref ( 3 * 2) in 
let r = x in
let f = 
  let r = ref 4 in
  let a = !x * 2 in
  fun u ->
  x := 1;
  u + !x + a + !r
in
let x = ref 7 in 
let a = 10 in
x := 2;
f (!r * a)
```
	- evaluates to 77 
- at the moment that `f` is defined, the body is evaluated immediately
	- only stuff after fun gets delayed to after we call `f`
- The `x` that is defined after `f` is not visible to `f`. `f` can only see the stuff that has been defined above it. 
- the `x` created after the function definition will shadow the original `x` and is separate from the stuff done in the function.
	- It actually doesn't do anything in the program; nothing uses it. 
- The type of `!` is `a ref -> a`
- 


idk...
```ocaml

type 'a bst =
  |Empty
  |Node of 'a bst * (int * 'a) * 'a bst
let rec insert (k, v) = function
  |Empty -> Node (Empty, (k,v), Empty)
  |Node (l,(k',v'),r) when k<k' -> Node

let rec lookup k = function 
| Empty => 
| Node (l, (k', _), _) when k < k' -> lookup k l 
| Node ()
```


#### fold
????