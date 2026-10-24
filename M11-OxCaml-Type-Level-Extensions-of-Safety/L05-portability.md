# Portability: data-race freedom across domains

# Portability: data-race freedom across domains

## Portability: data-race freedom across domains
- M10 defined the data race and left it a runtime hazard
- M11-L01 to L03: the resource axes. Locality, uniqueness, linearity.
- M11-L04: **contention**. Cross-domain access.
- This lecture (M11-L05): **portability**. Cross-domain crossing.

## The motivating example: a parallel gensym race
```ocaml
let gensym prefix =
  let counter = ref 0 in
  fun () ->
    let n = !counter in
    counter := n + 1;
    prefix ^ string_of_int n

(* In OCaml 5 today, this compiles, and on a multicore machine it races at runtime. *)
let d1 = Domain.spawn (fun () -> for _ = 1 to 1_000 do ignore (gensym "x") done)
let d2 = Domain.spawn (fun () -> for _ = 1 to 1_000 do ignore (gensym "x") done)
```
Runtime behaviour: lost updates, duplicate symbols, occasional crashes. Standard OCaml compiles it and lets you find out the hard way.

## The gensym race
- The ticket-dispenser closure from the imperative half, plus a prefix
- Sequential: fine
- Two domains calling `gensym` in parallel: classic data race on `count`
- All four ingredients present: two domains, shared `ref`, two writers, nothing ordering the accesses
- Vanilla OCaml compiles it. The race fires at runtime.

## `Domain.Safe.spawn` requires `portable`
Vanilla OCaml has `Domain.spawn` without this annotation, which is why the gensym race can compile today. OxCaml's `Domain.Safe.spawn` adds the portability constraint, and the type checker enforces it.

- `portable` values: immutable data, or mutable data properly synchronised
- `nonportable` values (default): `ref`, mutable records, arrays - cannot cross domain boundaries
- To share state safely, use `Atomic` or `Mutex` wrapped in `portable` containers

**Keywords**: OCaml, OxCaml, portability, portable, nonportable, Domain, Atomic, data race, gensym
**Next**: M11 · L06 · Tutorial: a resource-management API