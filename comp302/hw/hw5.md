### q1

```ocaml

let tree_depth_cps t k = match t with
  | Empty -> k 0
  | Tree(l, _, r) -> tree_depth_cps l (fun m -> let m = m + 1 in tree_depth_cps r (fun n -> let n = n+1 in k (max m n)))


```

### q2

```ocaml

let rec get_parent l = match l with
  | [x] -> x
  | h::t -> get_parent t
              
let rec subtree_helper parent tree = 
  match tree with
  | Empty -> None
  | Tree(l, e, r) -> 
      if parent = e then Some(l) else  (* check current tree against parent *)
        let fl = subtree_helper parent l in  (* search left subtree *)
        if fl <> None then fl else
          let fr = subtree_helper parent r in (* search right subtree *)
          if fr <> None then fr else None

let rec find_subtree prefix tree : 'a tree option = 
  if prefix = [] then Some(tree) else (* return current tree *)
    let parent = get_parent prefix in (* get the key of the tree's parent node *)
    subtree_helper parent tree 




(* TODO: Implement a CPS style find_subtree_cont function.*)

let rec subtree_helper_cps parent tree (k: 'a tree option -> 'a tree option) =
  match tree with
  | Empty -> k None
  | Tree(l, e, r) ->
      if parent = e then k (Some(l)) else  (* check current tree against parent *)
        subtree_helper_cps parent l (fun fl -> if fl <> None then k fl else
          subtree_helper_cps parent r (fun fr -> if fr <> None then k fr else k None))

  

let rec find_subtree_cps prefix tree =
  let k = (fun x -> x) in
  if prefix = [] then k (Some(tree)) else
    let parent = get_parent prefix in
    subtree_helper_cps parent tree (fun q -> k q)
```
