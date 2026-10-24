# Module 3 Revision Notes: Functions

## Key Concepts

### Functions as First-Class Values
- Functions are values like `int` or `string`
- Can be: named with `let`, passed as arguments, returned from functions, stored in data structures
- **Anonymous functions**: `fun x -> x + 1`

### Currying
```ocaml
let add x y = x + y
(* type: int -> int -> int = int -> (int -> int) *)
```
- Multi-arg function = chain of one-arg functions
- **Partial application**: `add 5` has type `int -> int`
- Enables: `List.map ((+) 10) [1;2;3]`

### Closures
```ocaml
let make_adder n = fun x -> x + n
let plus_five = make_adder 5
plus_five 100  (* = 105, remembers n=5 *)
```
- Functions capture their environment at creation
- Static scoping with value capture

### Recursion
```ocaml
let rec factorial n =
  if n = 0 then 1
  else n * factorial (n - 1)
```
- `rec` keyword required for self-reference
- Three parts: base case(s), recursive case on smaller input, termination

### Tail Recursion & Accumulators
```ocaml
let sum_up_to n =
  let rec go acc n =
    if n = 0 then acc
    else go (acc + n) (n - 1)
  in go 0 n
```
- Recursive call in **tail position** (last step)
- Compiler reuses stack frame (TCO) = constant stack space
- Pattern: extra `acc` parameter, identity element as initial value

### Local Functions & Mutual Recursion
```ocaml
let factorial n =
  let rec go acc n = ...
  in go 1 n

let rec is_even n = if n = 0 then true else is_odd (n - 1)
and is_odd n = if n = 1 then true else is_even (n - 1)
```
- `let rec ... in` hides helper from top-level namespace
- `and` glues mutually recursive functions
- TCO works across mutual recursion

---

## Module 3 Quiz

### MCQs

**Q1.** What is the type of `List.map ((+) 5)`?
- A) `int list -> int list`
- B) `int -> int list`
- C) `int list -> int`
- D) `('a -> 'b) -> 'a list -> 'b list`

**Answer: A** — Partial application: `(+) 5 : int -> int`, then `List.map` gives `int list -> int list`

**Q2.** Which of these is tail-recursive?
- A) `let rec sum = function [] -> 0 | h::t -> h + sum t`
- B) `let rec sum acc = function [] -> acc | h::t -> sum (acc+h) t`
- C) `let rec fact n = if n=0 then 1 else n * fact (n-1)`
- D) Both A and C

**Answer: B** — Recursive call is the last operation; accumulator pattern

**Q3.** What does `let f = fun x -> fun y -> x + y` evaluate to when applied as `f 3 4`?
- A) 7
- B) A function waiting for one more argument
- C) Type error
- D) 12

**Answer: A** — Curried function: `f 3` returns `fun y -> 3 + y`, then applied to `4` gives `7`

**Q4.** In `let rec go acc n = if n = 0 then acc else go (acc + n) (n - 1)`, what is the initial accumulator for summation?
- A) 1
- B) 0
- C) n
- D) -1

**Answer: B** — Identity element for addition

### MSQs

**Q5.** Which are true about currying in OCaml? (Select all that apply)
- [ ] A) Every function takes exactly one argument
- [ ] B) `int -> int -> int` means a function taking two ints
- [ ] C) Partial application is built-in
- [ ] D) Tuple arguments `(x,y)` enable currying
- [ ] E) Operators like `(+)` can be partially applied

**Answer: A, C, E** — B: it means `int -> (int -> int)`; D: tuples prevent currying

**Q6.** Which statements about recursion in OCaml are correct? (Select all that apply)
- [ ] A) `let rec` is required for recursive functions
- [ ] B) Without base case, recursion runs forever until stack overflow
- [ ] C) Tail recursion uses constant stack space
- [ ] D) Mutual recursion uses the `and` keyword
- [ ] E) All recursive functions are automatically tail-recursive

**Answer: A, B, C, D** — E: false, only tail-position calls get TCO

### NAT

**Q7.** What is `factorial 5` using the standard recursive definition?
**Answer: 120**

**Q8.** How many stack frames does a tail-recursive function use for input of size 1,000,000?
**Answer: 1** (constant)

### Code Analysis

**Q9.** What is the type of `let compose f g x = f (g x)`?
**Answer: `('b -> 'c) -> ('a -> 'b) -> 'a -> 'c`**

**Q10.** Convert to tail-recursive with accumulator:
```ocaml
let rec power x n = if n = 0 then 1 else x * power x (n-1)
```
**Answer:**
```ocaml
let power x n =
  let rec go acc n = if n = 0 then acc else go (acc * x) (n-1)
  in go 1 n
```