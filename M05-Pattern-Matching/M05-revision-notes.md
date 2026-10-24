# Module 5 Revision Notes: Pattern Matching

## Key Concepts

### Basic Patterns
- **Literal**: `| 0 -> "zero"`
- **Variable**: `| x -> x + 1` (binds matched value)
- **Wildcard**: `| _ -> "other"` (matches anything, binds nothing)

### Order Matters
```ocaml
(* Correct: specific first *)
let f = function
  | 0 -> "zero"
  | n -> "non-zero"

(* Wrong: wildcard first *)
let f = function
  | _ -> "anything"  (* matches everything! *)
  | 0 -> "zero"      (* UNREACHABLE - Warning 11 *)
```
**Rule**: Specific patterns first, general patterns last.

### Exhaustiveness (Warning 8)
Compiler checks if all cases covered:
```ocaml
let label = function
  | 0 -> "zero"
  | 1 -> "one"
(* Warning 8: missing case 2 *)
```
Fix: add more clauses or wildcard `_`.

### Redundant Clauses (Warning 11)
```ocaml
let f = function
  | x -> "any"    (* matches everything *)
  | 0 -> "zero"   (* UNREACHABLE - Warning 11 *)
```

### Patterns on Recursive Types
```ocaml
(* Lists *)
let rec length = function
  | [] -> 0
  | _ :: t -> 1 + length t

(* Trees *)
type 'a tree = Leaf | Node of 'a tree * 'a * 'a tree
let rec inorder = function
  | Leaf -> []
  | Node (l, v, r) -> inorder l @ [v] @ inorder r
```

### Nested Patterns
```ocaml
Some (x, _)  (* tuple inside constructor *)
(0, _) :: _  (* tuple inside cons *)

(* Diagonal idiom: match pair *)
let combine a b =
  match a, b with
  | None, None -> "both empty"
  | Some _, None -> "first only"
  | None, Some _ -> "second only"
  | Some _, Some _ -> "both present"
```
- `match a, b with` = `match (a, b) with`
- Exhaustiveness checked on cross-product

### Or-patterns
```ocaml
let is_east_or_west = function
  | East | West -> true
  | _ -> false
```
- `p1 | p2` matches either pattern
- All alternatives must bind same variables

### Guards (`when`)
```ocaml
let classify n =
  match n with
  | n when n > 0 -> "positive"
  | n when n < 0 -> "negative"
  | _ -> "zero"
```
- **Guards suppress exhaustiveness** — compiler treats `when` as "might fail"
- Always end with unguarded clause: `| _ -> ...`

### Interpreter Pattern
Every walker on a recursive ADT has same shape:
- One clause per constructor
- Recurse on sub-expressions
- Exhaustiveness checker ensures completeness

---

## Module 5 Quiz

### MCQs

**Q1.** What warning does this produce?
```ocaml
let f = function
  | 0 -> "zero"
  | x -> "non-zero"
  | _ -> "wildcard"
```
- A) Warning 8 (non-exhaustive)
- B) Warning 11 (unused match case)
- C) No warning
- D) Error

**Answer: B** — Third clause unreachable (Warning 11)

**Q2.** What is the result of `match (Some 5, None) with | Some x, Some y -> x+y | _ -> 0`?
- A) 5
- B) 0
- C) Type error
- D) Exception

**Answer: B** — Second is `None`, so falls to `_ -> 0`

**Q3.** Which pattern matches a list whose head is a pair?
- A) `(x, y) :: _`
- B) `x :: y :: _`
- C) `:: (x, y) _`
- D) `[(x, y)]`

**Answer: A** — Cons pattern with tuple pattern as head

**Q4.** What does this evaluate to?
```ocaml
let f = function
  | n when n > 0 -> "pos"
  | n when n < 0 -> "neg"
  | _ -> "zero"
in f 0
```
- A) "pos"
- B) "neg"
- C) "zero"
- D) Warning 8, then "zero"

**Answer: C** — `0` matches the wildcard

### MSQs

**Q5.** Which statements about exhaustiveness are true? (Select all that apply)
- [ ] A) Wildcard `_` on variant types suppresses exhaustiveness checking
- [ ] B) Guards (`when`) suppress exhaustiveness checking
- [ ] C) The compiler can prove arithmetic facts in guards
- [ ] D) Warning 8 means missing cases; Warning 11 means unreachable cases
- [ ] E) `match x with | 0 -> "zero" | 1 -> "one"` is exhaustive for `int`

**Answer: A, B, D** — C: compiler cannot reason about guards; E: `int` has infinite cases

**Q6.** Which are valid or-patterns? (Select all that apply)
- [ ] A) `1 | 2 | 3 -> ...`
- [ ] B) `Some x | Some y -> ...`
- [ ] C) `Some x | None -> ...`
- [ ] D) `x | y -> ...` (both variables)
- [ ] E) `A (1) | A (2) -> ...` (constructor with literal)

**Answer: A, C, E** — B: variables must be same name; D: variables must be same name

### NAT

**Q7.** How many cases does the exhaustiveness checker enumerate for `match (b1, b2) with` where both are `bool`?
**Answer: 4** (2×2 = 4 combinations)

**Q8.** In the diagonal idiom `match a, b with`, how many clauses are needed for two `option` values?
**Answer: 4** (None/None, Some/None, None/Some, Some/Some)

### Code Analysis

**Q9.** Fix the non-exhaustive match:
```ocaml
let rec sum = function
  | [] -> 0
  | h :: t -> h + sum t
```
**Answer: It IS exhaustive** — `[]` and `h::t` cover all list cases

**Q10.** What's wrong with this?
```ocaml
let classify n =
  match n with
  | n when n >= 0 -> "non-neg"
  | n when n < 0 -> "neg"
```
**Answer: Warning 8 (all clauses guarded)** — Add `| _ -> "zero"` or similar unguarded clause