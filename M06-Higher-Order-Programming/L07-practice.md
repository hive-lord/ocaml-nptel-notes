# Practice: recursion, higher-order functions, and syntax trees

# Practice: recursion, higher-order functions, and syntax trees

## Problem Structure
- **Part 1: list recursion and basics** (Problems 1-9): Pattern matching on lists and options, hand-written recursion, tail recursion. Hand-rolling recursion is the point; reach for stdlib only where a problem says so.
- **Part 2: the higher-order toolkit** (Problems 10-15): Same problems, now done with `map`, `filter`, `fold`, `|>`, composition. Follows from pipelines lecture and fold-everywhere tutorial.
- **Part 3: optimising a syntax tree** (Problems 16-17): Recursion over a custom ADT: the arithmetic expression AST from variants tutorial and interpreter tutorial, rewritten to a *simpler equivalent tree* (constant folding and algebraic identities).

## Example Problems

### Problem 2: `n_times`
```ocaml
n_times : ('a -> 'a) * int * 'a -> 'a
```
Applies `f` to `v` exactly `n` times. Tuple argument (not curried).

### Problem 5: `buckets`
Groups elements by predicate using fold and local helper.

### Problem 6: `remove_stutter`
```ocaml
let remove_stutter l =
  let rec aux prev acc = function
    | [] -> List.rev acc
    | x :: xs ->
        (match prev with
         | Some y when y = x -> aux prev acc xs
         | _ -> aux (Some x) (x :: acc) xs)
  in
  aux None [] l
```

### Problem 9: tail-recursive Fibonacci
As a *tail-recursive* function so `fib_tailrec 46 = 1836311903` returns instantly.

### Part 2: Higher-Order Toolkit
Several have a one-line answer once you pick the right combinator and accumulator.

### Problem 13: `run_length_encode`
Implement with a single `List.fold_right`.

### Problem 15: `dot_product`
```ocaml
let dot_product xs ys =
  List.map2 ( * ) xs ys
  |> List.fold_left ( + ) 0
```

**Concepts**: practice problems, list recursion, higher-order, tail recursion
**Keywords**: OCaml, practice, assignment, list, recursion, fold, tail-recursion
**Next**: M07 · L01 · Mutable references