# GADTs: hlists and witnesses

# GADTs: hlists and witnesses

## GADTs: hlists and witnesses
Start from last lecture's typed pretty-printer `show`, lifted to a *list*.

## Why We Need Hlists
Ordinary `'a list` requires every element to share `'a`. A tuple like `(int, string, bool)` works for fixed shape but does not let you recurse over its elements. When you need a *fixed-shape but mixed-typed* collection that you can also recurse over, the OCaml answer is an **hlist**.

## Heterogeneous Lists (hlists)
```ocaml
type _ hlist =
  | HNil : unit hlist
  | HCons : 'a * 'b hlist -> ('a * 'b) hlist
```
Actually uses the witness pattern from `show`:
```ocaml
type _ ty = ...  (* witnesses *)

type _ hlist =
  | HNil : unit hlist
  | HCons : 'a ty * 'b hlist -> ('a * 'b) hlist
```

## Witness List
Reuse last lecture's `ty` witness, one per element, indexed to match the hlist.

## `pp_hlist`: Fold Across the Whole List
```ocaml
let rec pp_hlist : type a. a ty -> a hlist -> string = ...
```

## Aside: `Printf.printf` Does the Same Thing
OCaml's `Printf.printf` works by exactly this mechanism, but the witness list is encoded inside the format string. When you write `Printf.printf "%d %s %b
"`, the compiler parses the format string into a witness list and uses it to insist on `int -> string -> bool -> unit` as the type of the remaining call.

This is what we mean when we say GADTs are everywhere in OCaml, quietly.

**Concepts**: heterogeneous list, hlist, type witness, generic programming, fold over witnesses
**Keywords**: OCaml, GADT, hlist, witness, Format, printf
**Next**: M08 · L07 · Tutorial: a tiny well-typed evaluator