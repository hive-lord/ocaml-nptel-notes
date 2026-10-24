# Property-based testing with QCheck

# Property-based testing with QCheck

## Property-based testing with QCheck
Introduction to the OCaml flavour of PBT: the **QCheck** library.

## The Limit of Unit Testing
You only check the cases you *thought of*.

## Property-based Testing (PBT)
Write a property, let the library generate inputs.

## QCheck: the API in One Example
```ocaml
let test_rev_involutive =
  QCheck.Test.make
    ~name:"rev is involutive"
    ~count:1000
    QCheck.(list int)
    (fun xs -> rev (rev xs) = xs)
```

## Properties: What Should Be True of `rev`?
1. **Involution**: `rev (rev xs) = xs` for every list `xs`
2. **Length preservation**: `List.length (rev xs) = List.length xs`
3. **Permutation**: the multiset of elements is unchanged

## A Second Example: `List.sort`
```ocaml
let is_sorted xs =
  let rec go = function
    | [] | [_] -> true
    | a :: b :: rest -> a <= b && go (b :: rest)
  in go xs

let same_elements xs ys =
  let count x l = List.length (List.filter (( = ) x) l) in
  List.length xs = List.length ys &&
  List.for_all (fun x -> count x xs = count x ys) xs

let test_sort_sorted =
  QCheck.Test.make
    ~name:"sort produces a sorted list"
    QCheck.(list int)
    (fun xs -> is_sorted (List.sort compare xs))

let test_sort_permutation =
  QCheck.Test.make
    ~name:"sort preserves the multiset"
    QCheck.(list_small int)
    (fun xs -> same_elements (List.sort compare xs) xs)

let test_sort_length =
  QCheck.Test.make
    ~name:"sort preserves length"
    QCheck.(list int)
    (fun xs -> List.length (List.sort compare xs) = List.length xs)

let _ = QCheck_runner.run_tests ~colors:false
  [test_sort_sorted; test_sort_permutation; test_sort_length]
```

## Equational Reasoning Gives Properties for Free
In a pure functional language, every law you can state in a textbook is a property you can hand to QCheck.
- Lists: `rev (rev xs) = xs`, `length (xs @ ys) = length xs + length ys`
- Each is a single line of OCaml turned into a QCheck property
- The laws *ARE* the properties
- In imperative languages: compute lhs, store; compute rhs, store; compare

## A Short History
QuickCheck (Haskell, 1999) → QCheck (OCaml, Simon Cruanes et al.)

**Concepts**: property-based testing, QCheck, generators, shrinking, properties, invariants, equational reasoning, custom arbitraries, input space, balanced trees
**Keywords**: OCaml, QCheck, property-based testing, PBT, QuickCheck, generators, shrinking, counterexample, sorted array, balanced BST, red-black tree, custom arbitrary, input space
**Next**: M09 · L06 · Model-based testing of stateful data structures