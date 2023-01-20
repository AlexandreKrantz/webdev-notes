```ocaml
(* Question 1 *)

(* TODO: Write a good set of tests for {!q1a_nat_of_int}. *)
let q1a_nat_of_int_tests : (int * nat) list = [(0, Z); (5, (S (S (S (S (S(Z)))))))]

  
(* TODO:  Implement {!q1a_nat_of_int} using a tail-recursive helper. *)
let q1a_nat_of_int (n : int) : nat = 
  match n with
  | 0 -> Z
  | _ -> 
      let rec q1a_nat_of_int_helper (tally : nat) (count : int) = 
        if count = 1 then tally else q1a_nat_of_int_helper (S (tally)) (count - 1)
      in 
      q1a_nat_of_int_helper (S Z) (n)

(* TODO: Write a good set of tests for {!q1b_int_of_nat}. *)
let q1b_int_of_nat_tests : (nat * int) list = [(Z, 0); ((S (S (S (S (S Z))))), 5)]

(* TODO:  Implement {!q1b_int_of_nat} using a tail-recursive helper. *)
let rec q1b_int_of_nat (n : nat) : int = 
  match n with 
  | Z -> 0
  | _ -> 
      let rec q1b_int_of_nat_helper (tally : nat) (count: int) (tallycount: nat) =
        if tally = tallycount then count else q1b_int_of_nat_helper tally (count+1) (S (tallycount))
      in
      q1b_int_of_nat_helper n 0 Z

(* TODO: Write a good set of tests for {!q1c_add}. *) 
let q1c_add_tests : ((nat * nat) * nat) list = [(((S(Z)), (S(S(Z)))), (S(S(S(Z)))))]

(* TODO: Implement {!q1c_add}. *)
let rec q1c_add (n : nat) (m : nat) : nat = 
  match n with
  | Z -> m
  | S(n') -> let h' = q1c_add n' m in
      S(h')


(* Question 2 *)

(* TODO: Implement {!q2a_neg}. *)
let q2a_neg (e : exp) : exp = 
  Times(e, Const(-1.0))

(* TODO: Implement {!q2b_minus}. *)
let q2b_minus (e1 : exp) (e2 : exp) : exp = 
  Plus(e1, (q2a_neg (e2)))

(* TODO: Implement {!q2c_quot}. *)
let q2c_quot (e1 : exp) (e2 : exp) : exp = 
  Times(e1, Pow(e2, -1))


(* Question 3 *)
let e1 = (Plus(Const(2.0), Var), 5.0)
(* TODO: Write a good set of tests for {!eval}. *)
let eval_tests : ((float * exp) * float) list = [((3.0, Plus(Const(2.0), Var)), 5.0)]

(* TODO: Implement {!eval}. *)
let rec eval (a : float) (e : exp) : float = 
  match e with 
  | Const(n') -> n'
  | Var -> a 
  | Plus(m', q') -> (eval a m') +. (eval a q')
  | Times(w', e') -> (eval a w') *. (eval a e')
  | Pow(r', t') -> (eval a r') ** (float_of_int t')


(* Question 4 *)

(* TODO: Write a good set of tests for {!diff_tests}. *)
let diff_tests : (exp * exp) list = [(Pow(Var,2), Times(Times(Const(2.0), Pow(Var,1)), Const(1.0))); (Times(Plus(Var, Const(3.0)), Plus(Times(Const(2.0), Var), Const(3.0))), (Plus
                                                                                                                                                                                 (Times (Plus (Const 1., Const 0.), Plus (Times (Const 2., Var), Const 3.)),
                                                                                                                                                                                  Times (Plus (Var, Const 3.),
                                                                                                                                                                                         Plus (Plus (Times (Const 0., Var), Times (Const 2., Const 1.)), Const 0.)))))]

(* TODO: Implement {!diff}. *)

let rec diff (e : exp) : exp = 
  match e with 
  | Const(n') -> Const(0.0)
  | Var -> Const(1.0)
  | Plus(m', q') -> Plus((diff m'), (diff q'))
  | Times(w', e') -> Plus(Times((diff w'), e'), Times(w', (diff e')))
  | Pow(r', t') -> Times(Times(Const(float_of_int t'), Pow((r'), (t' - 1))), diff r')

```