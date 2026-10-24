# Tutorial: Fibonacci, powers of two, fast power, digits

# Tutorial for Module 3

## This Lecture: The Module 3 Tutorial
Four small problems: Fibonacci, power-of-two test, fast power, digit counting.
Goal: consolidate recursion, accumulators, local helpers.

### Rule of Thumb
- Write the clear naive version first
- Convert with an accumulator when inputs get large

## Problem 1: Fibonacci, Naively
```ocaml
let rec fib n =
  if n < 2 then n
  else fib (n - 1) + fib (n - 2)

let _ = fib 20  (* = 6765 *)
```
Exponential time: `fib 40` is already slow.

### Linear-Time Fibonacci with Accumulator
```ocaml
let fib n =
  let rec go a b n =
    if n = 0 then a
    else go b (a + b) (n - 1)
  in go 0 1 n
```
- Tail-recursive, runs in constant stack space
- `fib 1_000_000` works (though result overflows int)

## Problem 2: Is It a Power of Two?
```ocaml
let rec is_pow2 n =
  if n <= 0 then false
  else if n = 1 then true
  else if n mod 2 = 1 then false
  else is_pow2 (n / 2)
```
- Already tail-recursive
- Base cases: `1` succeeds; zero, negatives, odds fail
- Recursive case: halve and ask again

## Problem 3: Fast Integer Power
```ocaml
let rec fast_power x n =
  if n = 0 then 1
  else if n mod 2 = 0 then
    let half = fast_power x (n / 2) in
    half * half
  else
    x * fast_power x (n - 1)
```
- Square-and-multiply: `O(log n)` multiplications
- *Not* tail-recursive (even case has `half * half` after recursive call)
- Logarithmic depth is comfortably small for typical exponents

## Problem 4: Digit Counting
```ocaml
let rec count_digits n =
  if n = 0 then 0
  else 1 + count_digits (n / 10)
```

## What You Should Be Able to Do Now
- Define functions, including anonymous functions with `fun`
- Use partial application (`add 5`)
- Write recursive functions with base and recursive cases
- Convert to tail-recursive with an accumulator
- Use local helpers via `let rec go ... in ...`
- Write mutually recursive functions with `let rec ... and ... = ...`

**Concepts**: worked recursive examples, tail vs naive recursion, memoization preview
**Keywords**: OCaml, tutorial, fibonacci, power of two, power, digits, recursion, tail recursion
**Next**: M03 · L07 · Practice: recursion, currying, and tail recursion