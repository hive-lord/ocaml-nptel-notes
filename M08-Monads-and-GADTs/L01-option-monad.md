# The option monad and `let*`

# The option monad and `let*`

## The option monad and `let*`
Module 8 is about two ideas that recur all over real OCaml code:
1. **Monads**: a pattern for *sequencing computations of a particular shape* without writing the plumbing by hand
2. **GADTs**: a type-system feature for giving variants more precise types

## Module 8 Roadmap
- **Monads** (L1-L3):
  - L1: the **option monad** and `let*` (you are here)
  - L2: the monad **laws**, the **list** monad, the **result** monad
  - L3: the **state** monad and parameterised state
- **GADTs** (L4-L6)
- **L7 tutorial**: a tiny well-typed evaluator combining both
- **L8 practice**: stretch problems on monads and GADTs

## A Realistic Example: Reaching into Optional Data
A `user` may have no `address`, and an `address` may have no `zip`. To get a user's zip we must descend through both options.

### Without `let*` (Pyramid of Doom)
```ocaml
let get_zip user =
  match user.address with
  | None -> None
  | Some addr ->
      match addr.zip with
      | None -> None
      | Some zip -> Some zip
```

### With `let*` over `option`
```ocaml
let get_zip user =
  let* addr = user.address in
  let* zip = addr.zip in
  Some zip
```
Clean, linear, reads top-to-bottom!

## The Option Monad
```ocaml
module Option = struct
  type 'a t = 'a option
  let return x = Some x
  let bind o f = match o with None -> None | Some x -> f x
  let ( let* ) = bind
end
```
- `return` = `Some`
- `bind` = pattern match on `None`/`Some`, short-circuits on `None`
- `let*` = syntax sugar for `bind`

## Activity
What is the type of `fun x -> x +. 1.0`? What does `let* x = Some 5 in Some (x + 1)` evaluate to?

**Concepts**: pyramid of doom, bind, return, option monad, let-operators, Option.bind, Option.map
**Keywords**: OCaml, monad, sequencing, bind, option, let*
**Next**: M08 · L02 · Monad laws, the list monad, and the result monad