# Locality: safe stack allocation

# Locality: safe stack allocation

## Locality: safe stack allocation
This module is about **OxCaml**, an actively-developed, open-source fork of the OCaml compiler (built at Jane Street, available on GitHub). It is a strict superset: every OCaml program is already a valid OxCaml program.

## OxCaml: Control and Safety
- **OxCaml**: a performance-oriented superset of OCaml
  - open source on GitHub
  - used in production at Jane Street
  - every OCaml program is already a valid OxCaml program
- Two kinds of extension:
  - **Control**: allocations and memory layout (performance)
  - **Safety**: data-race freedom and memory safety

## The Plan for the Module
- L1: **locality**. Values that must not escape; stack allocation.
- L2: **uniqueness**. The only reference; safe `free`.
- L3: **linearity**. Use at most once; safe `close`.
- L4: **contention**. How a shared value may be accessed.
- L5: **portability**. What may cross a domain boundary.
- L6: tutorial. One resource API built from the axes.

## Following Up on Module 10: the handle that escaped
Module 10 showed a runtime failure: a file handle escaping its scope. "Runtime scoping cannot prevent this. A stronger type system, one that tracks whether a value is allowed to escape, can reject `escaped := Some h` at compile time. That is the kind of guarantee a later module builds toward." This is that module, and **locality** is that guarantee.

## Locality Also Buys Performance: Stack Allocation
A value the compiler proves cannot escape goes on the stack, where the collector never has to look at it. Same win without leaving OCaml or its GC.

## Building a local list with `exclave_`
In vanilla OCaml, mapping `translate` across an `n`-point list allocates `2n` allocations on the heap. With local lists and `exclave_`, the same shape of code allocates the whole new list in the **caller's** region: no GC traffic.

## Proving Zero Allocation
`[@zero_alloc]` annotation, checked by the native `-O3` compiler: compilation fails unless the function provably never touches the heap.

## Common Pitfalls
"Locality and reference-counting / linear types are the same thing." They are not. Locality is about *scope*: where does this value live? Linearity is about *number of uses*: how many times can this be invoked?

## What's next
- Lecture 2: **uniqueness**. The only reference. Safe `free`.
- Lecture 3: **linearity**. Future-use tracking. Safe `close`.
- Lecture 4: **contention**. Cross-domain access.
- Lecture 5: **portability**. Cross-domain crossing.
- Lecture 6: tutorial. A bracketed resource-management API, designed, implemented, and attacked.

**Concepts**: OxCaml, modes, locality, local mode, stack allocation, exclave, mode crossing, regions
**Keywords**: OCaml, OxCaml, modes, locality, local, stack_, exclave_, regions, mode crossing
**Next**: M11 · L02 · Uniqueness: the only reference