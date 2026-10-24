# Model-based testing of stateful data structures

# Model-based testing of stateful data structures

## Model-based testing of stateful data structures
Testing stateful code by comparing against a *reference implementation*.

## A Worked Example: a two-list queue
### Under test: the two-list queue
```ocaml
type queue = {
  mutable front : int list;  (* next to leave, oldest first *)
  mutable back : int list;   (* arrivals, newest first *)
}
let create () = { front = []; back = [] }
let enqueue q x = q.back <- x :: q.back
let dequeue q =
  match q.front with
  | x :: rest -> q.front <- rest; Some x
  | [] ->
      match List.rev q.back with
      | [] -> None
      | x :: rest ->
          q.front <- rest;
          q.back <- [];
          Some x
let size q = List.length q.front + List.length q.back
```
Twenty lines of mutable state with one delicate moment: the refill.

### The Reference Implementation (model)
```ocaml
type model = int list ref  (* head of list = front of queue *)
let m_create () : model = ref []
let m_enqueue q x = q := !q @ [x]
let m_dequeue q =
  match !q with
  | [] -> None
  | x :: rest -> q := rest; Some x
let m_size q = List.length !q
```

## Step 1: Commands
```ocaml
type command =
  | Enqueue of int
  | Dequeue
  | Size
```

## Step 2: Generating commands
```ocaml
let command_gen : command QCheck.Gen.t =
  let open QCheck.Gen in
  oneof [
    map (fun x -> Enqueue x) (int_range 0 9);
    return Dequeue;
    return Size;
  ]
```

## Step 3: Shrinking commands
```ocaml
let command_shrink (c : command) : command QCheck.Iter.t =
  match c with
  | Enqueue x ->
      QCheck.Iter.map (fun x' -> Enqueue x') (QCheck.Shrink.int x)
  | Dequeue | Size -> QCheck.Iter.empty
```

## Step 4: Wrap into an `arbitrary`
```ocaml
let command_list_gen : command list QCheck.arbitrary =
  QCheck.make
    ~print:(fun cs -> "[" ^ String.concat "; " (List.map command_to_string cs) ^ "]")
    ~shrink:(QCheck.Shrink.list ~shrink:command_shrink)
    (QCheck.Gen.list_size (QCheck.Gen.int_range 0 30) command_gen)
```

## The Full Test
Execute commands on both queue and model, compare observable results after each step.

## When Does This Scale?
- Limits: concurrent code (test linearisability with `multicoretests`)
- Industrial cases: Quviq + Volvo + Ericsson; Jane Street (`ppx_quickcheck`); OCaml 5's multicoretests

**Concepts**: model-based testing, stateful testing, reference implementation, command sequences, observable equivalence, queue, PBT for state
**Keywords**: OCaml, QCheck, model-based testing, reference implementation, command, stateful, queue, hash table, association list, observable equivalence
**Next**: M09 · L07 · Tutorial: testing the expr evaluator with OUnit2 and QCheck