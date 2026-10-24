# Tutorial: fold across data structures

# Tutorial: fold across data structures

## Tutorial: fold across data structures
Capstone of Module 6. Toolkit: higher-order functions, `map`, `filter`, `fold_left`/`fold_right`, `|>`, composition.

Thesis: this small toolkit covers much list work without hand-coded recursion, *and* the same pattern lifts to other recursive types.

Seven problems: four rebuild `List` functions on top of `fold`, three lift `fold` to other data structures.

## Problem 1: `sum` and `product`
```ocaml
let sum xs = List.fold_left (+) 0 xs
let product xs = List.fold_left ( * ) 1 xs
```
Identity element for the operator (`0` for `+`, `1` for `*`).

## Problem 2: `concat`
```ocaml
let concat xss = List.fold_left ( @ ) [] xss
```

## Problem 3: `for_all` and `exists`
```ocaml
let for_all p xs = List.fold_left (fun acc x -> acc && p x) true xs
let exists p xs = List.fold_left (fun acc x -> acc || p x) false xs
```

## Problem 4: `count`
```ocaml
let count p xs = List.fold_left (fun acc x -> if p x then acc + 1 else acc) 0 xs
```

## Problem 5: Tree Folds in Three Orderings
```ocaml
type 'a tree = Leaf | Node of 'a tree * 'a * 'a tree

(* Pre-order: root, left, right *)
let fold_pre f acc = function
  | Leaf -> acc
  | Node (l, v, r) -> fold_pre f (fold_pre f (f acc v) l) r

(* In-order: left, root, right *)
let fold_in f acc = function
  | Leaf -> acc
  | Node (l, v, r) -> fold_in f (f (fold_in f acc l) v) r

(* Post-order: left, right, root *)
let fold_post f acc = function
  | Leaf -> acc
  | Node (l, v, r) -> fold_post f (fold_post f (fold_post f acc l) r) v
```

## Problem 6: Level-order with a Queue
Not a fold on the tree's own shape; uses a list-as-queue of pending subtrees (BFS).

## A Wider Example: Word Frequencies
```ocaml
let word_freqs text =
  text
  |> String.lowercase_ascii
  |> String.split_on_char ' '
  |> List.filter (fun w -> w <> "")
  |> List.fold_left (fun acc w ->
       let count = List.assoc_opt w acc in
       (w, Option.value ~default:0 count + 1) :: List.remove_assoc w acc)
      []
```
Pipeline: `map`-like, `filter`, `fold_left`. Accumulator: association list. `O(n²)` - for production use `Map` or `Hashtbl` (Module 7).

**Concepts**: worked higher-order, reimplement standard library, fold-everywhere, composition
**Keywords**: OCaml, List, tutorial, higher-order, rebuild
**Next**: M06 · L07 · Practice: recursion, higher-order functions, and syntax trees