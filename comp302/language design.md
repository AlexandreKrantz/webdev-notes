Notes based on chapter 10 of the course textbook.

A formal specification of a programming language has 3 steps:
1. Grammar of the language (i.e. What are the syntactically legal expressions?) 
2. Operational dynamic semantics (i.e. How is a given program executed?) 
3. Static semantics (= type system) (i.e. When is a given program well-typed? What can we statically say about the execution of a program without actually executing it?)

## Grammar
We define a set of expressions inductively with the Backus-Naur Form
```
Operations op ::= + | − | ∗ |<|= 
Expressions e ::= n | e1 op e2 | true | false | if e then e1 else e2
Values v ::= n | true | false
```
- Expressions that respect this definition are called "well-formed". Eg: `if true then (if false then 5 else 1 + 3) else 2 = 5`
	- Note that this expression is not well-typed. Grammar only tells us when expressions are syntactically well-formed. Grammar does not say anything whether a given expression is well-typed.
- Expressions that violate the definition are "ill-formed": `+23`

- Expression evaluate to a value `v`
### Implementing a simple evaluator
See textbook: ctrl-f "10.3 Implementing a simple evaluator"

Type checking approximates runtime behavior. It allows us to statically check whether expressions will lead to a runtime error. 
Type checkers give precise error messages pointing the programmer to the specific line in the code where the reasoning went wrong. This is so much easier than hunting down a bug. 

```
Types T ::= int | bool
```
	- more info on pdf page 108

We can extend our definition for expressions to allow for variables and `let` expressions.
```
Expressions e ::= . . . | x | let x = e1 in e2 end
```

### Free variables and substitutions
pdf page 110. 
Intuitively, a variable is considered free, if it is not bound.

- We will define a function FV which takes as input an expression e and returns a set of the free variables occurring in e.
- An expression e which has no free variables, i.e. FV(e) = ∅ is called closed.
- FV(let x = e1 in e2 end) = FVe1 ∪ (FV(e2)/ {x})
	- This implies that `x` is bounded by the the first part of the let expression, so it is no longer considered a free variable. 

Intuitively, when given an expression `let x = e1 in e2` end, we first evaluate the expression `e1` to some value `v1`. Next, we replace all free occurrences of `x` in the expression `e2` by the value `v1` and continue to evaluate `[v1/x]e2` to some value `v2`.
![](Pasted%20image%2020221115201830.png)

### Functions and function application
Now let's extend the possible expressions to include functions. Both the recursive and non-recursive kind:
```
Expressions e ::= . . . | fn x => e | e1 e2 | rec f => e

note that:
FV(fn x => e) = FV(e)/ {x} 
FV(rec f => e) = FV(e)/ {f} 
```
	- The definition highlights that both fn x => e and rec f => e bind variables. In the first case of fn x => e the input variable x is bound, and in the second case of rec f => e the function name is bound.

