# Practice: mutability, modules, and streams

# Practice: mutability, modules, and streams

## Problem 1: `sum_ref`
Sum a list using a single mutable accumulator and `List.iter`, not recursion and not `List.fold_left`. Practise the ref idiom.

## Problem 4: `gensym`
```ocaml
let gensym = 
  let counter = ref 0 in
  fun () -> 
    let n = !counter in
    counter := n + 1;
    "x" ^ string_of_int n
```
Value, not function-with-arguments. Evaluating allocates counter *once*, returns closure over it. Counter unreachable from outside.

## Problem 6: a growable array
Fixed-size array cannot grow. A *growable* array wraps a backing array plus a length, and *doubles* the backing array when it fills up.

## Problem 9: a doubly-linked-list node functor
```ocaml
module type NODE = sig
  type t
  type content
  val create : content -> t
  val get_content : t -> content
  val get_next : t -> t option
  val get_prev : t -> t option
  val set_next : t -> t option -> unit
  val set_prev : t -> t option -> unit
end

module MakeNode (C : Showable) : NODE with type content = C.t = struct
  type content = C.t
  type t = {
    value : content;
    mutable next : t option;
    mutable prev : t option;
  }
  let create v = { value = v; next = None; prev = None }
  let get_content n = n.value
  let get_next n = n.next
  let get_prev n = n.prev
  let set_next n m = n.next <- m
  let set_prev n m = n.prev <- m
end
```

## Problem 10: a doubly-linked-list functor
Build the list on top of the node from Problem 9.

**Concepts**: practice problems, references, mutable records, arrays, modules, functors, streams
**Keywords**: OCaml, practice, assignment, ref, mutable, array, module, functor, stream
**Next**: M08 · L01 · The option monad and `let*`