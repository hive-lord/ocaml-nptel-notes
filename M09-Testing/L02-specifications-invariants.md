# Specifications and invariants

# Specifications and invariants

## Specifications and invariants
None of this requires a new library. Everything is comments and ordinary OCaml. But it's the load-bearing layer under everything else: next lecture derives test cases from specifications, and PBT lectures turn specification clauses into executable properties.

## A Contract Between Two Parties
In OCaml, specifications conventionally live in documentation comments `(** ... *)`, attached to declarations in a module signature. The client reads the signature; the signature carries the contract.

## The Returns Clause
First sentence of a good function spec says what the result *is*, not how it's computed.
OCaml convention: write the function applied to named arguments, in square brackets, then state the result:
```ocaml
(** [pad n s] pads string [s] on the left with spaces to width [n].
    If [String.length s >= n], returns [s] unchanged. *)
val pad : int -> string -> string
```

## The Requires Clause (Preconditions)
What must be true of inputs for the function to behave as specified?
```ocaml
(** [div x y] returns [x / y].
    Requires: [y <> 0].
    Raises: [Division_by_zero] if [y = 0]. *)
val div : int -> int -> int
```

## The Raises Clause, or Change the Type
Three options for partial functions:
1. Document in `Raises`
2. Return `option` (caller handles `None`)
3. **Change the type** so partiality shows in the signature (most OCaml-flavoured)

## Abstraction Function and Representation Invariant
For data structures with hidden representation:
- **Abstraction function**: maps concrete representation → abstract value
- **Representation invariant (rep invariant)**: predicate that must hold of concrete representation

```ocaml
type rational = { num : int; den : int }
(** AF: { num = n; den = d } represents the rational number [n/d] *)
(** RI: [d > 0] and [gcd n d = 1] *)

let rep_ok r =
  if r.den <= 0 || gcd r.num r.den <> 1
  then raise (Failure "rep invariant violated")
  else r
```

## Activity
An interval type stores a record `{ lo : int; hi : int }` with representation invariant `lo <= hi`. Write `rep_ok : interval -> interval` that returns its argument when the invariant holds and raises `Failure` otherwise.

**Concepts**: specification, precondition, postcondition, raises clause, examples clause, abstraction function, representation invariant, rep_ok
**Keywords**: OCaml, specification, contract, precondition, postcondition, requires, returns, raises, abstraction function, representation invariant, rep_ok, data abstraction
**Next**: M09 · L03 · Designing test cases: black-box and glass-box