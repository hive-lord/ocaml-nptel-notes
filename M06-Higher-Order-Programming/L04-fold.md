# `fold`: reduce a list to a single value

# `fold`: reduce a list to a single value

## `fold`: reduce a list to a single value
`map` returns a list. `filter` returns a list. `fold` returns *anything*: a number, a string, a record, another list, a tree. If the answer is computed by walking elements and combining them, `fold` is the tool. Most general of the three; subsumes both `map` and `filter`.

## From `sum` and `all_true` to `fold`
```ocaml
let rec sum = function
  | [] -> 0
  | h :: t -> h + sum t

let rec all_true = function
  | [] -> true
  | h :: t -> h && all_true t
```
Same shape, two differences:
- Base case value (`0` vs `true`)
- Combining function (`+` vs `&&`)

```ocaml
let rec reduce f init = function
  | [] -> init
  | h :: t -> f h (reduce f init t)

let sum = reduce (+) 0
let all_true = reduce (&&) true
```

## `fold_left` Definition
```ocaml
let rec fold_left f acc = function
  | [] -> acc
  | h :: t -> fold_left f (f acc h) t
```
Accumulator comes *before* the list (small inconsistency, but convention since early OCaml).

## `fold_right`
```ocaml
let rec fold_right f lst acc = match lst with
  | [] -> acc
  | h :: t -> f h (fold_right f t acc)
```
Not tail-recursive; goes right-to-left.

## `fold_left` vs `fold_right`
| Property | `fold_left` | `fold_right` |
|----------|-------------|--------------|
| Tail-recursive | Yes | No |
| Order | Left-to-right | Right-to-left |
| Stack | Constant | O(n) |

## Activity
Express using only `List.fold_left`:
- `count_evens : int list -> int` counts even integers
- `max_of_default : int list -> int -> int` returns max or default

**Think about this**: When is each one the natural fit?

**Concepts**: fold, fold_left, fold_right, reduction, accumulator, generalization
**Keywords**: OCaml, fold, fold_left, fold_right, reduce, accumulator
**Next**: M06 · L05 · Function composition and pipelines