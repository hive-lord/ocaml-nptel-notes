# Practice: pattern matching, by hand

# Practice: pattern matching, by hand

## Problem Structure
- **Part 1: lists** (Problems 1-7): Matching `[]` against `x :: xs`, carrying an index or running answer, one merge puzzle
- **Part 2: binary trees** (Problems 8-10): Same `'a tree` matched on `Leaf` against `Node`
- **Part 3: your own variants** (Problems 11-12): Define a small variant type and match on it, including matching on a pair of values at once

## Problem 7: `merge`
```ocaml
merge : int list -> int list -> int list
```
Merges two already-sorted lists into one sorted list.
`merge [1; 3; 5] [2; 4] = [1; 2; 3; 4; 5]` (combining step of merge sort)

## Problem 12: `nat` (the natural numbers)
Define natural numbers as a variant and operate on them.

**Concepts**: practice problems, pattern matching, recursion, lists, trees, variants, options
**Keywords**: OCaml, practice, assignment, pattern matching, match, recursion, lists, trees, variants, option
**Next**: M06 · L01 · Functions as values, revisited