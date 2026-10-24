# Module 4 Revision Notes: Data Types

## Key Concepts

### Three Building Blocks
1. **Tuples** — bundle by position: `(3, "hi")` type `int * string`
2. **Records** — bundle by name: `{x = 3; y = 4}` type `point`
3. **Variants** — one of several shapes: `Some 3` | `None`

### Tuples
- Fixed arity = part of type: `int * int` ≠ `int * int * int`
- Cartesian product: `int * bool` has `|int| × |bool|` values
- Use for 2-3 self-evident positions; otherwise use records
- Multiple return values: `let divmod a b = (a/b, a mod b)`

### Records
- Require type declaration: `type point = { x: float; y: float }`
- **Nominal typing**: two record types with same fields are different
- **Immutable** by default
- **Structural equality**: `{x=1;y=2} = {x=1;y=2}` is true
- Functional update: `{ r with x = 5 }`
- Field access: `p.x`, `p.y`

### Variants (Sum Types)
```ocaml
type shape =
  | Circle of float
  | Rectangle of float * float
```
- **Product types** (tuples/records) = AND
- **Sum types** (variants) = OR
- Constructor applied by juxtaposition: `Circle 3.0`
- Multi-field payload needs parens: `Rectangle (4.0, 6.0)`
- Inline records: `Connected of { peer: string; bytes: int }`

### Parameterised Variants
```ocaml
type 'a option = None | Some of 'a
type 'a list = [] | (::) of 'a * 'a list
```
- `'a` = type variable (like Java `List<T>`)
- One declaration, any element type

### Recursive Variants
```ocaml
type 'a tree = Leaf | Node of 'a tree * 'a * 'a tree
type expr =
  | Int of int
  | Add of expr * expr
  | Var of string
```
- Type appears in its own definition
- Enables unbounded data with fixed declaration
- Foundation for ASTs, trees, lists

---

## Module 4 Quiz

### MCQs

**Q1.** What is the type of `(1, true, "hello")`?
- A) `int * bool * string`
- B) `(int * bool) * string`
- C) `int * (bool * string)`
- D) `int * bool * string * unit`

**Answer: A** — Triple has type `int * bool * string`

**Q2.** Given `type point = {x: float; y: float}` and `type vec = {x: float; y: float}`, what is the type of `{x=1.0; y=2.0}`?
- A) `point`
- B) `vec`
- C) `point` or `vec` (ambiguous)
- D) Type error

**Answer: A** — OCaml picks the first matching type in scope (nominal typing)

**Q3.** Which defines a variant for a binary tree of integers?
- A) `type tree = Leaf | Node of int * tree * tree`
- B) `type tree = Leaf of int | Node of tree * tree`
- C) `type tree = Leaf | Node of tree * int * tree`
- D) Both A and C

**Answer: C** — Standard form: `Node of left * value * right`

**Q4.** What does `Some 5` have as its type?
- A) `int option`
- B) `int`
- C) `option int`
- D) `Some int`

**Answer: A** — Constructor `Some` applied to `int` gives `int option`

### MSQs

**Q5.** Which are true about OCaml records? (Select all that apply)
- [ ] A) Records are mutable by default
- [ ] B) Records use structural equality
- [ ] C) Two record types with identical fields are compatible
- [ ] D) Functional update creates a new record
- [ ] E) Records require a type declaration

**Answer: B, D, E** — A: immutable by default; C: nominal typing makes them incompatible

**Q6.** Which are valid variant declarations? (Select all that apply)
- [ ] A) `type t = A | B of int | C of int * int`
- [ ] B) `type t = A of int | A of string` (duplicate constructor)
- [ ] C) `type 'a t = Nil | Cons of 'a * 'a t`
- [ ] D) `type t = { x: int }` (record syntax in variant)
- [ ] E) `type t = A of { x: int; y: int }` (inline record)

**Answer: A, C, E** — B: duplicate constructors not allowed; D: that's a record, not a variant

### NAT

**Q7.** How many constructors does `type 'a list = [] | (::) of 'a * 'a list` have?
**Answer: 2**

**Q8.** What is the arity of the tuple `(1, 2, 3, 4, 5)`?
**Answer: 5**

### Code Analysis

**Q9.** Complete the `eval` function for this AST:
```ocaml
type expr = Int of int | Add of expr * expr | Mul of expr * expr
let rec eval = function
  | Int n -> n
  | Add (e1, e2) -> eval e1 + eval e2
  | Mul (e1, e2) -> _
```
**Answer: `eval e1 * eval e2`**

**Q10.** Why does this fail?
```ocaml
type t = A of int | B of int
let f = function A x -> x | B x -> x + 1
let _ = f (A 5)
```
**Answer: It doesn't fail** — This is valid OCaml. Exhaustive match on both constructors.