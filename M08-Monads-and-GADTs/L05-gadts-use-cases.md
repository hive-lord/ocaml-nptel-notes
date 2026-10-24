# GADTs: use cases beyond toy interpreters

# GADTs: use cases beyond toy interpreters

## GADTs: use cases beyond toy interpreters
Toy expression-AST is the standard "first example" but leaves misleading impression: that GADTs are mostly for interpreters. They are not. Three idioms where GADTs earn their keep:
1. Typed pretty-printers
2. Type-safe builders that prevent illegal states
3. Length-indexed lists: safe `hd`/`tl`, equal-length `zip`

Common thread: *type witnesses* - values whose runtime shape is uninteresting but whose *type* carries information the program needs at compile time. GADTs are how OCaml encodes witnesses naturally.

## Use 1: Typed Pretty-Printers
```ocaml
type _ ty =
  | T_int : int ty
  | T_bool : bool ty
  | T_pair : 'a ty * 'b ty -> ('a * 'b) ty

let rec show : type a. a ty -> a -> string = function
  | T_int -> string_of_int
  | T_bool -> string_of_bool
  | T_pair (a, b) -> fun (x, y) -> "(" ^ show a x ^ ", " ^ show b y ^ ")"
```

## Use 2: Type-Safe Builders
```ocaml
type _ query =
  | Select : 'a select_ty -> 'a query
  | Where : 'a query -> ('a -> bool) -> 'a query
```
Type catches a bad filter: you cannot `ORDER BY` a non-`SELECT`.

## Use 3: Length-Indexed Lists (Vectors)
```ocaml
type _ vec =
  | Nil : 'a vec
  | Cons : 'a * 'a vec -> 'a vec
```
Wait, that's not length-indexed. Real version uses Peano numbers:
```ocaml
type z
type 'n s

type ('a, 'n) vec =
  | Nil : ('a, z) vec
  | Cons : 'a * ('a, 'n) vec -> ('a, 'n s) vec
```

### Safe `hd` and `tl`
```ocaml
let hd : type a n. (a, n s) vec -> a = function
  | Cons (x, _) -> x
(* Nil case impossible - compiler knows! *)
```

### `zip`: Equal Lengths Only
```ocaml
let zip : type a b n. (a, n) vec -> (b, n) vec -> (a * b, n) vec = ...
(* Different lengths don't type-check *)
```

## Two More Idioms, Next Lecture
- Heterogeneous lists
- Format strings and `Printf`

**Concepts**: type witnesses, typed pretty-printers, type-safe builders, Peano encoding, length-indexed lists
**Keywords**: OCaml, GADT, type witness, query builder, length-indexed list, Peano encoding, vec
**Next**: M08 · L06 · GADTs: hlists and witnesses