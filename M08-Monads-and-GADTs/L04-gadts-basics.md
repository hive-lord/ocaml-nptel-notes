# GADTs: variants with type-level information

# GADTs: variants with type-level information

## GADTs: variants with type-level information
Module 8 shifts gears: from sequencing (monads) to a more advanced type-system feature called *generalized algebraic data types* (GADTs).

- Ordinary variants: "value is one of a finite set of cases"
- GADTs add: "and each case can have a *different* type index"
- Practical consequence: wrong combinations become type errors, not runtime crashes
- Idea is simple; notation is unusual; type theory is involved

## Why the Naive Version Fails
```ocaml
type 'a expr =
  | Int_lit : int -> int expr
  | Bool_lit : bool -> bool expr
  | Add : int expr * int expr -> int expr
  | If : bool expr * 'a expr * 'a expr -> 'a expr
```

Without help, OCaml fixes a *single* `'a` across the whole match.
`Int_lit` forces `'a = int`; `Bool_lit` forces `'a = bool`: they conflict → type error.

## The Fix: `type a.` Annotation
```ocaml
let rec eval : type a. a expr -> a = function
  | Int_lit n -> n
  | Bool_lit b -> b
  | Add (e1, e2) -> eval e1 + eval e2
  | If (c, t, f) -> if eval c then eval t else eval f
```
- `type a.` = *locally abstract type* annotation: "for any `a`, this function takes an `a expr` and returns an `a`"
- Each case *refines* `a`: `Int_lit n -> n` forces `a = int`, so `n : int` matches return type
- *Required*, not optional: `eval` recurses at a different type than its own (polymorphic recursion), which OCaml cannot infer

## Pattern Matching: Type Refinement
The `type a. ...` syntax says "this function is polymorphic in `a`, and inside the body `a` is an abstract type." What lets each branch refine `a` independently.

## When to Reach for GADTs
- Typed embedded languages (ASTs that can't represent ill-typed programs)
- Type witnesses (values whose type carries information)
- Length-indexed vectors, heterogeneous lists

**Concepts**: GADT, generalized algebraic data types, type-level information, pattern matching on GADTs, polymorphic recursion, units of measure, phantom types
**Keywords**: OCaml, GADT, type refinement, type-safe AST, polymorphic recursion, units of measure, phantom type
**Next**: M08 · L05 · GADTs: use cases beyond toy interpreters