# Practice: programming with modes

# Practice: programming with modes

## Problem 10: a `portable` closure that captures a `ref`
(And other stretch problems on OxCaml modes)

## Module 11 summary
| Axis | Default | Other mode(s) | Tracks | Replaces |
|------|---------|---------------|--------|----------|
| Locality | `global` | `local` | Escape | `return &x` |
| Uniqueness | `aliased` | `unique` | Past aliasing | use-after-free, double-free |
| Linearity | `many` | `once` | Future use | use-after-close, double-close |
| Contention | `uncontended` | `shared`, `contended` | Cross-domain access | racy reads on shared mutable state |
| Portability | `nonportable` | `portable` | Cross-domain crossing | shared `ref` racing |

Five axes, zero runtime cost: what testing sampled and M10 mapped, modes now prove, for every run.

**Concepts**: practice problems, OxCaml, modes, locality, uniqueness, linearity, contention, portability
**Keywords**: OCaml, OxCaml, practice, modes, local, exclave, unique, once, contended, portable, stack allocation
**Next**: M12 · L01 · Why do we need an OS? (Unikernels / MirageOS)