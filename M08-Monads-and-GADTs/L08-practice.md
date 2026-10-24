# Practice: monads and GADTs

# Practice: monads and GADTs

Stretch problems, harder than lectures, book-only (no slides).

## Part 1: Monads

### Problem 1: `Ref_monad`
Implement a monad that simulates OCaml-style references holding one fixed value type. State threaded by the monad is a counter paired with a functional heap. A `ref` is just the integer index of its cell.

```ocaml
module type REF_MONAD = sig
  type value
  type ref
  include MONAD
  val mk_ref : value -> ref t
  val ( ! ) : ref -> value t
  val ( := ) : ref -> value -> unit t
  val run_state : 'a t -> 'a
end
```

### Problem 2: `Poly_ref_monad`
Lift the restriction that all cells hold the same type. Heap stores `Univ.t`, each reference carries its own packer.

## Part 2: GADTs

### Problem 3: `cross_v_l`
Pairing a single value with every element of a vector does not change its length.
```ocaml
cross_v_l : 'a -> ('b, n) vec -> ('a * 'b, n) vec
```

## Part 3: the typed interpreter
Return to the GADT-typed AST from the tutorial: an `'a expr` that can only be built well-typed, run by `eval : 'a expr -> 'a`.

**Concepts**: practice problems, state monad, functional references, universal types, length-indexed lists, type-level arithmetic, typed interpreter, type witness, constant folding
**Keywords**: OCaml, practice, assignment, monad, state, ref, GADT, length-indexed, plus, mult, min, witness, simplify
**Next**: M09 · L01 · Why test a type-safe program?