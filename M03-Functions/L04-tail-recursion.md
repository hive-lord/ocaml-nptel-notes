# Tail recursion and accumulators

# Tail Recursion and Accumulators

## This Lecture: Tail Recursion
- Naive recursive functions crash on big inputs: *stack overflow*
  - Each call keeps a frame to finish work after the return
- The fix: rewrite so the recursive call is the *last* step
  - Compiler reuses the frame: constant stack space
- The tool: the *accumulator pattern*, one extra parameter
- After this lecture: recursion as cheap as a C `for` loop

## What a Stack Overflow Looks Like
```ocaml
let rec sum_up_to n =
  if n = 0 then 0
  else n + sum_up_to (n - 1)
```
In the recursive case `n + sum_up_to (n - 1)`, after the call returns we still need to add `n`. We must remember `n` across the call → stack frame.

Stack Overflow takes its name from this failure!

## The Accumulator Pattern
```ocaml
let sum_up_to n =
  let rec go acc n =
    if n = 0 then acc
    else go (acc + n) (n - 1)
  in go 0 n
```
- `go` takes an accumulator `acc` and the remaining `n`
- Initial accumulator = identity element (`0` for addition)
- Recursive call is the *last* step → tail position
- Compiler turns this into a loop (TCO = Tail-Call Optimisation)

## Walking Through It
The C version and tail-recursive OCaml version compile to nearly identical assembly. Think of tail recursion as "the functional way to write a `for` loop."

## Tail-Recursive Factorial
```ocaml
let factorial n =
  let rec go acc n =
    if n = 0 then acc
    else go (acc * n) (n - 1)
  in go 1 n
```

## Activity
Take the `power : int -> int -> int` function from the recursion lecture and rewrite it tail-recursively with an accumulator. Same signature; same answers; constant stack.

**Concepts**: tail call, tail-recursive functions, accumulator pattern, stack frames, TCO
**Keywords**: OCaml, tail recursion, accumulator, stack overflow, optimization
**Next**: M03 · L05 · Local functions and mutual recursion