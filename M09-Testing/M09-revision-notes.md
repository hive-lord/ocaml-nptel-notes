# Module 9 Revision Notes: Testing

## Key Concepts

### Types vs Tests
| Catches | Types | Tests |
|---------|-------|-------|
| Type errors | ✓ | incidentally |
| Exhaustiveness | ✓ (warnings) | incidentally |
| Parametricity | ✓ | ✗ |
| Wrong output | ✗ | ✓ |
| Off-by-one | ✗ | ✓ |
| Swapped args | ✗ | ✓ |
| Performance | ✗ | ✓ (benchmarks) |

Types shrink test surface; tests target behaviour on well-typed input.

### Specifications
```ocaml
(** [pad n s] pads [s] on left with spaces to width [n].
    If [String.length s >= n], returns [s] unchanged. *)
val pad : int -> string -> string
```
- **Returns clause**: what the result *is*
- **Requires clause**: preconditions
- **Raises clause**: documented exceptions
- **Change the type** to make partiality visible (most OCaml-flavoured)

### Representation Invariants
```ocaml
type rational = { num: int; den: int }
(** AF: {num=n; den=d} represents n/d *)
(** RI: d > 0 && gcd n d = 1 *)

let rep_ok r = if r.den <= 0 || gcd r.num r.den <> 1
               then raise (Failure "RI violated") else r
```
- **Abstraction Function (AF)**: concrete → abstract
- **Rep Invariant (RI)**: predicate on concrete representation

### Test Design
- **Black-box**: from spec only (partitioning, boundaries)
- **Glass-box**: from implementation (path coverage)
- **Coverage**: `bisect_ppx` — instrument, record, report

### Unit Testing (OUnit2)
```ocaml
open OUnit2
let test_push_pop _ =
  let s = Stack.create () in
  Stack.push 7 s;
  assert_equal ~printer:string_of_int 7 (Stack.pop s)

let suite = "stack" >::: [ "push/pop" >:: test_push_pop ]
```
- Fresh state per test case
- `assert_equal ~printer` for readable failures
- `assert_raises` for exceptions

### Property-Based Testing (QCheck)
```ocaml
let test_rev_involutive =
  QCheck.Test.make
    ~name:"rev is involutive"
    ~count:1000
    QCheck.(list int)
    (fun xs -> rev (rev xs) = xs)
```
- Write *property*, library generates inputs
- **Generators**: `int`, `list int`, `list_small int`, custom
- **Shrinking**: finds minimal counterexample
- Equational reasoning → properties for free

### Model-Based Testing
```ocaml
type command = Enqueue of int | Dequeue | Size
(* Execute commands on both implementation and reference model *)
(* Compare observable results after each step *)
```
- Reference implementation = specification
- Commands + generators + shrinking
- Scales to hash tables, databases, concurrent systems

---

## Module 9 Quiz

### MCQs

**Q1.** What does the type system NOT catch?
- A) Type errors
- B) Wrong output for known input
- C) Exhaustiveness violations
- D) Parametricity violations

**Answer: B** — Types don't catch semantic bugs

**Q2.** In a spec, what does the `Requires` clause specify?
- A) What the function returns
- B) Preconditions on inputs
- C) What exceptions may be raised
- D) Example usage

**Answer: B** — Preconditions

**Q3.** What is the key difference between black-box and glass-box testing?
- A) Black-box uses random inputs
- B) Black-box derives tests from spec; glass-box from implementation
- C) Glass-box is only for OUnit2
- D) Black-box requires QCheck

**Answer: B** — Black-box = spec-driven; Glass-box = implementation-driven

**Q4.** What does `QCheck.Test.make ~count:1000 (list int) prop` do?
- A) Runs `prop` on 1000 fixed lists
- B) Generates 1000 random int lists, tests `prop` on each
- C) Tests `prop` on lists of length 1000
- D) Shrinks counterexamples to size 1000

**Answer: B** — Generates random inputs

### MSQs

**Q5.** Which are true about representation invariants? (Select all that apply)
- [ ] A) RI must hold after every public operation
- [ ] B) RI is checked by the type system
- [ ] C) `rep_ok` enforces RI at runtime
- [ ] D) AF maps abstract values to concrete
- [ ] E) RI violations indicate bugs

**Answer: A, C, E** — B: type system doesn't check RI; D: AF maps concrete→abstract

**Q6.** Which are QCheck concepts? (Select all that apply)
- [ ] A) Generators
- [ ] B) Shrinking
- [ ] C) Properties
- [ ] D) Test cases written by hand
- [ ] E) Arbitrary = generator + printer + shrinker

**Answer: A, B, C, E** — D: QCheck generates automatically

### NAT

**Q7.** How many paths through `max3 x y z` with nested ifs?
**Answer: 4**

**Q8.** What does `list_small int` generate vs `list int`?
**Answer: Smaller lists (for performance when testing expensive properties)**

### Code Analysis

**Q9.** What property should `List.rev` satisfy?
```ocaml
fun xs -> List.rev (List.rev xs) = xs
```
**Answer: Involution** — `rev (rev xs) = xs`

**Q10.** In model-based testing, what is the "reference implementation"?
**Answer:** A simple, obviously correct model (e.g., `int list ref` for a queue) used to verify the real implementation