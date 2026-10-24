# The state monad and parameterised state

# The state monad and parameterised state

## The state monad and parameterised state
Thread state through a chain without mutation. `return`, `bind`, `get`, `set`, `run`.

Written once as a functor `State(S)` behind the interface `STATE_MONAD`, parameterised by the state type.

Worked example: a `gensym` that issues fresh names.

State monad versus a plain `ref`.

**Parameterised state**: when the state's *type* changes per step. A typed stack machine whose ill-typed programs are compile errors. The bridge to GADTs.

## State Monad Interface
```ocaml
module type STATE_MONAD = sig
  type 'a t
  type state
  val return : 'a -> 'a t
  val bind : 'a t -> ('a -> 'b t) -> 'b t
  val get : state t
  val set : state -> unit t
  val run : 'a t -> state -> 'a * state
end
```

## Gensym Example
```ocaml
module Gensym = State(struct type state = int end)
let fresh_name prefix =
  let* n = Gensym.get in
  let* () = Gensym.set (n + 1) in
  Gensym.return (prefix ^ string_of_int n)
```

## Parameterised State
When the state's *type* changes per step. A typed stack machine where ill-typed programs are compile errors.

**Concepts**: state monad, STATE_MONAD, functor, get, set, gensym, parameterised monad, PSTATE_MONAD, stack machine, WebAssembly, let*
**Keywords**: OCaml, state monad, STATE_MONAD, functor, get, set, gensym, parameterised monad, PSTATE_MONAD, stack machine, WebAssembly, let*
**Next**: M08 · L04 · GADTs: variants with type-level information