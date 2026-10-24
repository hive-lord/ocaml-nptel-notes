# Module 8 Revision Notes: Monads and GADTs

## Key Concepts

### Monads
A **monad** = a type `m` + `return` + `bind` (or `let*`) for sequencing computations.

#### Option Monad
```ocaml
let ( let* ) o f = match o with None -> None | Some x -> f x
let* addr = user.address in
let* zip = addr.zip in
Some zip
```
- Short-circuits on `None`
- Eliminates "pyramid of doom"

#### Monad Laws (must hold for lawful monads)
```ocaml
(* Left identity  *)  let* x = return v in f x  ===  f v
(* Right identity *)  let* x = m in return x    ===  m
(* Associativity  *)  let* y = (let* x = m in f x) in g y  ===  let* x = m in let* y = f x in g y
```
- Enable safe refactoring of `let*` chains

#### List Monad (Non-determinism)
```ocaml
let* x = [1;2;3] in
let* y = [10;20] in
Some (x + y)
(* [11;21;12;22;13;23] — all combinations *)
```
- `return x = [x]`
- `bind xs f = List.concat_map f xs`

#### Result Monad
```ocaml
type ('a, 'e) result = Ok of 'a | Error of 'e
let* x = Ok 5 in Ok (x + 1)  (* Ok 6 *)
let* x = Error "oops" in Ok (x + 1)  (* Error "oops" *)
```
- Like `option` but failure carries payload
- Short-circuits on first `Error`

#### Common Monad Interface
```ocaml
module type MONAD = sig
  type 'a t
  val return : 'a -> 'a t
  val bind : 'a t -> ('a -> 'b t) -> 'b t
end
```

### State Monad
Threads state through computations without mutation:
```ocaml
module type STATE_MONAD = sig
  type state
  type 'a t
  val get : state t
  val set : state -> unit t
  val run : 'a t -> state -> 'a * state
end
```

### GADTs (Generalized Algebraic Data Types)
Variants where each constructor can have a **different type index**:

```ocaml
type _ expr =
  | Int_lit : int -> int expr
  | Bool_lit : bool -> bool expr
  | Add : int expr * int expr -> int expr
  | If : bool expr * 'a expr * 'a expr -> 'a expr
```

#### Type Refinement in Pattern Matching
```ocaml
let rec eval : type a. a expr -> a = function
  | Int_lit n -> n
  | Bool_lit b -> b
  | Add (e1, e2) -> eval e1 + eval e2
  | If (c, t, f) -> if eval c then eval t else eval f
```
- `type a.` = locally abstract type (required for polymorphic recursion)
- Each branch refines `a` based on constructor

### GADT Use Cases
1. **Typed pretty-printers**: `show : type a. a ty -> a -> string`
2. **Type-safe builders**: Prevent illegal states at compile time
3. **Length-indexed vectors**: Safe `hd`/`tl`, equal-length `zip`

### Heterogeneous Lists (HLists)
```ocaml
type _ hlist =
  | HNil : unit hlist
  | HCons : 'a ty * 'b hlist -> ('a * 'b) hlist
```
- Mixed-type lists with type-level tracking
- Used by `Printf.printf` internally

---

## Module 8 Quiz

### MCQs

**Q1.** What does `let* x = Some 5 in Some (x + 1)` evaluate to?
- A) `Some 6`
- B) `Some 5`
- C) `None`
- D) Type error

**Answer: A** — Binds `x = 5`, computes `Some 6`

**Q2.** Which monad law states `let* x = m in return x === m`?
- A) Left identity
- B) Right identity
- C) Associativity
- D) Commutativity

**Answer: B** — Right identity

**Q3.** What is `bind [1;2] (fun x -> [x; x*10])` in the list monad?
- A) `[1; 10; 2; 20]`
- B) `[[1; 10]; [2; 20]]`
- C) `[1; 2; 10; 20]`
- D) `[1; 2]`

**Answer: A** — `concat_map`: map then concatenate

**Q4.** In a GADT, what does `type a.` mean in `let rec eval : type a. a expr -> a`?
- A) Type annotation for return value
- B) Locally abstract type (polymorphic recursion)
- C) Type variable declaration
- D) Existential type

**Answer: B** — Required when function recurses at different type

### MSQs

**Q5.** Which are true about the option monad? (Select all that apply)
- [ ] A) `return x = Some x`
- [ ] B) `bind None f = None`
- [ ] C) `bind (Some x) f = f x`
- [ ] D) `let*` chains short-circuit on first `None`
- [ ] E) It satisfies all three monad laws

**Answer: A, B, C, D, E** — All correct

**Q6.** Which are GADT features? (Select all that apply)
- [ ] A) Constructors can have different result types
- [ ] B) Pattern matching refines type variables
- [ ] C) Enable typed ASTs that can't represent ill-typed programs
- [ ] D) Same as regular variants with more syntax
- [ ] E) Used by `Printf.printf` internally

**Answer: A, B, C, E** — D: false, they're strictly more expressive

### NAT

**Q7.** How many monad laws are there?
**Answer: 3**

**Q8.** What is the type of `If` in the typed AST?
**Answer: `bool expr * 'a expr * 'a expr -> 'a expr`**

### Code Analysis

**Q9.** Why does this fail without `type a.`?
```ocaml
let rec eval = function
  | Int_lit n -> n
  | Bool_lit b -> b
```
**Answer:** OCaml tries to unify `int` and `bool` for the single type variable `'a`

**Q10.** Complete the `eval` for division with option monad:
```ocaml
| Div (e1, e2) ->
    let* a = eval e1 in
    let* b = eval e2 in
    _
```
**Answer: `if b = 0 then None else Some (a / b)`**