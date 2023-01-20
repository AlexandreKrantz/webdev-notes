```ocaml
(* Question 1: Manhattan Distance *)
(* TODO: Write a good set of tests for distance. *)
let distance_tests = [
    (* Your test cases go here *) 
  (((0,0), (0,0)), 0) ;
  (((2,5), (0,2)), 5) ;
  (((0,2), (2,5)), 5) ;
  (((4,7), (5,6)), 2)

] ;;

(* TODO: Correct this implementation so that it compiles and returns
         the correct answers.
*)
let distance (x1, y1) (x2, y2) = 
  let xdiff = if x2 - x1 < 0 then x1 - x2 else x2 - x1 in
  let ydiff = if y2 - y1 < 0 then y1 - y2 else y2 - y1 in 
  xdiff + ydiff;


(* Question 2: Binomial *)
(* TODO: Write your own tests for the binomial function.
         See the provided tests for fact, above, for how to write test cases.
         Remember that we assume that  n >= k >= 0; you should not write test cases where this assumption is violated.
  *)
  let binomial_tests = [
  (* Your test cases go here. Correct the incorrect test cases for the function. *)
    ((2, 1), 2);
    ((5, 0), 1);
    ((3, 2), 3);
    ((3, 3), 3); 
  ]

(* TODO: Correct this implementation so that it compiles and returns
         the correct answers.
*)
let binomial n k =
  raise NotImplemented



(* Question 3: Lucas Numbers *)

(* TODO: Write a good set of tests for lucas_tests. *)
let lucas_tests = [
]

(* TODO: Implement a tail-recursive helper lucas_helper. *)
let rec lucas_helper params =
  raise NotImplemented


(* TODO: Implement lucas that calls the previous function. *)
let lucas n =
  raise NotImplemented

```