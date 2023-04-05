double sided crib sheet allowed for the final
similar difficulty (or even easier) than the midterm
no short answer questions - either mc or long answers

## defunctionalization
Basically: what the cpu does to get rid of the higher order functions (because cpu only deals with normal jump instructions). 

Enables you to refactor code patterns.

You can't send a function over a network. So instead you represent the function as a data type and send the data type over the server, the client then reinterprets the function. 

```ocaml
type IntPredicate = 
	| isEven

let apply (p : IntPredicate) (n : int) : bool = 
	match p with 
	| isEven -> n mod 2 = 0 
```


expression steps if it can rewritten according to the operational semantics of the language
eg.: 
`1 + (`