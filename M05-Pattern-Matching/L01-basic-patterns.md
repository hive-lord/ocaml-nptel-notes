# Basic patterns: literals, variables, wildcards

# Basic Patterns

## Pattern Matching Moves to the Centre
Pattern matching is the primary way to deconstruct data in OCaml.

## The Plan for Module 5
- **L1** (this lecture): three simplest patterns: literal, variable, wildcard
- **L2**: pattern matching on recursive structures (lists, trees)
- **L3**: patterns inside patterns; records, inline records, diagonal idiom; or-patterns
- **L4**: `when`-guards
- **L5**: exhaustiveness (most load-bearing static check)
- **L6**: tutorial: an interpreter for the OCaml AST

## Literal Patterns
Match exact values:
```ocaml
let is_zero = function
  | 0 -> true
  | _ -> false
```

## Variable Patterns
Match anything and bind it:
```ocaml
let add_one x = x + 1
(* equivalent to *)
let add_one = function
  | x -> x + 1
```

## Wildcard Pattern
`_` matches anything but binds nothing:
```ocaml
let is_zero = function
  | 0 -> true
  | _ -> false
```

## Order Matters!
```ocaml
let f = function
  | 0 -> "zero"
  | n -> "non-zero"
(* Correct: specific first *)

let f = function
  | n -> "anything"   (* matches everything! *)
  | 0 -> "zero"       (* UNREACHABLE - Warning 11 *)
```
**Discipline**: specific patterns first, general patterns last.

## Catch-all Wildcard Caution
On variant types, wildcards suppress exhaustiveness checking. If you add a constructor later, no warning!

## Patterns Are Everywhere
- Function parameters: `let f (x, y) = ...`
- `let` bindings: `let (a, b) = ...`
- Function parameter patterns accept *one* pattern - variants with >1 constructor are non-exhaustive

## Exhaustiveness, Lightly
```ocaml
let label = function
  | 0 -> "zero"
  | 1 -> "one"
(* Warning 8: not exhaustive. Missing case: 2 *)
```
Fix: add more clauses, or add wildcard `_`.

## Two Checks
1. **Warning 8** (partial-match): missing cases
2. **Warning 11** (unused match case): unreachable clause

**Concepts**: pattern matching, literal patterns, variable patterns, wildcards, exhaustiveness, order
**Keywords**: OCaml, pattern matching, literal, variable, wildcard, exhaustiveness
**Next**: M05 · L02 · Pattern matching on lists and trees