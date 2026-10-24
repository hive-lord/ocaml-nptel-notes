# Monad laws, the list monad, and the result monad

# Monad laws, the list monad, and the result monad

## Monad laws, the list monad, and the result monad
Previous lecture built the option monad and `let*`. A monad is "a type plus `return` and `bind`". But a *lawful* monad's `return` and `bind` also satisfy three equations, the **monad laws**.

## The Three Monad Laws
```ocaml
(* Left identity *)
bind (return x) f  ===  f x

(* Right identity *)
bind m return      ===  m

(* Associativity *)
bind (bind m f) g  ===  bind m (fun x -> bind (f x) g)
```

In `let*` form:
```ocaml
let* x = return v in f x  ===  f v
let* x = m in return x    ===  m
let* y = (let* x = m in f x) in g y  ===  let* x = m in let* y = f x in g y
```

- Left/right identity fence `return` in: it can only wrap, do nothing else
- Associativity: nested `bind`s regroup freely → `let*` chain reads as flat sequence
- They hold for every monad (option, list, result, state)
- OCaml's type system cannot enforce equalities; check on paper once per monad

## The List Monad: Definition
- Models a computation with *many* results at once (non-determinism)
- `bind xs f` = map `f` across `xs` and concatenate = `List.concat_map`
- `return x = [x]`: one value lifted to one-element list

```ocaml
module ListMonad = struct
  type 'a t = 'a list
  let return x = [x]
  let bind xs f = List.concat_map f xs
  let ( let* ) = bind
end
```

### Non-determinism with `let*`
```ocaml
let* x = [1; 2; 3] in
let* y = [10; 20] in
Some (x + y)
(* All combinations: [11; 21; 12; 22; 13; 23] *)
```

## The Result Monad: Failure with Information
- `option`: "maybe a value"
- `result`: "either a value, or an error with a payload"
- Both carry value on success; only `result` carries info on failure

```ocaml
type ('a, 'e) result = Ok of 'a | Error of 'e
```

### Result.bind
```ocaml
module ResultMonad = struct
  type 'a t = ('a, string) result
  let return x = Ok x
  let bind r f = match r with Ok x -> f x | Error e -> Error e
  let ( let* ) = bind
end
```
Short-circuits on first `Error`; subsequent steps don't run.

## One Interface for Every Monad
```ocaml
module type MONAD = sig
  type 'a t
  val return : 'a -> 'a t
  val bind : 'a t -> ('a -> 'b t) -> 'b t
end

module OptionMonad : MONAD with type 'a t = 'a option = ...
module ListMonad : MONAD with type 'a t = 'a list = ...
module ResultMonad : MONAD with type 'a t = ('a, string) result = ...
```
Three monads, one `let*`. That's why a single `let*` serves all of them.

## Activity
Use the list monad's non-determinism to find every Pythagorean triple `a*a + b*b = c*c` with `a < b` and `a, b, c` each in `1..30`.

**Concepts**: monad laws, list monad, non-determinism, result monad, Result.bind, error propagation
**Keywords**: OCaml, monad laws, list monad, concat_map, result, Result.bind, let*
**Next**: M08 · L03 · The state monad and parameterised state