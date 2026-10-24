# `filter`: keep what passes the predicate

# `filter`: keep what passes the predicate

## `filter`: keep what passes the predicate
Second of the three canonical higher-order list functions. Where `map` transforms every element and keeps them all, `filter` keeps each element as it was, but drops the ones that fail a test.

## Type Signature
`('a -> bool) -> 'a list -> 'a list`
- Function argument is a *predicate* (returns `bool`)
- Result list has same element type as input
- No transformation; just selection

## Definition
```ocaml
let rec filter p = function
  | [] -> []
  | h :: t -> if p h then h :: filter p t else filter p t
```

## Combining `map` and `filter`
```ocaml
let _ =
  [1; 2; 3; 4; 5]
  |> List.map (fun x -> x * x)
  |> List.filter (fun y -> y > 5)
  (* = [9; 16; 25] *)
```

## A Real-World Filter Pipeline
```ocaml
type book = { title : string; year : int; pages : int }
let library = [
  { title = "OCaml"; year = 2020; pages = 350 };
  { title = "Rust"; year = 2024; pages = 600 };
  { title = "Old"; year = 1985; pages = 200 };
  { title = "Recent"; year = 2023; pages = 50 };
]
let modern_long = List.filter
  (fun b -> b.year >= 2020 && b.pages > 100)
  library
let _ = List.map (fun b -> b.title) modern_long  (* = ["OCaml"; "Rust"] *)
```
Select-where in database lingo, two chained calls in OCaml.

## `filter_map`: One Pass Instead of Two
```ocaml
let _ =
  List.filter_map
    (fun b -> if b.year >= 2020 && b.pages > 100 then Some b.title else None)
    library
(* = ["OCaml"; "Rust"] *)
```

## Tail Recursion and `filter`
Modern OCaml compiles `List.filter` with tail recursion modulo cons, so the natural definition is stack-safe.

**Concepts**: filter, predicate, list filtering, filter_map, partition
**Keywords**: OCaml, filter, predicate, list, higher-order
**Next**: M06 · L04 · `fold`: reduce a list to a single value