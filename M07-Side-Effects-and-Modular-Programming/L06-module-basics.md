# Module basics

# Module basics

## Module basics
OCaml's answer to structuring code: the *module system*. A module is a named collection of definitions (values, types, exceptions, sub-modules) you can refer to as a unit.

## This Lecture: Module basics
- Everything so far at top level: one shared global namespace
- Real programs need *structure*: hundreds of definitions, many files, name collisions
- Standard library is a tree of modules: `List`, `String`, `Array`, `Option`, `Map`, ...
- Every `.ml` file is a module

## Module Syntax
```ocaml
module Stack = struct
  type 'a t = 'a list
  let empty = []
  let push x s = x :: s
  let pop = function
    | [] -> None
    | x :: xs -> Some (x, xs)
end
```

## Modules Contain Types Too
```ocaml
module Color = struct
  type t = R | G | B
  let to_string = function
    | R -> "red" | G -> "green" | B -> "blue"
end
```
Convention: name the principal type `t` → `Color.t`, `Set.t`, `Map.t`.

## Modules Are Not First-Class
- Cannot pass module as argument to regular function
- Cannot store module in a list
- Cannot return one from an `if`
- OCaml has *two* type systems: value-level and module-level

## Opening Modules
```ocaml
open List
let _ = map (fun x -> x + 1) [1; 2; 3]
```
Or local open: `List.(map (fun x -> x + 1) [1; 2; 3])`

## Activity
Define a module `Stack` maintaining an integer stack with `push : int -> unit`, `pop : unit -> int option`, `peek : unit -> int option`. Use a `ref` inside the module.

**Concepts**: modules, struct, namespacing, opening, naming hygiene
**Keywords**: OCaml, module, struct, open, namespace
**Next**: M07 · L07 · Module signatures