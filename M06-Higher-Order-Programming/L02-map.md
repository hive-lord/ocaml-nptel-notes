# `map`: transform every element

# `map`: transform every element

## `map`: transform every element
Most-used higher-order function in everyday OCaml. Any time you have a list and want a new list of "the same things, but transformed somehow," `map` is the right tool.

## This Lecture: `map`
- `map f xs`: a new list, each element transformed by `f`
- Type: `('a -> 'b) -> 'a list -> 'b list`
- Same length, same order
- Pattern: take a list, get a list of "the same things, transformed"

## Definition
```ocaml
let rec map f = function
  | [] -> []
  | h :: t -> f h :: map f t
```
Nine lines, including the example. Original functions collapse into one-liners:
```ocaml
let double_each = map (fun x -> x * 2)
let string_lengths = map String.length
```

## `map` in the Standard Library
Use `List.map` rather than redefining it.

## Partial Application + `map`
```ocaml
let _ = List.map ((+) 10) [1; 2; 3]  (* = [11; 12; 13] *)
let _ = List.map (( * ) 2) [1; 2; 3]  (* = [2; 4; 6] *)
```

## Activity
Write `zip_with : ('a -> 'b -> 'c) -> 'a list -> 'b list -> 'c list` that pairs up two lists element-wise using the given combining function. What happens for lists of different lengths?

**Think about this**: The natural definition of `map` conses onto a recursive call, so it is not tail-recursive. What goes wrong if you fix it by appending to an accumulator with `@`?

**Concepts**: map, transformation, list traversal, polymorphism, function arguments
**Keywords**: OCaml, map, list, higher-order, transformation
**Next**: M06 · L03 · `filter`: keep what passes the predicate