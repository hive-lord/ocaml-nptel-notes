# Unit testing

# Unit testing

## Unit testing
From a case table to a suite using **OUnit2**.

## From a Case Table to a Suite
```ocaml
open OUnit2

let test_push_pop_seven _ =
  let s = Stack.create () in
  Stack.push 7 s;
  assert_equal ~printer:string_of_int 7 (Stack.pop s)

let suite =
  "stack" >::: [
    "push and pop" >:: test_push_pop_seven;
  ]
```
- Each case gets a **name**; `>:::` collects them into the suite
- Names are exactly what the runner prints when a case fails
- `assert_equal` has no generic way to print arbitrary values → pass `~printer`

## Each Case from a Known State
When code under test is *stateful* (mutable stack, file, connection), each case must start from a known state, or cases contaminate one another. OCaml idiom: a helper that builds a fresh value, called at the top of every case.

## Positive and Negative Cases
```ocaml
let test_lifo _ =
  let s = Stack.create () in
  Stack.push 1 s; Stack.push 2 s; Stack.push 3 s;
  assert_equal ~printer:string_of_int 3 (Stack.pop s);
  assert_equal ~printer:string_of_int 2 (Stack.pop s);
  assert_equal ~printer:string_of_int 1 (Stack.pop s)

let test_pop_empty_raises _ =
  let s = Stack.create () in
  assert_raises Empty (fun () -> Stack.pop s)

let suite =
  "stack" >::: [
    "LIFO order" >:: test_lifo;
    "pop empty raises" >:: test_pop_empty_raises;
  ]
```

## When a Function Breaks, the Report Points at It
```
E.
==============================================================================
Error: stack:0:LIFO order.
  ...
  Empty
------------------------------------------------------------------------------
Ran: 2 tests in: 0.00 seconds.
FAILED: Cases: 2  Tried: 2  Errors: 1  Failures: 0  Skip: 0  Todo: 0
```

## What a Runner Does, and Does Not, Do
- Runs cases, reports pass/fail
- Does not generate test cases for you
- Does not know your specification

## Activity
Write an OUnit2 test case for the value-oriented `Stack`: create a fresh stack, push `7`, pop, and assert (with a printer) that the popped value is `7`.

**Concepts**: OUnit2, unit test, test suite, runner, fixture, assert_raises, dune, runtest, continuous integration
**Keywords**: OCaml, OUnit2, unit test, test suite, runner, fixture, assert_raises, dune, runtest, continuous integration
**Next**: M09 · L05 · Property-based testing with QCheck