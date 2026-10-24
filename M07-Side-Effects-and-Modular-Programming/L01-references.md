# Mutable references

# Mutable References

## Mutable References
For six modules we wrote OCaml without mutation. Every value was immutable; fresh state = fresh value. This is the *functional* style with real benefits: `f x` produces the same answer regardless of when you run it.

But sometimes mutation is the right tool.

## Six Modules Without Mutation
- M1-M6: every value immutable. Fresh state = fresh value.
- Functional-style payoff:
  - `f x` gives same answer wherever you call it
  - reasoning reduces to substitution
  - compiler can inline and reorder freely
- But sometimes mutation is the right tool

## When Mutation Is the Right Tool
- Web server: request counter across requests
- Memoization table: cached results across calls
- Long-lived event loop: connection pool that grows/shrinks
- Threading state through every call by hand is noisier than needed
- OCaml's stance (unlike Haskell): mutation **available**, not the default
- Simplest opt-in: the **mutable reference cell**, or `ref`

## Why Three Operators?
- **Creation**: `let counter = ref 0` binds a name to a fresh cell containing 0
- **Read**: `!counter` reads from the cell
- **Write**: `counter := 1` writes to the cell
- C makes this same distinction implicitly with pointers

## Mutation Breaks Equational Reasoning
```ocaml
let counter = ref 0
let get () = !counter
let inc () = counter := !counter + 1

get ()  (* = 0 *)
inc ()
get ()  (* = 1  -- same call, different answer! *)
```
This is why OCaml is *functional-first*. Default: pure code. Isolate mutation behind small surface area when needed.

## Activity
Write `make_running_total : unit -> (int -> int)` that returns a closure. Each call adds its argument to a running total and returns the new total. Two calls to `make_running_total ()` must produce two independent accumulators.

**Solution**:
```ocaml
let make_running_total () =
  let total = ref 0 in
  fun x -> total := !total + x; !total
```

**Concepts**: mutation, ref, !, :=, side effects, when to use mutation
**Keywords**: OCaml, ref, mutation, side effects, !, :=
**Next**: M07 · L02 · Mutable records and arrays