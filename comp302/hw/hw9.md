```
let myex1 = Rec("f", Arrow([Int; Int], Bool), 
                Fn([("x", Int); ("y", Int)], 
                   If(Primop(Equals, [Var("x"); Var("y")]), B(true), 
                      Apply(Var("f"), [Var("x"); Var("y")]))))
    
    
let myex2 = Fn ([("x", Int); ("y", Bool)], If (Var "y", I 0, I 1)) 
let myex3 = Fn ([("x", Int); ("y", Bool)], If (Var "y", Var "x", I 0))
let myex4 = Rec("f", Arrow([Int; Int], Bool), 
                Fn([("x", Int); ("y", Int)], 
                   If(Primop(Equals, [Var("x"); Var("y")]), B(true), 
                      Apply(Var("g"), [Var("x"); Var("y")]))))
    
    

(**To DO: Write a good set of tests for free_variables **)
let free_variables_tests = [
  (* An example test case.
     Note that you are *only* required to write tests for Let, Rec, Fn, and Apply!
  *)
  (Let ("x", I 1, I 5), []); 
  (Let ("x", I 1, Primop(Plus, [Var "x"; Var "y"])), ["y"]); 
  (Let ("x", Primop(Plus, [Var "a"; Var "b"]), Primop(Times, [Var "x"; I 2])), ["a"; "b"]);
  (Fn([("x", Int); ("y", Int)], Primop(Plus, [Var "x"; Var "y"])), []);
  (Fn([("x", Int); ("y", Int)], Primop(Plus, [Var "x"; Var "z"])), ["z"]); 
  (ex2, []);
  (ex3, []);
  (myex1, []);
  (ex4, []);
  (ex1, []);
  (ex5, []);
  (ex6, []);
  (ex7, []);
  (myex4, ["g"]);
]

(* TODO: Implement the missing cases of free_variables. *)
let rec free_variables : exp -> name list =
  (* Taking unions of lists.
     If the lists are in fact sets (all elements are unique),
     then the result will also be a set.
  *)
  let union l1 l2 = delete l2 l1 @ l2 in
  let union_fvs es =
    List.fold_left (fun acc exp -> union acc (free_variables exp)) [] es
  in
  function
  | Var y -> [y]
  | I _ | B _ -> []
  | If(e, e1, e2) -> union_fvs [e; e1; e2]
  | Primop (_, args) -> union_fvs args
  | Fn (xs, e) -> 
      let bodyvars = free_variables e in 
      let argnames = List.map (fun (name, tp) -> name) xs in 
      delete argnames bodyvars 
      
      
      (*let rec inFnArgs name argList = 
         match argList with
         | (a,b)::otherArgs -> if a = name then true else inFnArgs name otherArgs
         | [] -> false
       in 
       List.filter (fun el -> if inFnArgs el xs = true then false else true) (free_variables e) *)
  | Rec (x, _, e) ->
      List.filter (fun el -> if el = x then false else true) (free_variables e)
  | Let (x, e1, e2) -> let temp = List.filter (fun el -> if x = el then false else true) (free_variables e2) in 
      (free_variables e1) @ temp
  | Apply (e, es) -> union_fvs (e::es) 

let myex5 = Apply (Var "x", [Var "y"; I 5])
let myex5 = Apply (Var "x", [Let("y", I 5, Primop(Equals , [I 5; I 5]))])
let myex6 = Apply (Let ("x", I 5, I 5), [Let ("y", I 5, I 5); Var "z"])
let myex7 = Rec ("f", Bool, Var "f")
let myex8 = Rec ("f", Bool, Var "g")



(* TODO: Write a good set of tests for unused_vars. *)
let unused_vars_tests = [
  (* An example test case.
     Note that you are *only* required to write tests for Rec, Fn, and Apply!
  *)
  (Let ("x", I 1, I 5), ["x"]); 
  (myex2, ["x"]);
  (myex3, []); 
  (myex5, ["y"]);
  (myex6, ["x"; "y"]);
  (myex7, []);
  (myex8, ["f"])
  
]

(* TODO: Implement the missing cases of unused_vars. *)
let rec unused_vars =
  function
  | Var _ | I _ | B _ -> []
  | If (e, e1, e2) -> unused_vars e @ unused_vars e1 @ unused_vars e2
  | Primop (_, args) ->
      List.fold_left (fun acc exp -> acc @ unused_vars exp) [] args
  | Let (x, e1, e2) ->
      let unused = unused_vars e1 @ unused_vars e2 in
      if List.mem x (free_variables e2) then
        unused
      else
        x :: unused

  | Rec (x, _, e) -> 
      let unused = unused_vars e in 
      if (List.mem x (free_variables e)) = true then unused else x::unused
      
      
  | Fn (xs, e) -> 
      let free = free_variables e in 
      let unused_args = List.filter (fun (a,b) -> if List.mem a free = true then false else true) xs in
      List.map (fun (a,b) -> a) unused_args

  | Apply (e, es) -> 
      let unused_lists = List.map (fun exp -> unused_vars exp) es in
      let unused = List.fold_left (fun var_list acc -> var_list @ acc) [] unused_lists in
      unused @ unused_vars e

(* TODO: Write a good set of tests for subst. *)
(* Note: we've added a type annotation here so that the compiler can help
   you write tests of the correct form. *)
let subst_tests : (((exp * name) * exp) * exp) list = [
  (* An example test case. If you have trouble writing test cases of the
     proper form, you can try copying this one and modifying it.
     Note that you are *only* required to write tests for Rec, Fn, and Apply!
  *)
  (((I 1, "x"), (* [1/x] *)
    (* let y = 2 in y + x *)
    Let ("y", I 2, Primop (Plus, [Var "y"; Var "x"]))),
   (* let y = 2 in y + 1 *)
   Let ("y", I 2, Primop (Plus, [Var "y"; I 1])));
  
  (((I 1, "x"), (* [1/x] *)
    (* let y = 2 in y + x *)
    Apply(Var "y", [I 2; Primop (Plus, [Var "y"; Var "x"])])),
   (* let y = 2 in y + 1 *)
   Apply(Var "y", [I 2; Primop (Plus, [Var "y"; I 1])])); 
  
  (((I 1, "x"), (* [1/x] *)
    (* let y = 2 in y + x *)
    Rec("y", Int, Primop (Plus, [Var "y"; Var "x"]))),
   (* let y = 2 in y + 1 *)
   Rec("y", Int, Primop (Plus, [Var "y"; I 1 ]))); 
  
  (((I 1, "x"), (* [1/x] *)
    (* let y = 2 in y + x *)
    Rec("y", Int, Primop (Plus, [Var "x"; Var "x"]))),
   (* let y = 2 in y + 1 *)
   Rec("y", Int, Primop (Plus, [I 1; I 1 ]))); 

                                  
  (((I 1, "x"), (* [1/x] *)
    (* let y = 2 in y + x *)
    Fn([("y", Int)], Primop (Plus, [Var "y"; Var "x"]))),
   (* let y = 2 in y + 1 *)
   Fn([("y", Int)], Primop (Plus, [Var "y"; I 1 ]))); 
  
  (((I 1, "y"), (* [1/x] *)
    (* let y = 2 in y + x *)
    Fn([("y", Int); ("x", Int)], Primop (Plus, [Var "x"; Var "x"]))),
   (* let y = 2 in y + 1 *)
   Fn([("y", Int); ("x", Int)], Primop (Plus, [Var "x"; Var "x"]))); 

  
]



(* TODO: Implement the missing cases of subst. *)
let rec subst ((e', x) as s) exp =
      (* Applying a list of substitutions to an expression, leftmost first *)
  let subst_list subs exp =
    List.fold_left (fun exp sub -> subst sub exp) exp subs
  in 
  match exp with
  | Var y ->
      if x = y then e'
      else Var y
  | I n -> I n
  | B b -> B b
  | Primop (po, args) -> Primop (po, List.map (subst s) args)
  | If (e, e1, e2) ->
      If (subst s e, subst s e1, subst s e2)
  | Let (y, e1, e2) ->
      let e1' = subst s e1 in
      if y = x then
        Let (y, e1', e2)
      else
        let (y, e2) =
          if List.mem y (free_variables e') then
            rename y e2
          else
            (y, e2)
        in
        Let (y, e1', subst s e2) 
          
  | Rec (y, t, e) ->
      let e'' = subst s e in 
      if y = x then
        Rec (y, t, e'')
      else 
        let (y, e) = 
          if List.mem y (free_variables e') then
            rename y e
          else (y, e)
        in 
        Rec (y, t, subst s e)

  | Fn (xs, e) -> 
      let namesList = List.map (fun ((name, tp) as d) -> name) xs in 
      let typesList = List.map (fun ((name, tp) as d) -> tp) xs in 
      let (renamedList, renamedExp) = rename_all namesList e in 
      let argList = List.combine renamedList typesList in
      let exp2 = subst s renamedExp in 
      Fn (argList, exp2)
                  (* extract names from xs. with the list of names, 
                  rename all the names. form the renamings on e as well (with rename all). 
                  construct a new fn *)

  | Apply (e, es) -> Apply(subst s e, List.map (fun e' -> subst s e') es) 
                       
                       

and rename x e =
  let x' = freshVar x in
  (x', subst (Var x', x) e)

and rename_all names exp =
  List.fold_right
    (fun name (names, exp) ->
       let (name', exp') = rename name exp in
       (name' :: names, exp'))
    names
    ([], exp)


```