```ocaml
(*--------------------------------------------------------------*)
(* Q1 : String to Characters to String                  *)
(*--------------------------------------------------------------*)

(* 1.1 Turn a string into a list of characters. *)
let string_explode (s : string) : char list = 
  let len = String.length s in 
  let explode_helper x = String.get s x
  in tabulate explode_helper len
  
(* 1.2 Turn a list of characters into a string. *)
let string_implode (l : char list) : string =
  let implode_helper acc cur =
    acc ^ (Char.escaped cur)
  in List.fold_left implode_helper "" l

(* ------------------------------------------------------------------------*)
(* Q2 : Bank Account *)
(* ------------------------------------------------------------------------*)

let open_account (pass: password) : bank_account =
  let current_pass = ref pass in
  let current_amt = ref 0 in
  let check_pass vpass = (!current_pass) = vpass in
  let check_entry_amt amt = amt >= 0 in
  let check_withdraw_amt amt = (!current_amt - amt) >= 0 in 
  {
    
    update_pass = (fun vpass npass -> 
        match check_pass vpass with
        | true -> current_pass := npass
        | false -> raise wrong_pass);
    
    retrieve = (fun vpass amt -> 
        match (check_pass vpass, check_entry_amt amt, check_withdraw_amt amt) with
        | (false, _, _) -> raise wrong_pass
        | (_, false, _) -> raise negative_amount
        | (_, _, false) -> raise not_enough_balance
        | _ -> current_amt := (!current_amt)-amt);
              
    deposit = (fun vpass amt -> 
        match (check_pass vpass, check_entry_amt amt) with
        | (false, _) -> raise wrong_pass
        | (_, false) -> raise negative_amount
        | _ -> current_amt := (!current_amt)+amt);
    
    show_balance = (fun vpass -> 
        match check_pass vpass with
        | false -> raise wrong_pass
        | _ -> !current_amt
      );
  }
```

