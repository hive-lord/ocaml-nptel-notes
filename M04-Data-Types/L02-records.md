# Records

# Records

## Records
A record is a bundle, like a tuple, but with each component identified by *name* instead of position. Same role as C `struct` or Java data class.

## This Lecture: Records
- A *record* is a bundle with components named
- For 2 or 3 self-evident positions: tuple. Otherwise: record.
- Two semantic differences from C/Java:
  - **Immutable** by default
  - **Structurally compared**, like an `int` or `string`

## Declaring a Record Type
Unlike tuples, records require a *type declaration* first:
```ocaml
type point = { x : float; y : float }
let origin = { x = 0.0; y = 0.0 }
let p = { x = 3.0; y = 4.0 }
```
OCaml's records are *nominally* typed: two record types with the same fields are not interchangeable.

## Type Inference for Records Can Surprise You
```ocaml
type point = { x : float; y : float }
type vec = { x : float; y : float }

(* These are DIFFERENT types! *)
let p = { x = 1.0; y = 2.0 }  (* inferred as point, first matching type *)
```

## Functional Update
```ocaml
let p = { x = 3.0; y = 4.0 }
let p' = { p with x = 5.0 }  (* { x = 5.0; y = 4.0 } *)
```
Creates a new record with specified fields changed, others copied.

## Accessing Fields
```ocaml
let distance_from_origin p = sqrt (p.x *. p.x +. p.y *. p.y)
```

## Activity
Define a record type `book` with fields `title : string`, `author : string`, `year : int`. Create one. Define `book_title` that returns the title.

**Concepts**: records, named fields, record types, functional update, dot access
**Keywords**: OCaml, record, named fields, record type, functional update
**Next**: M04 · L03 · Variants (sum types)