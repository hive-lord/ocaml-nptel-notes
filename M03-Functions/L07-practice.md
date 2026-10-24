# Practice: recursion, currying, and tail recursion

# Practice: Recursion, Currying, and Tail Recursion

This is a *Practice* chapter (worksheet, no slides/video). Problems use only Module 3 material: functions, recursion, currying, partial application, tail recursion with accumulators, mutual recursion, if-expressions. No lists, tuples, records, pattern matching.

## Problem Structure
- **Part 1: Numbers in, numbers out** (Problems 1-5): Short warm-ups on recursion over integers and strings
- **Part 2: Divisors, counting, and search** (Problems 6-9): Recursion that scans a range while carrying a running answer
- **Part 3: Recursion patterns** (Problems 10-12): A value-returning loop, a pair of mutually recursive functions, and a higher-order function

## Example Problems

### Problem 2: `multiply`
```ocaml
(* Multiply a * b using only addition, tail-recursively *)
let multiply a b =
  let rec go acc n =
    if n = 0 then acc
    else go (acc + a) (n - 1)
  in
  if b < 0 then - (go 0 (-b))
  else go 0 b
```

### Problem 3: `reverse_int`
```ocaml
let reverse_int n =
  let rec go acc m =
    if m = 0 then acc
    else go (acc * 10 + m mod 10) (m / 10)
  in
  go 0 n
```

### Problem 12: `fixpoint`
```ocaml
let rec fixpoint f x =
  let y = f x in
  if y = x then x else fixpoint f y
```

**Concepts**: practice problems, recursion, tail recursion, accumulators, mutual recursion, currying
**Keywords**: OCaml, practice, assignment, recursion, accumulator, tail-recursion, mutual recursion, currying
**Next**: M04 · L01 · Tuples