Let’s introduce a new quaternary relation `env |- e : t -| C`, which should be read as follows: “in environment `env`, expression `e` is inferred to have type `t` and generates constraint set `C`.” A constraint is an equation of the form `t1 = t2` for any types `t1` and `t2`.

If we think of the relation as a type-inference function, the colon in the middle separates the input from the output. The inputs are `env` and `e`: we want to know what the type of `e` is in environment `env`. The function returns as output a type `t` and constraints `C`. 

Rule for `if` expressions:
```
env |- if e1 then e2 else e3 : 't -| C1, C2, C3, t1 = bool, 't = t2, 't = t3
  if fresh 't
  and env |- e1 : t1 -| C1
  and env |- e2 : t2 -| C2
  and env |- e3 : t3 -| C3
```

Rule for anonymous functions: 
```
Since there is no type annotation on `x`, its type must be inferred:

env |- fun x -> e : 't1 -> t2 -| C
  if fresh 't1
  and env, x : 't1 |- e : t2 -| C
```

Rule for function application:
```
env |- e1 e2 : 't -| C1, C2, t1 = t2 -> 't
  if fresh 't
  and env |- e1 : t1 -| C1
  and env |- e2 : t2 -| C2
```

Type unification algorithm: 
- Until the constraint set is empty:
	- Pick and remove a constraint `t1 = t2` from the set 
	- Reduce based on `t1` and `t2` 
		- Update solution with new substitution, add new constraints back to the set, or fail if there are any inconsistencies. 
- Basically, the purpose of this is to simplify the constraints when possible and reduce the number of type variables. 

Worked example:
```
fun f -> fun x -> f (( + ) x 1)

1. f:t1 |- fun x -> f (( + ) x 1)
2. f:t1, x:t2 |- f (( + ) x 1)
3. f:t1, x:t2 |- f (( + ) x 1) => a / C1, C2, a1 = a2 -> a
	a. f:a1 = t1
	b. (( + ) x 1)
	   (( + ) x) : int -> int
		
```