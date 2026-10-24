# Exhaustiveness checking

# Exhaustiveness Checking

## Exhaustiveness Checking
The compiler looks at every `match` and asks: "do these clauses cover every possible value?" If no, it warns and names the missing case.

## How the Check Works
- Takes the type of the matched value
- Computes the set of *value shapes* the type admits
- Subtracts shapes covered by your clauses
- If remainder non-empty: non-exhaustive, reports a witness

## Exhaustiveness on Nested Patterns
```ocaml
let category = function
  | (true, true) -> "both"
  | (true, false) -> "only first"
  | (false, true) -> "only second"
(* Warning: missing (false, false) *)
```
Compiler enumerates all combinations of nested shapes (2×2 = 4 for bool pairs).

## Exhaustiveness and Guards: Reminder
Guards suppress exhaustiveness. Always finish with an unguarded clause:
```ocaml
let classify = function
  | n when n > 0 -> "positive"
  | n when n < 0 -> "negative"
  | _ -> "zero"
```

## Two Checks
1. **Exhaustiveness** (Warning 8): missing cases
2. **Redundant clauses** (Warning 11): unreachable cases

## Takeaways
The division of labour: compiler finds all sites (mechanical); you decide what each should do (interesting). Without exhaustiveness, you'd do both, badly.

**Concepts**: exhaustiveness, partial match, redundant clauses, refactor with the compiler
**Keywords**: OCaml, exhaustiveness, pattern matching, warning 8, warning 11
**Next**: M05 · L06 · Tutorial: an interpreter for the OCaml AST