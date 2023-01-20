### reference (aka mutable variables)

OCaml has `for` and `while` loops
`!v` read the value of the reference (pointer) `v`
`v := newVal` updates the value of a reference

```ocaml
let fact n =
	let v = ref 1 in (* create reference variable; it has type `int ref` *)
	for i = 1 to n do (* the for loop has type `unit` because nothing is returned*)
		v := !v * i
	done;
	!v
```

Pros with for loop is that it's more performant in some cases; no need to worry about recursion. This is an imperative progam.

Cons is that unlike function programs, you can't prove validity with induction. 

If you have a `;` then you're gonna have two things: a expression on the left `e1 ` and an expression on the right `e2`. Usually `e1` is a side effect and `e2` is something you want to evaluate/return after the side effect  has occured. 

a new type `A ref` means a reference to a value of type `A`

`unit` is a "nothing" type. It can only have one value: `()`

```ocaml
let counter = ref 0

(* tick simply increments the value of counter. We only care about tick's side effects, so it doesn't need to take any inputs. tick also returns the updated value of the counter. *)
let tick () =
	counter := !counter + 1; (* do side effect. Add the ; so ocaml can move on *)
	!counter
```
You've got to call tick with `tick ()`

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

### records
Record data type: named tuples, kinda like dictionaries except they are *immutable*. Much like stucts in C.
[3.4. Records and Tuples — OCaml Programming: Correct + Efficient + Beautiful (cs3110.github.io)](https://cs3110.github.io/textbook/chapters/data/records_tuples.html)
```ocaml
type rec_counter = {
	tick : unit -> int; 
	reset : unit -> unit; 
}

let make_counter () : rec_counter = 
	let c = ref 0 in {
		tick = (fun () -> c := !c + 1);
		reset = (fun () -> c := 0)
	}

let c1 = make_counter ()
c1.tick()
c1.tick()

let c2 = make_counter ()
c2.tick()
```
