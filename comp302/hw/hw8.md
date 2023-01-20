```ocaml
(* TODO: Q1a *)
let rec take (n : int) (s : 'a stream) : 'a list =
  match n with 
  | 0 -> []
  | 1 -> [s.head]
  | _ -> [s.head] @ (take (n-1) (s.tail ()))

(* TODO: Q1b *)
let rec drop (n : int) (s : 'a stream) : 'a stream =
  match n with
  | 0 -> s
  | 1 -> s.tail ()
  | _ -> drop (n-1) (s.tail ())

(* TODO: Q2a *)
let zeroes : int stream = 
  unfold (fun seed -> (0, seed)) (fun seed -> (0, seed))

(* TODO: Q2b *)
let natural_numbers : int stream =
  unfold (fun seed -> (seed, seed+1)) 0

(* TODO: Q2c *)
let even_numbers : int stream =
  unfold (fun seed -> (seed, seed+2)) 0

(* TODO: Q3 *)
let map2 (f : 'a -> 'b -> 'c) (s1 : 'a stream) (s2 : 'b stream) : 'c stream =
  let f2 = fun (a,b) -> f a b in
  let s3 = zip s1 s2 in
  map f2 s3

(* TODO: Q4a *)
let fibonacci : int stream = 
  let f = fun cur next -> (cur, next) in
  unfold (fun seed -> match seed with | (cur, next) -> cur, f next (cur+next)) (f 0 1)


(* TODO: Q4b *)
let lucas : int stream =
  let f = fun cur next -> (cur, next) in
  unfold (fun seed -> match seed with | (cur, next) -> cur, f next (cur+next)) (f 2 1)


```