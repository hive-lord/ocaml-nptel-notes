# Tuples

# Tuples

## So Far: One Value at a Time
This lecture covers the simplest aggregate: the *tuple*. A tuple is a fixed-size bundle of values, possibly of different types, identified by *position*. The number of components is called its *arity*.

## The Plan for Module 4
Three building blocks for *every* data type in OCaml:
- **Tuples** (this lecture): bundle by position, e.g. `(3, "hi")`
- **Records** (L2): bundle by name, e.g. `{x = 3; y = 4}`
- **Variants** (L3): "one of several shapes", e.g. `Some 3` vs `None`
Then recursive types and tutorials put them to work.

## A Tuple Is Several Values Bundled
```ocaml
let pair = (3, true)
let triple = (1, "two", 3.0)
let nested = ((1, 2), (3, 4))
```
Types: `int * bool`, `int * string * float`, `(int * int) * (int * int)`
The `*` in a type position = "and" (Cartesian product).

## Tuples Have a Fixed Size as Part of Their Type
```ocaml
let _ : int * int = (1, 2)
let _ : int * int * int = (1, 2, 3)
```
These are *different types* - arity 2 vs arity 3.
Contrast Python: 2-tuple and 3-tuple are both `tuple`.
OCaml: static checking, fresh type for every shape.

## When to Use a Tuple
- You want access by *name* → use a record
- Ten components → use a record
- Number of values varies → use a list
**Rule of thumb**: two or three things, positions speak for themselves.

## Returning Multiple Values
OCaml functions always return *one* value. But that value can be a tuple.
```ocaml
let divmod a b = (a / b, a mod b)
let _ = divmod 17 5  (* = (3, 2) *)

(* Caller destructures: *)
let (q, r) = divmod 17 5
```
The OCaml idiom for multiple return values.

## Tuples in Collections
```ocaml
let pairs = [(1, "one"); (2, "two")]  (* type: (int * string) list *)
```
Searching by key needs pattern matching (Module 5).

**Concepts**: tuples, product types, pair, fst, snd, destructuring, tuple patterns
**Keywords**: OCaml, tuple, pair, product type, destructuring
**Next**: M04 · L02 · Records