``` ocaml
n : 'b -> ('b -> 'b) -> 'b

fun z s -> n z (fun a -> s (s a))
```
What is a church numeral: 2 = `S(S(Z))`
`'b` is a chruch numeral. 
The function multiplies the church numeral by 2. 
**^ IDK how this works**

church booleans: 
``` ocaml
type 'a cb = 'a -> 'a -> 'a 

let tt : 'a cb = fun a b -> a
let ff : 'a cb = fun a b -> b

let logical_and i1 i2 = fun a b -> i1 (i2 a b) b
let logical_or i1 i2 = fun a b -> i1 a (i2 a b)
```
	- you can test it out by plugging in combinations of tt and ff into the logical_and and logical_or functions.

#### other practice questions
```ocaml
(* N-ary trees. Each node can have a varying number of subtrees. *)
type 'a ntree = Tree of 'a * 'a ntree list
(* A leaf of the tree has an empty list of subtrees. *)

(* Recursively collapse a tree by applying `f` to the node's element and to the
list of collapsed subtrees. Use List.map as part of your implementation. *)
let rec fold (f : 'a -> 'b list -> 'b) (t : 'a ntree) : 'b =
  failwith "todo"

(* Sum all values of an int ntree using `fold` above. *)
let rec sum (t : int ntree) : int =
  failwith "todo"

(* Transform all the elements of a ntree, keeping the same ntree shape.
Implement this without pattern-matching, instead using `fold`.
Hint: since we want to output a `'b ntree`, we choose for the variable 'b in the
type of `fold` the type `'b ntree`.
So the more specific type of `fold` that we work with will be
`fold : ('a -> 'b ntree list -> 'b ntree) -> 'a ntree -> 'b ntree *)
let rec map (f : 'a -> 'b) (t : 'a ntree) : 'b ntree =
  failwith "todo"

```


