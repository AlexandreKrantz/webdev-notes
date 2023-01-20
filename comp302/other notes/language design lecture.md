## Grammar
![](Pasted%20image%2020221109120119.png)
![](Pasted%20image%2020221109120023.png)
![](Pasted%20image%2020221109120100.png)
#### Evaluation notation
![](Pasted%20image%2020221109120857.png)
![](Pasted%20image%2020221109122246.png)
- Reading the rule B-OP from the bottom to the top, this rules gives a recipe on how to evaluate an expression e1 op e2, by recursively evaluating its sub-expressions.
![](Pasted%20image%2020221109122538.png)


### Dynamic Semantics
![](Pasted%20image%2020221109141924.png)

### Type Checking
![](Pasted%20image%2020221109142204.png)

### Free Variables
let x = 5 in x + 1
- x = 5 is a binder. It binds x = 5 to the scope x + 1.
- x + 1 in isolation doesn't contain a binder, so when looking at x in this context we can say x is a "free variable".
- FV(e) returns the set of free variable names occuring in the expression e. It's defined inductively based on the structure of the expression e. 
![](Pasted%20image%2020221115151030.png)
- Substituation is an operation that eliminates a free variable from an expression.
![](Pasted%20image%2020221115151232.png)
![](Pasted%20image%2020221115151318.png)

"Capture-avoiding substitution"

