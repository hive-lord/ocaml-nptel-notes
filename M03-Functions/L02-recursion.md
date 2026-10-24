# Recursion

# Recursion

## This Lecture: Recursion
- A *recursive* function calls itself
- In OCaml, recursion replaces most uses of `for`/`while` loops
- Every recursive function has the same three-part shape:
  1. One or more *base cases* (direct answer, no recursion)
  2. A *recursive case* on a smaller input
  3. Termination: the smaller input approaches a base case
- Recursion in OCaml is cheap; the compiler is good at it
- Tail recursion (next lecture) makes it efficient

## A First Recursive Function: Factorial
```ocaml
let rec factorial n =
  if n = 0 then 1
  else n * factorial (n - 1)

let _ = factorial 5  (* = 120 *)
```
- `rec` keyword: without it, `factorial` isn't in scope in its body
- **Base case**: `if n = 0 then 1` - without one, recursion never stops
- **Recursive case**: `n * factorial (n - 1)` - defines answer in terms of smaller input

## Why `let rec` and Not Just `let`?
- OCaml rejects: `Unbound value factorial`
- Inside plain `let f = ...`, the name `f` isn't yet in scope
- `let rec` brings the name into scope inside the body

## Recursion on Lists
```ocaml
let rec length = function
  | [] -> 0
  | _ :: rest -> 1 + length rest
```
- Base case `[] -> 0`: empty list has length 0
- Recursive case `_ :: rest -> 1 + length rest`: strip head, recur

## Recursion on Numbers
```ocaml
let rec sum_up_to n =
  if n = 0 then 0
  else n + sum_up_to (n - 1)

let _ = sum_up_to 10  (* = 55 *)
```

## The Mental Model
1. **What is the input made of?** A number is either zero or one more than another number. A list is either empty or a head followed by another list.
2. **What is the base case?** Smallest possible input (usually zero for numbers, `[]` for lists).
3. **What is the recursive case?** Given a non-smallest input, reduce it toward the base case.

**Concepts**: recursion, base case, recursive case, termination, let rec, structural recursion
**Keywords**: OCaml, recursion, base case, recursive case, factorial, list recursion
**Next**: M03 · L03 · Currying and partial application