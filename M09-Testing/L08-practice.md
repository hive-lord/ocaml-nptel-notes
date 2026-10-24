# Practice: writing tests

# Practice: writing tests

## Problem Structure
- **Part 1: properties** (Problems 1-4): Write a QCheck property for a given function: a single Boolean fact that should hold for every input.
- **Part 2: unit tests** (Problems 5-6): Write OUnit2 suites with example-based and boundary cases.
- **Part 3: generators and a model** (Problems 7-8): Write a generator that reaches the corners of its space, and a model-based property that pits an implementation against a reference.

## Problem 1: a property for `clamp`
```ocaml
let prop_clamp clamp (lo, hi, x) =
  let r = clamp lo hi x in
  lo <= r && r <= hi
```

## Problem 2: a property for `gcd`
```ocaml
let prop_gcd_divides gcd (a, b) =
  let g = gcd a b in
  a mod g = 0 && b mod g = 0
```

## Problem 4: cross-checking `merge` against a reference
```ocaml
let prop_merge merge (xs, ys) =
  let xs = List.sort compare xs and ys = List.sort compare ys in
  merge xs ys = List.sort compare (xs @ ys)
```

## Problem 6: boundary tests for `clamp`
```ocaml
open OUnit2
let suite_clamp clamp =
  "clamp" >::: [
    "below" >:: (fun _ -> assert_equal ~printer:string_of_int 0 (clamp 0 10 (-5)));
    "above" >:: (fun _ -> assert_equal ~printer:string_of_int 10 (clamp 0 10 99));
    "inside" >:: (fun _ -> assert_equal ~printer:string_of_int 7 (clamp 0 10 7));
    "at lo" >:: (fun _ -> assert_equal ~printer:string_of_int 0 (clamp 0 10 0));
    "at hi" >:: (fun _ -> assert_equal ~printer:string_of_int 10 (clamp 0 10 10));
  ]
```

## Problem 7: a generator that reaches the corners
```ocaml
let gen_signed : int QCheck.Gen.t =
  QCheck.Gen.oneof_weighted [
    (1, QCheck.Gen.return 0);
    (3, QCheck.Gen.int_range (-1000) (-1));
    (3, QCheck.Gen.int_range 1 1000);
  ]
```

## Problem 8: a model-based property for a counter
```ocaml
let prop_counter ops cmds =
  let c = ops.create () in
  let model = ref 0 in
  let step cmd =
    match cmd with
    | Incr -> ops.incr c; model := !model + 1
    | Add n -> ops.add c n; model := !model + n
  in
  List.iter step cmds;
  ops.get c = !model
```

**Concepts**: practice problems, property-based testing, unit testing, generators, model-based testing, QCheck, OUnit2
**Keywords**: OCaml, practice, assignment, testing, QCheck, OUnit2, property, generator, model-based testing
**Next**: M10 · L01 · What memory safety is, and why it is a security story