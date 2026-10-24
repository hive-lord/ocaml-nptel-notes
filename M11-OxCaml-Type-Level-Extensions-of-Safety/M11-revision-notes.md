# Module 11 Revision Notes: OxCaml — Type-Level Extensions of Safety

## Key Concepts

### OxCaml
- Performance-oriented **superset** of OCaml (Jane Street, open source)
- Every OCaml program is valid OxCaml
- Two extensions: **Control** (allocations/layout) + **Safety** (data-race freedom, memory safety)

### Five Safety Axes (Modes)

| Axis | Default | Other Mode(s) | Tracks | Replaces |
|------|---------|---------------|--------|----------|
| **Locality** | `global` | `local` | Escape (scope) | `return &x` |
| **Uniqueness** | `aliased` | `unique` | Past aliasing | use-after-free, double-free |
| **Linearity** | `many` | `once` | Future use | use-after-close, double-close |
| **Contention** | `uncontended` | `shared`, `contended` | Cross-domain access | racy reads on shared mutable |
| **Portability** | `nonportable` | `portable` | Cross-domain crossing | shared `ref` racing |

**Zero runtime cost**: what testing sampled and M10 mapped, modes now prove for every run.

---

### Locality (`@ local`)
- Values that **must not escape** their scope
- Enables **stack allocation** (no GC traffic)
- `@ local` annotation on values/closures
- `[@zero_alloc]` proves function never touches heap

### Uniqueness (`@ unique`)
- Compiler proves reference is the **only one** to its target
- Enables safe `free` (no double-free, no use-after-free)
- `Unique_ref` API: `create`, `get`, `free` (consumes the reference)
- **Closure puzzle**: uniqueness alone can't police unique value in closure

### Linearity (`@ once`)
- Tracks **future use**: "use at most once"
- Solves closure puzzle: closure capturing `unique`/`once` becomes `once`
- `Handle` API: `with_handle` brackets resource, callback is `@ once`
- Affine (at most once) — discarding allowed

### Contention
- How a **shared** value may be **accessed** across domains
- `uncontended` (default): normal reads/writes
- `shared`: only atomic ops (`Atomic.get`, `Atomic.set`)
- `contended`: mutex/synchronisation required

### Portability
- Whether a value may **cross** to another domain
- `nonportable` (default): `ref`, mutable records, arrays — cannot cross
- `portable`: immutable data, or properly synchronised mutable data
- `Domain.Safe.spawn` requires `portable` closure

---

### Bracketed Resource Pattern
```ocaml
module Handle : sig
  type t
  val with_handle : string -> (t -> 'a) -> 'a
end = struct
  type t = { fd: Unix.file_descr; mutable closed: bool }
  let with_handle path f =
    let fd = Unix.openfile path [Unix.O_RDWR] 0o644 in
    let h = { fd; closed = false } in
    let result = f h in
    Unix.close fd; result
end
```
- Client **never sees** `open`/`close`
- Guarantees: paired, no escape, no double-close, no use-after-close
- `@ local` on callback prevents handle escape
- `@ once` on callback prevents double-invocation

---

## Module 11 Quiz

### MCQs

**Q1.** What does `@ local` enable?
- A) Heap allocation
- B) Stack allocation (no GC)
- C) Cross-domain sharing
- D) Unique references

**Answer: B** — Stack allocation, compiler proves no escape

**Q2.** What's the difference between uniqueness and linearity?
| | Uniqueness | Linearity |
|---|-----------|-----------|
| Forbids aliasing? | Yes | No |
| Forbids dropping? | No | No (at most once) |
| Tracks | Past | Future |
| Best for | `free` | `close` |

**Q3.** What mode does a closure capturing a `unique` value get?
- A) `unique`
- B) `once`
- C) `many`
- D) `global`

**Answer: B** — Closure becomes `once` (linearity polices future use)

**Q4.** What does `Domain.Safe.spawn` require that vanilla `Domain.spawn` doesn't?
- A) `@ local` on the closure
- B) `@ unique` on the closure
- C) `@ portable` on the closure
- D) `@ once` on the closure

**Answer: C** — Portability constraint prevents gensym race

### MSQs

**Q5.** Which are true about OxCaml's five axes? (Select all that apply)
- [ ] A) Zero runtime cost
- [ ] B) They replace testing entirely
- [ ] C) `local` enables stack allocation
- [ ] D) `portable` allows cross-domain crossing
- [ ] E) `contended` means normal reads/writes allowed

**Answer: A, C, D** — B: they complement testing; E: `contended` requires synchronisation

**Q6.** In the bracketed `Handle` API, why is the callback `@ once`?
- [ ] A) To allow multiple calls
- [ ] B) To prevent double-close / use-after-close
- [ ] C) To enable stack allocation
- [ ] D) To allow cross-domain use

**Answer: B** — Linearity prevents double-invocation of callback that closes handle

### NAT

**Q7.** How many safety axes does OxCaml introduce?
**Answer: 5**

**Q8.** What is the default mode for portability?
**Answer: `nonportable`**

### Code Analysis

**Q9.** Why does this gensym race in vanilla OCaml 5?
```ocaml
let gensym = let c = ref 0 in fun () -> let n = !c in c := n+1; "x"^string_of_int n
Domain.spawn (fun () -> for _=1 to 1000 do ignore (gensym "x") done)
```
**Answer:** Shared `ref` across domains with no synchronisation — data race on `c`

**Q10.** In the `Handle.with_handle` implementation, what prevents the handle from being stored in a global `ref`?
**Answer:** The callback is annotated `@ local` — the compiler proves the handle doesn't escape the callback's scope