# Tutorial: a tiny AST for OCaml

# Tutorial: a tiny AST for OCaml

## Tutorial for Module 4 (part 1)
Module 4 introduced tuples, records, variants, recursive variants, polymorphism, `option`/`result`. This tutorial puts them to work building a small algebraic data type.

### The Example: A Tiny AST for OCaml Itself
Every OCaml expression (`5`, `x + 3`, `if y < 0 then 0 else y`, `let x = 5 in x + x`) is internally a value of a variant type inside the OCaml compiler.

## A First Cut: Literals and Arithmetic
```ocaml
type expr =
  | Int of int
  | Add of expr * expr
```
Two constructors. `Int` carries an OCaml `int`; `Add` carries *two* sub-expressions (recursion = arbitrarily nested arithmetic).

## Adding More Operators
```ocaml
type expr =
  | Int of int
  | Add of expr * expr
  | Sub of expr * expr
  | Mul of expr * expr
```

## Nested `let ... in`
```ocaml
type expr =
  | Int of int
  | Add of expr * expr
  | Sub of expr * expr
  | Mul of expr * expr
  | Var of string
  | Let_in of string * expr * expr

(* let x = 5 in let y = 10 in x + y *)
let e_let_nested =
  Let_in ("x", Int 5,
    Let_in ("y", Int 10,
      Add (Var "x", Var "y")))
```

## Design Decision: Optional Annotations
OCaml: `let x = 5 in ...` or `let x : int = 5 in ...`
The annotation is optional - perfect fit for `option`:
```ocaml
type ty = T_int | T_bool | T_float
type expr =
  | Int of int
  | Add of expr * expr
  | Var of string
  | Let_in of string * ty option * expr * expr
```

## The Shape of Module 4
- **Variants** with payloads (every `expr` constructor)
- **Recursive variants** (sub-expressions are themselves `expr`s)
- **Tuples** in constructor payloads (`expr * expr` for binary ops)
- **`option`** for the optional type annotation

**Concepts**: worked ADT design, recursive variants, abstract syntax, AST
**Keywords**: OCaml, AST, abstract syntax tree, ADT, recursive variant, tutorial
**Next**: M04 · L06 · Tutorial: a tiny file system (if exists) / M05 · L01 · Basic patterns