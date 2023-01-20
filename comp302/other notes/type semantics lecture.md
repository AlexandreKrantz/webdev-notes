## Operational Semantics
```ocaml
type prim = LT | EQ | Neg | Plus | Not

type exp =
  | I of int
  | B of bool
  | If of exp * exp * exp
  | Prim of prim * exp list
              
exception RuntimeError of string
              
let rec eval (e : exp) : exp = match e with
  | I n -> I n
  | B b -> B b
  | If (e, e1, e2) -> begin match eval e with
      | B b -> if b then eval e1 else eval e2
      | _ -> raise (RuntimeError "condition not a boolean")
    end
  | Prim (op, es) ->
      eval_prim op (List.map eval es)
        
and eval_prim op vs : exp = match op, vs with
  | LT, [I n1; I n2] -> B (n1 < n2)
  | EQ, [I n1; I n2] -> B (n1 = n2)
  | EQ, [B b1; B b2] -> B (b1 = b2)
  | Neg, [I n] -> I (0 - n)
  | Not, [B b] -> B (not b)
  | Plus, [I n1; I n2] -> I (n1 + n2)
  | _ -> raise (RuntimeError "primop mismatch")        

```

Note: `begin` and `end` are used just like multi-line parentheses. 
This code:
```ocaml
If (e, e1, e2) -> begin match eval e with
      | B b -> if b then eval e1 else eval e2
      | _ -> raise (RuntimeError "condition not a boolean")
    end
```
Is equivalent to this:
```ocaml
If (e, e1, e2) -> ( match eval e with
      | B b -> if b then eval e1 else eval e2
      | _ -> raise (RuntimeError "condition not a boolean")
    )
```

## Static Semantics
Static semantics can be checked prior to evaluation. 
For example, type inference. 




```ocaml
(* Title: Lecture notes on programming language implementation & theory.
   Author: Jacob Thomas Errington
   Date: Fall 2022 *)

(* Syntax of types.

   Types T ::= Int | Bool | T1 -> T2 *)
type tp =
  | Int
  | Bool
  | Arrow of tp * tp


(* Syntax of primitive operations.

   Primops op ::= < | = | - | + | not *)
type prim = LT | EQ | Neg | Plus | Not

(* Type synonym to distinguish variable names from other strings. *)
type name = string

(* Syntax of expressions. *)
type exp =
  | I of int                (* ... | -1 | 0 | 1 | ... *)
  | B of bool               (* true | false *)
  | If of exp * exp * exp   (* if e then e1 else e2 *)
  | Prim of prim * exp list (* op e OR e1 op e2 *)
  | Var of name             (* x *)
  | Let of name * exp * exp (* let x = e1 in e2 *)

  (* Compared to what we saw in class, these two are added. *)
  | Fun of name * tp * exp  (* fun (x : T) -> e *)
    (* Functions will require a type annotation on their input. Otherwise type
    inference will not be possible unless we introduce polymorphism, which is
    more complicated. *)
  | App of exp * exp        (* e1 e2 -- application of a function to an argument *)
  | Clo of env * name * exp (* No real syntax for this. *)
  (* This last constructor represents a _closure_. The exp is the _body_ of
     a function, the name is the name of the free variable in that body,
     and the env is the environment at the moment that the function was
     evaluated.

     The idea is that when we want to evaluate a function `fun x -> e`,
     we take the current environment `r` and construct a little bundle
     (r, x, e) -- this is the closure. Notice we do not evaluate `e` yet!

     Then, when we want to evaluate an application, e.g. e1 e2, we require that
     the LHS evaluates to closure, e.g. (r, x, e), and evaluate e2 to a value v.
     Then, to finally evaluate the body of the function `e`, we can "go back
     in time" by using the stored environment `r`, extending it with a binding
     for the free variable `x` to form `(x, v) :: r`. *)

(* Since environments appear in the syntax of expressions now, we must
   define `env` mutually recursive with `exp` by using `and`. *)
and env = (name * exp) list

exception UnboundVariable of name

let lookup_var x l =
  match List.assoc_opt x l with
  | None -> raise (UnboundVariable x)
  | Some v -> v

exception RuntimeError of string

let rec eval (r : env) (e : exp) : exp = match e with
  | I n -> I n
  | B b -> B b
  | If (e, e1, e2) -> begin match eval r e with
      | B b -> if b then eval r e1 else eval r e2
      | _ -> raise (RuntimeError "condition not a boolean")
    end
  | Prim (op, es) ->
      eval_prim op (List.map (eval r) es)
  | Let (x, e1, e2) ->
      let v = eval r e1 in (* to be CBN, don't eval e1! Store it as is in env *)
      let r' = (x, v) :: r in
      eval r' e2
  | Var x -> lookup_var x r (* to be CBN, eval variable here *)

  (* These are the new cases to handle functions: *)

  | Fun (x, _tp, e) -> Clo (r, x, e) (* We store the current env inside the closure. *)
  | Clo (r, x, e) -> Clo (r, x, e) (* We should not actually encounter Clo during evaluation. *)

  (* The logic here is summarized above in the syntax of closures. *)
  | App (e1, e2) -> begin match eval r e1 with
    | Clo (r_clo, x, e) ->
      let v = eval r e2 in
      eval ((x, v) :: r_clo) e
    | _ -> raise (RuntimeError "App LHS not a closure")
  end

and eval_prim op vs : exp = match op, vs with
  | LT, [I n1; I n2] -> B (n1 < n2)
  | EQ, [I n1; I n2] -> B (n1 = n2)
  | EQ, [B b1; B b2] -> B (b1 = b2)
  | Neg, [I n] -> I (0 - n)
  | Not, [B b] -> B (not b)
  | Plus, [I n1; I n2] -> I (n1 + n2)
  | _ -> raise (RuntimeError "primop mismatch")

type type_error =
  | ConditionNotBool of tp
  | BranchesMismatched of tp * tp
  | WrongArgument of tp * tp (* trying to apply a function to the wrong type of thing *)
  | AppLhsNotFunction of tp (* trying to apply smth that isn't a function *)

exception TypeError of type_error

(* In the presence of variables, we need something similar to an env above to
   track what type is associated with each variable.
   Whereas for the evaluator this is called an _environment_, we call what the
   typechecker uses a _context_. *)
type ctx = (name * tp) list

(* We use the letter g for the context, because in papers it usually the greek
letter Gamma. *)
let rec infer (g : ctx) (e : exp) : tp = match e with
  | I _ -> Int
  | B _ -> Bool
  | If (e, e1, e2) ->
      begin match infer g e with
        | Bool ->
            let t1 = infer g e1 in
            let t2 = infer g e2 in
            if t1 = t2 then t1 else raise (TypeError (BranchesMismatched (t1, t2)))
        | t ->
            raise (TypeError (ConditionNotBool t))
      end
  | Prim (op, es) -> infer_prim op (List.map (infer g) es)

  (* Here are the new cases for handling let-in expressions and functions: *)
  | Let (x, e1, e2) ->
    let t = infer g e1 in (* this will be the type of the variable x in the body, e2. *)
    infer ((x, t) :: g) e2

    (* The typing rule for let-expressions needs to consider the context as it
       changes when we infer the type of the body.

       The typing rule will look like:     G |- e : T
       meaning "In the context G, expression e has type T"

       The syntax of G is the following:

       Contexts G ::= . | G, x : T

       That is, either a context is empty (.) or it is extended with a new
       declaration x : T associating a variable "x" to a type "T".


        G |- e1 : T1       G, x : T1 |- e2 : T
       ----------------------------------------
               G |- let x = e1 in e2 : T
    *)

  | Fun (x, t, e) ->
    (* Now you see why we needed the type annotation on `x`: it allows us to form
       an extended context in which to infer the type of the body `e`. *)
    Arrow (t, infer ((x, t) :: g) e)

    (* The typing rule for functions is the following:

                 G, x : T1 |- e : T2
       ---------------------------------------
          G |- fun (x : T1) -> e : T1 -> T2
    *)

  | Var x -> (* when we encounter a variable, we simply look up its type in the context. *)
    (* This is very similar to how we evaluate a variable above. *)
    lookup_var x g

    (* Typing rule for variables:

           G(x) = T
       ----------------
          G |- x : T

       The syntax G(x) means to look up x in the context G.
    *)

  | App (e1, e2) ->
    (* To infer the type of an application:
       - infer the type of the LHS, call it t
       - ensure that t is an Arrow t1 -> t2
       - infer the type of the RHS, call it t_arg
       - ensure that t_arg = t1 (compatible argument type)
       - return t2 as the result type of the application

       These steps can be figured out from / are essentially summarized in the
       typing rule for applications:

         e1 : T1 -> T2       e2 : T1
       --------------------------------
                e1 e2 : T2
     *)
     begin match infer g e1 with
     | Arrow (t1, t2) ->
       let t1' = infer g e2 in
       if t1 = t1' then t2 else raise (TypeError (WrongArgument (t1, t1')))
     | t ->
       raise (TypeError (AppLhsNotFunction t))
     end

  | Clo (_, _, _) -> failwith "Clo is impossible"
    (* Closures are only created during evaluation, which only happens AFTER we
       typecheck the program. So the typechecker should NEVER see any closures.
       If this happens, it is not a type error in the program, but rather a
       coding error in our typechecker.
    *)

and infer_prim op (ts : tp list) : tp = match op, ts with
  | Plus, [Int; Int] -> Int
  | Not, [Bool] -> Bool
  | EQ, [Int; Int] -> Bool (* if we allow only EQ on ints *)
  | EQ, [t1; t2] when t1 = t2 -> Bool
  | _ -> failwith "todo"

(* Some syntax sugar to make writing programs easier below. *)
let let_ name exp body = Let (name, exp, body)
let fn (param, ty) body = Fun (param, ty, body)
let v x = Var x

(* And here is an example program featuring a higher-order function.
   In a more human-readable syntax, it looks like this:
   let compose =
     fun (f: int -> int) ->
     fun (g : int -> int) ->
     fun (x : int) ->
     f (g x)
   in
   let two = 2 in
   let plus2 = fun (x : int) -> x + two in
   let times2 = fun (x : int) -> x + x in
   let g = compose plus2 times2 in
   g 15
*)
let example =
  let_ "compose" begin
    fn ( "f", Arrow (Int, Int) ) @@
    fn ( "g", Arrow (Int, Int) ) @@
    fn ( "x", Int ) @@
    App (v "f", App (v "g", v "x"))
  end @@
  let_ "two" (I 2) @@
  let_ "plus2" begin
    fn ("x", Int) @@
    Prim (Plus, [v "x"; v "two"])
  end @@
  let_ "times2" begin
    fn ("x", Int) @@
    Prim (Plus, [v "x"; v "x"])
  end @@
  let_ "g" (App (App (v "compose", v "plus2"), v "times2")) @@
  App (v "g", I 15)

let ty = infer [] example (* should be Int *)
let result = eval [] example (* should be 32 *)
```