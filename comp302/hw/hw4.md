# Notes
**Check out how the change thing works for backtracking and try to do smth like that.**

# My Code
```ocaml
(* formula generators *)
let formulaTest1 = parse_formula "a"
let formulaTest2 = parse_formula "~a"
let formulaTest3 = parse_formula "((a | b) | c) | d" 
let formulaTest4 = parse_formula "(a & b) & (c & d)"
let formulaTest5 = parse_formula "(a & ~b)"
let formulaTest6 = parse_formula "~(a & b)"
let formulaTest7 = parse_formula "a & b | c"
let formulaTest9 = parse_formula "~~~~~~~~(a & b)"
let formulaTest10 = parse_formula "a & (b & a) & ~a"




(** Question 1 *)

(* TODO: Add test cases. *)
let collect_variables_tests : (formula * Variable_set.t) list = [ 
  (formulaTest1, Variable_set.singleton "a");
  (formulaTest2, Variable_set.singleton "a");
  (formulaTest3, Variable_set.singleton "a" |> Variable_set.add "b" |> Variable_set.add "c" |> Variable_set.add "d");
  (formulaTest4, Variable_set.singleton "a" |> Variable_set.add "b" |> Variable_set.add "c" |> Variable_set.add "d");
  (formulaTest5, Variable_set.singleton "a" |> Variable_set.add "b");
  (formulaTest9, Variable_set.singleton "a" |> Variable_set.add "b"); 
]

(* TODO: Implement the function. *)
let rec collect_variables (formula : formula) : Variable_set.t =
  let collection = Variable_set.empty in 
  match formula with 
  | Variable (x) -> Variable_set.add x collection
  | Conjunction (x,y) -> 
      Variable_set.union (Variable_set.union (collect_variables x) (collect_variables y)) collection 
  | Disjunction (x,y) ->
      Variable_set.union (Variable_set.union (collect_variables x) (collect_variables y)) collection 
  | Negation (x) -> Variable_set.union (collect_variables x) collection

(** Question 2 *)

(* TODO: Add test cases. *)
let eval_success_tests : ((truth_assignment * formula) * bool) list = [
  ((Variable_map.singleton "x" true, Variable("x")), true);
  ((Variable_map.singleton "x" false, Variable("x")), false); 
  ((Variable_map.singleton "x" true, Negation(Variable("x"))), false); 
  ((Variable_map.singleton "x" false, Negation(Variable("x"))), true); 
  (((Variable_map.singleton "x" true |> Variable_map.add "y" false, Conjunction(Variable("x"), Variable("y"))), false));
  (((Variable_map.singleton "x" true |> Variable_map.add "y" true, Conjunction(Variable("x"), Variable("y"))), true));
  (((Variable_map.singleton "x" false |> Variable_map.add "y" true, Conjunction(Variable("x"), Variable("y"))), false));
  (((Variable_map.singleton "x" false |> Variable_map.add "y" false, Conjunction(Variable("x"), Variable("y"))), false));
  (((Variable_map.singleton "x" true |> Variable_map.add "y" false, Disjunction(Variable("x"), Variable("y"))), true));
  (((Variable_map.singleton "x" true |> Variable_map.add "y" true, Disjunction(Variable("x"), Variable("y"))), true));
  (((Variable_map.singleton "x" false |> Variable_map.add "y" true, Disjunction(Variable("x"), Variable("y"))), true));
  (((Variable_map.singleton "x" false |> Variable_map.add "y" false, Disjunction(Variable("x"), Variable("y"))), false));
  (((Variable_map.singleton "x" true |> Variable_map.add "y" false |> Variable_map.add "z" false, Disjunction(Variable("x"), Negation(Conjunction(Variable("y"), Variable("z")))))), true);
  (((Variable_map.singleton "x" true |> Variable_map.add "y" false |> Variable_map.add "z" false, Conjunction(Variable("x"), Negation(Conjunction(Variable("y"), Variable("z")))))), true);
  
  ((Variable_map.singleton "a" true |> Variable_map.add "b" true, formulaTest10), false);
  
]

(* TODO: Add test cases. *)
let eval_failure_tests : ((truth_assignment * formula) * exn) list = [
  (((Variable_map.singleton "x" true, Conjunction(Variable("x"), Variable("y"))), Unassigned_variable "y"));
  (((Variable_map.singleton "x" true, Conjunction(Variable("x"), Negation(Conjunction(Variable("y"), Variable("z")))))), Unassigned_variable "z"); 


]

(* TODO: Implement the function. *)
let rec eval (state : truth_assignment) (formula : formula) : bool =
  match formula with 
  | Conjunction (x,y) -> (eval state x) && (eval state y)
  | Disjunction (x,y) -> ((eval state x) || (eval state y)) && ((eval state y) || (eval state x))
  | Negation (x) -> not (eval state x)
  | Variable (x) -> 
      let s = x in
      match (Variable_map.find_opt x state) with
      | Some bool -> bool
      | None -> raise (Unassigned_variable s)

(** Question 3 *)

(* TODO: Add test cases. *)
let find_satisfying_assignment_tests : (formula * truth_assignment option) list = [ 
  (Variable("x"), Some (Variable_map.singleton "x" true));
  
]

let merge_yes m1 m2 =
  if (Variable_map.cardinal m1) < (Variable_map.cardinal m2) then
    Variable_map.fold Variable_map.add m1 m2
  else
    Variable_map.fold Variable_map.add m2 m1

(* TODO: Implement the function. *)
let find_satisfying_assignment (formula : formula) : truth_assignment = 
  let elements_map = Variable_map.empty in
  let elements_list = (collect_variables (formula)) |> Variable_set.elements in 
  let rec add_to_map (el_list : 'a list) : truth_assignment = 
    match el_list with 
    | [] -> elements_map
    | x :: xs -> merge_yes (elements_map |> Variable_map.add x true) 
                   (add_to_map xs) in 
  
  let truth_assignment = add_to_map elements_list in
  if (eval truth_assignment formula ) = true then truth_assignment 
  else 
    raise Unsatisfiable_formula

```
# Instructions
### SAT
In this assignment, you will use exception-based backtracking to solve a well-known problem called _SAT_. SAT is short for "boolean satisfiability." Simply put, if you have a boolean formula with variables, SAT asks if there is any way to assign `true` and `false` to each variable so that the value of the formula is `true`. In this assignment, we write `&` for boolean AND, `|` for boolean OR, and `~` for boolean NOT.

For example, the formula `x & ~y` is satisfiable, by assigning `x` to `true` and `y` to `false`. The formula `x & ~x` is not satisfiable, because no matter what value we give to `x`, `x & ~x` will be `false`.

### Boolean Formulae

In HW2, you worked with expression trees of `float`s. In this assignment, you will work with a different type, `formula`, of `bool`ean expressions. The biggest difference is that in HW2, the tree had one unnamed variable. In this HW, the trees can have many named variables.

The `Conjunction` and `Disjunction` constructors represent boolean AND and boolean OR, respectively.

When `eval`uating expressions in HW2, you had to pass in a value to use for that variable. In this HW, the trees have many named variables. When `eval`uating expressions, you will have to use a _dictionary_ from variable names to boolean values. These dictionaries are called _truth assignments_. You will also want to know all of the variables which appear in an expression, which is a set of names.

In the Prelude for this assignment, we've given you a function `parse_formula : string -> formula`. You can use it however you want, including to write tests. It parses expressions like those shown above, including `"(a | ~b) & c"`, `"~~a"` and `"a1 | a2 | a3"`. Try playing with it in the toplevel! Note that LearnOCaml compiles your tests in your browser and does not always optimize, so larger input strings may cause the parser to run out of memory. We tested it successfully, but your mileage may vary.

You **are not** required to understand how the parser works, but feel free to look around the implementation if you are interested.

### `Variable_set` and `Variable_map`

In order to make sets of variables and dictionaries of variables, we have defined the `Variable_set` and `Variable_map` modules for you. The interface is very similar to simplified one you saw in class. These modules are defined using OCaml's standard [`Set.Make`](https://v2.ocaml.org/api/Set.Make.html) and [`Map.Make`](https://v2.ocaml.org/api/Map.Make.html) functors. The links in the previous sentence will take you to their documentation.

For example, we could define the set of variables `{x,y,z}` as

```
let xyz : Variable_set.t
    =  Variable_set.singleton "x"
    |> Variable_set.add "y"
    |> Variable_set.add "z";;
```

- Note that the `|>` operator basically works like the `|` operator in bash. 
	- The basic idea is that `e |> f = f e`.
	- Instead of writing `let x = 12 in e`, you could write `12 |> fun x -> e`

We could also define the truth assignment that maps `x` to `true`, and `y` and `z` to `false`:

```
let xyz_truth_asgn : truth_assignment
    =  Variable_map.singleton "x" true
    |> Variable_map.add       "y" false
    |> Variable_map.add       "y" false
```

You are encouraged to define some constant variable sets and truth assignments to use in your tests.

## Question 1 (10 points)

Implement the function `collect_variables : formula -> Variable_set.t`.

This function takes a formula and returns the set of variable names which appear anywhere in the formula. You may use any functions you want and whichever recursive style you prefer.

## Question 2 (15 points)

Implement the function `eval : truth_assignment -> formula -> bool`.

This function is very similar to the `eval` function from HW2. However, now you are evaluating boolean formulae instead of float arithmetic. You also need to use the correct boolean value for each variable. We recommend using `Variable_map.find_opt` to perform lookups in the truth assignment. If a variable is needed but does not appear in the truth assignment, you must raise an `Unassigned_variable x` exception, where `x` is the unassigned variable.

Once again, you may use any functions you want and whichever recursive style you prefer.

The test cases you must implement for this problem are split into two lists. `eval_success_tests` is a list of tests for when `eval` does not raise an exception and yields a `bool`, and `eval_failure_tests` is a list of tests for when `eval` raises an exception.

## Question 3 (25 points)

Implement the function `find_satisfying_assignment : formula -> truth_assignment`.

This function takes a `formula` and produces a `truth_assignment` which makes the formula evaluate to `true`.

-   If no such assignment exists, you must raise an `Unsatisfiable_formula` exception.
-   The truth assignment you return must be _sharp_, which means it must have an assignment for every variable in the expression and no others.
-   You can (and should) use the functions from both previous problems.
-   You must use exception-based backtracking search to implement this function. The autograder **will not check this**.

The test cases you must implement for this problem use `truth_assignment option` as output type. A `None` output corresponds to `find_satisfying_assignment` raising an exception on an unsatisfiable formula given as input. A `Some t` output corresponds to `find_satisfying_assignment` succeeding with `t` as satisfying truth assignment.

## Further Reading

SAT turns out to be a surprisingly hard problem. But it's very useful - a _huge_ variety of practical problems can be encoded as SAT problems. Examples include the longest path problem, register allocation, OCaml type inference, and many others.

If you're interested in the SAT problem and how high-powered solvers tackle it, the paper _A survey of SAT solver_ [W. Gong, X. Zhou 2017] is a good starting point./
