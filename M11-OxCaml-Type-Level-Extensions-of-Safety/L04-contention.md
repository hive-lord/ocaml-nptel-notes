# Contention: synchronisation at compile time

# Contention: synchronisation at compile time

## Contention: synchronisation at compile time
First half of this module built the resource story: locality (stack allocation without escape), uniqueness (safe `free`), linearity (no double-`close`). Each axis let the compiler refuse a program that mishandles a value's lifetime.

This lecture and the next move to the other half of OxCaml's safety story: making *concurrency* data-race-free at the type level.

Vanilla OCaml 5 gives you domains (real OS-thread parallelism) but no compile-time help with the races those domains can create. Runtime conventions: don't share a `ref` across domains; if you must share, wrap it in `Mutex` or `Atomic`.

OxCaml closes the gap with two new axes that work as a pair:
- **Contention** (this lecture): how a shared value may be *accessed*
- **Portability** (next lecture): whether a value may *cross* to another domain

## A new bug class: cross-domain races
- Memory bugs: M10 closed by GC + types
- Stack escape, unsafe `free`, double `close`: closed by locality, uniqueness, linearity
- Still open: **data races** across domains
- Vanilla OCaml 5 ships domains but no compile-time race check
- This lecture and the next: how OxCaml fixes that, at the type level

## Recall: what a data race is
Two threads race when they make two memory accesses (each a read or a write) such that:
- They target the **same location**
- At least **one is a write**
- **No synchronisation** orders them (no join, no lock, no atomic)

In OCaml a race is *bounded*, not undefined. But bounded is still surprising, and hard to reason about; the goal now is for races not to happen at all.

## Contention Modes
- **`uncontended`** (default): value not shared across domains; normal reads/writes allowed
- **`shared`**: value may be shared; only atomic operations allowed (`Atomic.get`, `Atomic.set`)
- **`contended`**: value is shared and accessed via mutex/synchronisation

## Rule 1 in action
If you remove the `@ contended` annotation, the value defaults to `uncontended` and everything compiles as in regular OCaml. The annotation is opt-in safety.

**Concepts**: contention, uncontended, contended, shared, data race, Atomic, mode crossing
**Keywords**: OCaml, OxCaml, contention, uncontended, contended, shared, Atomic, mode crossing, data race
**Next**: M11 · L05 · Portability: data-race freedom across domains