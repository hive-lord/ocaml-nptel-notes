# Module 7 Revision Notes: Side Effects & Modular Programming

## Key Concepts

### Mutable References (`ref`)
```ocaml
let counter = ref 0      (* creation *)
let _ = !counter         (* read *)
counter := !counter + 1  (* write *)
```
- Breaks equational reasoning: `f x` can give different answers
- OCaml is **functional-first**: mutation available, not default
- Use cases: counters, memoization tables, caches

### Mutable Records & Arrays
```ocaml
type counter = { mutable count: int }
let c = { count = 0 }
c.count <- c.count + 1

let a = [| 1; 2; 3 |]
a.(1) <- 999
```
- `ref` = single-field mutable record
- Arrays: fixed-size, O(1) access, in-place update
- `for`/`while` loops for imperative iteration

### Exceptions
```ocaml
exception MyError of string
raise (MyError "oops")

try risky () with
| MyError msg -> handle msg
| Not_found -> default
```
- **Exceptions**: unexpected failures ("should not happen")
- **`option`/`result`**: expected, predictable outcomes
- Built-in: `Not_found`, `Invalid_argument`, `Failure`, `Division_by_zero`

### Streams & Laziness
```ocaml
(* Thunk: delayed computation *)
let thunk = fun () -> expensive ()

(* Lazy: memoised thunk *)
let lazy_val = lazy (expensive ())
Lazy.force lazy_val

(* Stream: infinite sequence *)
type 'a stream = Cons of 'a * (unit -> 'a stream)
```
- Compute on demand, not upfront
- Fibonacci stream, Sieve of Eratosthenes

### Memoization
```ocaml
let memo f =
  let table = Hashtbl.create 16 in
  fun x ->
    try Hashtbl.find table x
    with Not_found ->
      let y = f x in Hashtbl.add table x y; y
```
- Requires **purity** (OCaml doesn't enforce this)
- `memo_rec` for recursive functions

### Modules
```ocaml
module Stack = struct
  type 'a t = 'a list
  let empty = []
  let push x s = x :: s
  let pop = function [] -> None | x::xs -> Some (x, xs)
end
```
- Every `.ml` file = a module
- Contains values, types, exceptions, submodules
- Convention: principal type named `t` (`Stack.t`)

### Signatures (Interfaces)
```ocaml
module type STACK = sig
  type 'a t
  val empty : 'a t
  val push : 'a -> 'a t -> 'a t
  val pop : 'a t -> ('a * 'a t) option
end

module Stack : STACK = struct ... end
```
- **Abstract types**: `type 'a t` hides implementation
- Enables representation changes without breaking callers
- `.mli` files = external signatures

### Functors
```ocaml
module MakeMap (Ord : ORDERED_TYPE) = struct
  type key = Ord.t
  type 'a t = ...
end
```
- Functions from modules to modules
- Parameterised by module with required operations
- `Map.Make`, `Set.Make`, `Hashtbl.Make`

---

## Module 7 Quiz

### MCQs

**Q1.** What does `!` do on a `ref`?
- A) Creates a new reference
- B) Reads the value
- C) Writes the value
- D) Deletes the reference

**Answer: B** — `!r` reads, `r := v` writes

**Q2.** Which is true about OCaml arrays?
- A) They are immutable
- B) They can grow dynamically
- C) Access is O(1) with bounds checking
- D) They use `::` for construction

**Answer: C** — Fixed-size, mutable, bounds-checked

**Q3.** What is the difference between `option` and exceptions?
- A) No difference
- B) `option` for expected cases, exceptions for unexpected
- C) Exceptions are faster
- D) `option` cannot carry data

**Answer: B** — `option`/`result` for predictable outcomes; exceptions for "should not happen"

**Q4.** What does `module type S = sig type t val x : t end` declare?
- A) A module implementation
- B) A module signature (interface)
- C) A functor
- D) A record type

**Answer: B** — `sig ... end` = signature

### MSQs

**Q5.** Which break equational reasoning? (Select all that apply)
- [ ] A) `ref` mutation
- [ ] B) `Array` mutation
- [ ] C) Pure functions
- [ ] D) Exceptions
- [ ] E) `let` shadowing

**Answer: A, B, D** — C: pure functions preserve it; E: shadowing is not mutation

**Q6.** Which are true about functors? (Select all that apply)
- [ ] A) Functors take modules as input, produce modules as output
- [ ] B) Functors are first-class values (can be stored in lists)
- [ ] C) `Map.Make` is a functor
- [ ] D) Functors enable generic data structures
- [ ] E) Functors are the same as Java generics

**Answer: A, C, D** — B: functors live at module level, not value level; E: similar but different mechanism

### NAT

**Q7.** What is the result of `let a = ref 5 in a := !a + 3; !a`?
**Answer: 8**

**Q8.** How many type parameters does `('a, 'e) result` have?
**Answer: 2**

### Code Analysis

**Q9.** What's wrong with this memoization?
```ocaml
let bad_memo f =
  let table = Hashtbl.create 16 in
  fun x ->
    if Hashtbl.mem table x then Hashtbl.find table x
    else let y = f x in Hashtbl.add table x y; y
```
**Answer: Race condition in concurrent code** — Use `try/with Not_found` pattern instead

**Q10.** Why use abstract types in signatures?
```ocaml
module type STACK = sig type 'a t ... end
```
**Answer:** Hide implementation — allows changing representation (list → array) without breaking clients