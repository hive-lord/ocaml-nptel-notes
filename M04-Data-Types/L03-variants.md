# Variants (sum types)

# Variants (sum types)

## Variants (Sum Types)
Tuples and records express *and*: a point has `x` *and* `y`.
Variants express *or*: a `shape` is a circle *or* a square *or* a rectangle.

## Product Types vs. Sum Types
- **Tuples and records** = **product types** (AND)
  - A `point` has an `x` *and* a `y`
  - A `person` has a `name` *and* an `age` *and* an `email`
- **Variants** = **sum types** (OR)
  - A `shape` is a `Circle` *or* a `Square` *or* a `Rectangle`
  - A parse result is `Ok` *or* `Error`
- Like a C/Java `enum`, but each alternative can carry data
- Most useful data types combine both: variants for the *kinds*, records/tuples for the data inside each kind

## Combining Variants and Records: A Card
```ocaml
type suit = Clubs | Diamonds | Hearts | Spades
type rank = Ace | Two | Three | ... | King
type card = { suit : suit; rank : rank }
```
- Two enum variants (the *axes* of choice)
- One record (the *bundling*)
- A card is "this suit AND this rank" - 4 × 13 = 52 total

## Constructors with Payload
```ocaml
type shape =
  | Circle of float
  | Rectangle of float * float
```
- `Circle 3.0` - constructor applied to payload by juxtaposition
- `Rectangle (4.0, 6.0)` - tuple payload, parens required for multi-component

## Inline Records as Constructor Payloads
```ocaml
type tcp_state =
  | Listening
  | Connected of { peer: string; bytes_sent: int }
```
Added in OCaml 4.03 (2016), idiomatic for payloads of more than two pieces.

## Variants Are Pervasive
`bool`, `list`, `option`, `result` are all variants.

**Concepts**: variants, sum types, constructors, payloads, parameterised variants
**Keywords**: OCaml, variant, sum type, constructor, ADT, algebraic data type
**Next**: M04 · L04 · Recursive types: lists, trees, expressions