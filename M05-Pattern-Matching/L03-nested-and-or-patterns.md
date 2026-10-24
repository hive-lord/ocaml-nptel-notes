# Nested patterns and or-patterns

# Nested Patterns and Or-patterns

## Nested Patterns
A pattern can contain *other patterns* inside it:
```ocaml
Some (x, _)  (* tuple pattern inside constructor pattern *)
(0, _) :: _  (* tuple inside cons inside cons-context *)
```

## The Diagonal Idiom: Match on a Pair
Instead of nested matches:
```ocaml
let combine a b =
  match a, b with
  | None, None -> "both empty"
  | Some _, None -> "first only"
  | None, Some _ -> "second only"
  | Some _, Some _ -> "both present"
```
- `match a, b with` = sugar for `match (a, b) with`
- Four clauses cover 2×2 grid in one match
- Exhaustiveness checked on cross-product in one pass

## Or-patterns
```ocaml
let is_east_or_west = function
  | East | West -> true
  | _ -> false
```
`East | West` matches `East` or `West`. Compiler sees all constructors covered.

## Or-patterns Nested Inside Other Patterns
```ocaml
let is_non_positive = function
  | 0 | (-1) | (-2) | (-3) -> true  (* illustration; use guard for real check *)
  | _ -> false
```

## Or-patterns with `as`
```ocaml
let f = function
  | (x, _) as p -> p  (* binds the whole matched value *)
  | _ -> (0, 0)
```
Main use: with or-patterns to bind whichever alternative matched.

**Concepts**: nested patterns, or-patterns, tuple patterns inside variants, alternation, diagonal idiom
**Keywords**: OCaml, pattern matching, nested patterns, or-patterns, alternation
**Next**: M05 · L04 · Guards: when-clauses on patterns