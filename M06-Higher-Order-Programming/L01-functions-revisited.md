# Functions as values, revisited

# Functions as Values, Revisited

## From Module 3, Leaned On
- Module 3: functions are **first-class values** (named, passed, returned, stored)
- Module 6: we lean on that idea for the entire module
- A **higher-order function** is one that:
  - takes a function as an argument, *or*
  - returns a function as its result
  - (most interesting ones do both)
- Lets you extract the *pattern* of a computation from the *details*; reuse the same pattern across many concrete computations

## The Plan for Module 6
- **L1** (today): higher-order functions; the anatomy
- **L2** (`map`): apply a function to each element
- **L3** (`filter`): keep elements that satisfy a predicate
- **L4** (`fold`): collapse a collection to a value
- **L5** (pipelines): the `|>` operator
- **L6** (tutorial): rebuild parts of `List`, then fold across binary trees and rose trees

## The Same Shape, Three Times: Factor the Work Out
```ocaml
let rec double_each = function
  | [] -> []
  | h :: t -> (h * 2) :: double_each t

let rec string_lengths = function
  | [] -> []
  | h :: t -> (String.length h) :: string_lengths t
```
Same structure, different work at each step. Factor out the walk!

## Callbacks: The GUI/Event Idiom
```ocaml
type event_handler = unit -> unit
let register_click handler = ...
```
In OCaml, no special machinery needed: a function is a value, just pass it.

## Function Composition
```ocaml
let compose f g = fun x -> f (g x)
let square_then_inc = compose (fun x -> x + 1) (fun x -> x * x)
let _ = square_then_inc 4  (* = 17 *)
```
- Takes two functions, returns a third
- Higher-order on both ends: in *and* out
- Not in stdlib by default; one line to write yourself

## Activity
Given `let apply_twice f x = f (f x)`, what is `apply_twice (fun x -> x + 1) 0`?

**Concepts**: higher-order functions, callbacks, function composition, factoring
**Keywords**: OCaml, higher-order, callback, composition, function value
**Next**: M06 · L02 · `map`: transform every element