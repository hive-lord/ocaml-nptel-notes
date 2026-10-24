# Uniqueness: the only reference

# Uniqueness: the only reference

## Uniqueness: the only reference
In M10 we made the case that OCaml's GC takes the use-after-free problem off the table for *memory*. But "resource" is wider than "memory": file descriptors, DB connections, FFI-allocated buffers, mapped pages. The OCaml type system traditionally had nothing to say about these.

OxCaml's **uniqueness** axis is the type-level fix. Like locality, it is a *mode*: it tracks not what a reference is but *how* it may be used, here whether any other reference to it still exists. The compiler proves, for designated references, that they are the *only* reference to their target.

## Where We Are
- M10: GC eliminates use-after-free for **memory**
- But "resource" is wider: file descriptors, DB connections, FFI-allocated buffers, mapped pages
- The OCaml type system traditionally had nothing to say about these
- This lecture: **uniqueness**
  - the compiler proves a reference is the *only* one
  - today's thread: `free` exactly once, checked at compile time

## Unique References
```ocaml
module type Unique_ref = sig
  type 'a t
  val create : 'a -> 'a t
  val get : 'a t -> 'a
  val free : 'a t -> unit
end
```
- `create` returns a unique reference
- `free` consumes it (can't be called twice)
- Compiler enforces uniqueness: no aliasing allowed

## A Puzzle to End On: capturing a unique value in a closure
```ocaml
let f () =
  let r = Unique_ref.create 42 in
  let g () = Unique_ref.free r in
  g ()  (* first call OK *)
  g ()  (* second call? *)
```
Uniqueness alone cannot police a unique value that hides inside a closure. "No other references" stops being the property that protects you, and "how many times can this be called" takes over. That axis is **linearity**, the next lecture.

## What's next
- Lecture 3: **linearity**. Future-use tracking, `once` and `many`. Resolves today's closure puzzle.
- Lecture 4: **contention**. Cross-domain access.
- Lecture 5: **portability**. Cross-domain crossing.
- Lecture 6: tutorial. A resource-management API combining the axes.

**Concepts**: uniqueness, unique, aliased, manual resource management, use-after-free, double-free, in-place update, closure capture
**Keywords**: OCaml, OxCaml, uniqueness, unique mode, aliased, free, Unique_ref, closure capture
**Next**: M11 · L03 · Linearity: use at most once