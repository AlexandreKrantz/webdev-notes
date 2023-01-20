```ocaml
(* Q1: 5 points *)
let traverse_tests : (((int -> int option) * int list) * int list option) list = [
  (((fun x -> Some(x*2)), [1;2;3;4;5]), Some([2;4;6;8;10]));
  (((fun x -> if (x mod 2) = 0 then Some(x) else None), [0;2;4;6]), Some([0;2;4;6]));
  (((fun x -> if (x mod 2) = 0 then Some(x) else None), [0;2;3;6]), None)

]

  
let rec traverse (f : 'a -> 'b option) (l : 'a list) : 'b list option =
  match l with 
  | [] -> Some([])
  | h::t -> 
      match f h, (traverse f t) with
      | Some(x), Some(tl) -> Some(x :: tl)
      | None, _ -> None
      | _, None -> None
```

```ocaml

let rec traverse' (f : 'a -> 'b Maybe.t) (l : 'a list) : 'b list Maybe.t =
  let open Maybe in
  match l with 
  | [] -> return []
  | h::t -> bind (f h) (fun y -> bind (traverse' f t) (fun z -> return (y::z))) 

```

```ocaml
(* Q2.1: 2 points *)
let ap (mx : 'a Maybe.t) (fm : ('a -> 'b) Maybe.t) : 'b Maybe.t =
  let open Maybe in 
  bind mx (fun x -> bind fm (fun y -> return (y x)))

```