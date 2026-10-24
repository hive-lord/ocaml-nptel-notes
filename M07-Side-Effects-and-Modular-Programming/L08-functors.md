# Functors

# Functors

## Functors
Functions take values. Modules collect values. Natural next step: *functions from modules to modules*. In OCaml these are called **functors**.

## This Lecture: functors
- Input one or more modules, output a new module
- Borrowed from category theory; practical meaning: parameterised modules
- Standard library uses them everywhere: `Map.Make`, `Set.Make`, `Hashtbl.Make`

## The Classic Example: `Map.Make`
```ocaml
module type OrderedType = sig
  type t
  val compare : t -> t -> int
end

module Map = struct
  module Make (Ord : OrderedType) = struct
    type key = Ord.t
    type 'a t = ...
    ...
  end
end
```
User supplies ordered type (type + compare); functor returns full map specialised to that type.

## Functors vs Java Generics
Java `TreeSet<E>` requires `E` implements `Comparable<E>`. OCaml carries same info in a *module* (type `t` + `compare` function).

## Including a Functor's Output
```ocaml
module IntMap = Map.Make (struct
  type t = int
  let compare = Stdlib.compare
end)

module ExtendedIntMap = struct
  include IntMap
  let find_or_default k d m = ...
end
```
Pattern: extend standard library data structures with project-specific helpers.

## Activity
Why is `Set` a functor (`Set.Make`) rather than a plain `Set` type parameterized by `'a`?
Answer: A set needs an ordering for its elements.

**Concepts**: functors, parameterized modules, Map.Make, generic data structures
**Keywords**: OCaml, functor, Map.Make, Set.Make, parameterized modules
**Next**: M07 · L09 · Tutorial: a queue functor