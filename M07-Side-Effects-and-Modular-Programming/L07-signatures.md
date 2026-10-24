# Module signatures

# Module signatures

## Module signatures
OCaml's encapsulation story. Same idea as `private`/`public`/`interface`, but at the *module* level. Mechanism is structural, checked at compile time.

## This Lecture: Module signatures
- Last lecture: a `Stack` module's internal `ref` leaked out
- Fix: **module signatures**
- A signature is the type-level description of a module: which names escape, with which types
- Constrain a module by a signature; anything not listed becomes invisible

## Signature Syntax
```ocaml
module type STACK = sig
  type 'a t
  val empty : 'a t
  val push : 'a -> 'a t -> 'a t
  val pop : 'a t -> ('a * 'a t) option
end

module Stack : STACK = struct
  type 'a t = 'a list
  let empty = []
  let push x s = x :: s
  let pop = function
    | [] -> None
    | x :: xs -> Some (x, xs)
end
```

## Abstract Types
```ocaml
module type STACK = sig
  type 'a t        (* abstract - implementation hidden *)
  val empty : 'a t
  val push : 'a -> 'a t -> 'a t
  val pop : 'a t -> ('a * 'a t) option
end
```
If you later switch representation, no external code notices. `Set.t`, `Map.t`, `Hashtbl.t`, `Buffer.t`: all abstract.

## Signatures in `.mli` Files
Convention: implementation in `foo.ml`, interface in `foo.mli`. Compiler enforces match.

**Concepts**: signatures, abstract types, encapsulation, .mli files
**Keywords**: OCaml, signature, abstract type, encapsulation, module
**Next**: M07 · L08 · Functors