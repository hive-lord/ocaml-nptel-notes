# Tutorial: a queue functor

# Tutorial: a queue functor

## Tutorial for Module 7
Builds a small *functional queue* (FIFO) using the classic two-stack trick. Packages it as a module with a signature that hides the representation. Then turns it into a functor parameterised by the element type, with a printer attached.

## The Signature
```ocaml
module type QUEUE = sig
  type 'a t
  val empty : 'a t
  val is_empty : 'a t -> bool
  val enqueue : 'a -> 'a t -> 'a t
  val dequeue : 'a t -> ('a * 'a t) option
end
```
Type `'a t` is abstract: outside `Queue`, you cannot see it's a two-list record.

## Two-Stack Queue Implementation
```ocaml
module Queue : QUEUE = struct
  type 'a t = { front : 'a list; back : 'a list }
  let empty = { front = []; back = [] }
  let is_empty q = q.front = [] && q.back = []
  let enqueue x q = { q with back = x :: q.back }
  let rec dequeue q =
    match q.front, q.back with
    | [], [] -> None
    | [], _ ->
        dequeue { front = List.rev q.back; back = [] }
    | x :: front', _ -> Some (x, { q with front = front' })
end
```

## Why Hide the Representation?
1. **Invariants**: Queue assumes `front` is "front in normal order". Hiding enforces it.
2. **Change**: Switch implementation later, no caller breaks.

## Turning It Into a Functor
```ocaml
module type SHOWABLE = sig
  type t
  val string_of_t : t -> string
end

module MakeQueue (E : SHOWABLE) : QUEUE with type 'a t = 'a Queue.t = struct
  type 'a t = { front : 'a list; back : 'a list }
  (* ... same implementation, using E.string_of_t for printing ... *)
end
```

## Applying the Functor
```ocaml
module IntShowable = struct
  type t = int
  let string_of_t = string_of_int
end

module IntQueue = MakeQueue (IntShowable)
```
- **Specialised**: `IntQueue.elt` is `int`, period
- **Generic**: queue logic same regardless of element type
- **Composable**: `StringQueue` is one line
- Same as `Map.Make`, `Set.Make`, `Hashtbl.Make`

**Concepts**: worked module, abstract type, functor, two-stack queue
**Keywords**: OCaml, queue, two-stack, functor, tutorial, module
**Next**: M07 · L10 · Practice: mutability, modules, and streams