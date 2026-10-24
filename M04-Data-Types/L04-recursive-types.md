# Recursive types: lists, trees, expressions

# Recursive Types

## Recursive Types: Lists, Trees, Expressions
A variant becomes more interesting when a constructor carries a value of *the same type* being defined. This self-reference lets you describe data of *unbounded size* with a *fixed* type declaration.

## A First Recursive Variant: A List of Integers
```ocaml
type intlist =
  | INil
  | ICons of int * intlist
```
`INil` = empty list. `ICons` carries an `int` (head) and another `intlist` (tail).
The type `intlist` appears *inside its own definition*.

## Parameterised Variants
```ocaml
type 'a lst =
  | Nil
  | Cons of 'a * 'a lst

let ints = Cons (1, Cons (2, Cons (3, Nil)))
let strs = Cons ("hello", Cons ("world", Nil))
```
One declaration; any element type. `ints : int lst`, `strs : string lst`.

### `'a` Is a Type Variable
- Regular **variable**: name for unknown *value* (`x`, `n`, `s`)
- **Type variable**: name for unknown *type* (`'a`, `'b`)
- OCaml syntax: single quote + identifier
- Same idea: Java `List<T>`, C++ `std::vector<T>`, Rust `Vec<T>`

## OCaml's Built-in Lists Are Just Variants
```ocaml
type 'a list =
  | []
  | (::) of 'a * 'a list
```

## The `option` Type
```ocaml
type 'a option =
  | None
  | Some of 'a
```
A built-in **parameterised variant**. Mental model: a **box** that is either empty (`None`) or contains a value (`Some x`).

## Modelling Arithmetic Expressions
```ocaml
type expr =
  | Int of int
  | Add of expr * expr
  | Mul of expr * expr
```
Recursive variant models entire mini-languages.

**Concepts**: recursive variants, parameterised variants, polymorphism, option type, expression trees
**Keywords**: OCaml, recursive variant, parameterised variant, option, algebraic data type
**Next**: M04 · L05 · Tutorial: a tiny AST for OCaml