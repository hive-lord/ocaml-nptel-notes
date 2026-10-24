# Tutorial: a resource-management API

# Tutorial: a resource-management API

## Today's plan
1. The design problem: a file-handle protocol
2. The trick: do not expose `open` and `close`
3. The bracketed signature
4. The implementation: a sealed module
5. Attack the API three ways
6. Why the callback is `@ once`
7. Compare to C's `FILE *`

## The design problem
1. **Open and close are always paired**. Every successful open is followed by exactly one close, on every path, including paths where the client's code raises an exception.
2. **No handle escape**. The handle cannot outlive its file: no stashing it in a global `ref`, no returning it, no capturing it in a closure that runs later.
3. **No double close**.
4. **No use after close**.

## The trick: do not expose `open` and `close`
```ocaml
module type HANDLE = sig
  type t
  val with_handle : string -> (t -> 'a) -> 'a
end
```
Client never sees `open` or `close`. The library brackets the resource; the client writes the middle.

## The bracketed signature
```ocaml
module Handle : sig
  type t
  val with_handle : string -> (t -> 'a) -> 'a
end = struct
  type t = { fd : Unix.file_descr; mutable closed : bool }
  
  let with_handle path f =
    let fd = Unix.openfile path [Unix.O_RDWR] 0o644 in
    let h = { fd; closed = false } in
    let result = f h in
    Unix.close fd;
    result
end
```

## Attack the API three ways
1. **Try to stash handle in global ref**: blocked by `@ local` on callback
2. **Try to close twice**: blocked - `close` not exposed
3. **Try to use after close**: blocked - `close` not exposed

| Guarantee | Delivered by |
|-----------|--------------|
| Open and close always paired | the bracket |
| No handle escape | locality (`@ local`) |
| No double close | unwritable: `close` not exposed |
| No use after close | unwritable, for same reason |

## Why the callback is `@ once`
The callback is `@ once` because it captures the handle (which is `unique`/`once`). A closure capturing a `once` value is itself `once`. This prevents the callback from being called twice (which would double-close or use-after-close).

## Module 11 summary
| Axis | Default | Other mode(s) | Tracks | Replaces |
|------|---------|---------------|--------|----------|
| Locality | `global` | `local` | Escape | `return &x` |
| Uniqueness | `aliased` | `unique` | Past aliasing | use-after-free, double-free |
| Linearity | `many` | `once` | Future use | use-after-close, double-close |
| Contention | `uncontended` | `shared`, `contended` | Cross-domain access | racy reads on shared mutable state |
| Portability | `nonportable` | `portable` | Cross-domain crossing | shared `ref` racing |

Five axes, zero runtime cost: what testing sampled and M10 mapped, modes now prove, for every run.

## What's next
Module 11 closes the type-level chapter of the safety story. The next module (M12) zooms out: if the language is this safe, what falls out if you write the *operating system* itself in OCaml? The answer is **unikernels**, and that is where we go next.

**Concepts**: tutorial, file handle, bracket, with_handle, locality, linearity, uniqueness, resource management, API design
**Keywords**: OCaml, OxCaml, tutorial, file handle, with_handle, bracket, local, once, unique
**Next**: M11 · L07 · Practice: programming with modes