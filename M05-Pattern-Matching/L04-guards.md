# Guards: when-clauses on patterns

# Guards: when-clauses on patterns

## Guards
Patterns match on *shape* (this constructor, that literal). Guards match on a *computation*.

```ocaml
let classify n =
  match n with
  | n when n > 0 -> "positive"
  | n when n < 0 -> "negative"
  | _ -> "zero"
```
Syntax: `| pat when <bool-expr> -> rhs`. Clause fires only if pattern matches *and* guard is true.

## Exhaustiveness with Guards Is Conservative
```ocaml
let classify n =
  match n with
  | n when n > 0 -> "positive"
  | n when n <= 0 -> "non-positive"
(* Warning 8: all clauses are guarded! *)
```
Even though guards logically cover every integer, the compiler cannot see that. It treats every guard as "might fail at runtime."

## The Fix: Always Add an Unguarded Clause
```ocaml
let classify n =
  match n with
  | n when n > 0 -> "positive"
  | n when n < 0 -> "negative"
  | _ -> "zero"  (* unguarded catch-all *)
```

## Prefer Patterns When They Suffice
```ocaml
(* Pure pattern version - keeps exhaustiveness checking *)
let classify = function
  | 0 -> "zero"
  | n when n > 0 -> "positive"
  | _ -> "negative"
```
Reach for `when` only when the pattern language is not enough.

**Concepts**: when-guards, conditional pattern matching, exhaustiveness with guards
**Keywords**: OCaml, pattern matching, when, guard, conditional pattern
**Next**: M05 · L05 · Exhaustiveness checking