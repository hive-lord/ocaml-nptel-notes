# Linearity: use at most once

# Linearity: use at most once

## Linearity: use at most once
What the language needs to enforce is step 4 (use no further), not "the handle has no other references" (which it may, depending on the program). Linearity is the axis that says "after this use, there are no more." That is exactly the file-handle discipline.

## Resolving the Closure Puzzle
This is also why the two axes ship together: uniqueness alone cannot police a unique value that hides inside a closure, and linearity is exactly the axis that can.

- Rule: a closure capturing a `unique` or `once` value is itself `once` (inferred from the captures, no annotation needed)
- Uniqueness alone cannot police a capture; linearity is the axis that can

## A file-handle module
```ocaml
module type HANDLE = sig
  type t
  val open_file : string -> t
  val read : t -> string
  val close : t -> unit
end
```
This is exactly the same shape as the `Unique_ref` example. The shape is generic: any linear/unique resource API ends up looking like an ownership chain. The *meaning* differs (linearity tracks future use, uniqueness tracks past aliasing), but the *programming model* is the same.

## Two bugs caught, and one that is not
### Bug 3: forgetting to close, not caught
Abandonment is not an error; fighting ordinary OCaml control flow would be impractical. So the forgotten `close` stays a silent resource leak, best handled by runtime patterns (`fun_protect`, `with_`-style wrappers) or a finaliser as a backstop. Linearity removes the *double* uses; it does not chase the missing ones.

## Linearity vs uniqueness: why both?
| Uniqueness | Linearity |
|------------|-----------|
| Forbids aliasing? | Yes | No |
| Forbids dropping? | No | No (at most once) |
| Tracks | Past | Future |
| Best for | `free` | `close` |

Pick uniqueness when "no other references" is what matters. Pick linearity when "never used twice" is what matters.

## A footnote: where linearity comes from
Linear logic (Girard, 1987) powered a wave of research into linear type systems in the 1990s. The breakthrough was realising you can have *both* worlds in one language: most code stays unrestricted, only resource-handling code opts into linearity. OxCaml's linearity is this practical packaging of the 1987 idea, with one pragmatic softening: `once` is *at most* once (affine), so discarding a value is allowed.

**Concepts**: linearity, once, many, linear types, closure capture, file handle, resource discipline, linear logic
**Keywords**: OCaml, OxCaml, linearity, once, many, linear types, file handle, Linear Logic, Girard
**Next**: M11 · L04 · Contention: synchronisation at compile time