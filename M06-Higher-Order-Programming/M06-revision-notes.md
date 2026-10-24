# Module 6 Revision Notes: Higher-Order Programming

## Key Concepts

### Higher-Order Functions
- Takes function as argument **or** returns function as result
- Extracts *pattern* of computation from *details*

### `map` — Transform Every Element
```ocaml
List.map : ('a -> 'b) -> 'a list -> 'b list
let double_each = List.map (fun x -> x * 2)
let lengths = List.map String.length
```
- Same length, same order
- Partial application: `List.map ((+) 10) [1;2;3]`

### `filter` — Keep What Passes Predicate
```ocaml
List.filter : ('a -> bool) -> 'a list -> 'a list
let positives = List.filter (fun x -> x > 0)
```
- No transformation, just selection
- Combine with `map`: `list |> filter p |> map f`
- `filter_map`: one pass doing both

### `fold` — Reduce to Single Value
```ocaml
List.fold_left : ('acc -> 'a -> 'acc) -> 'acc -> 'a list -> 'acc
List.fold_right : ('a -> 'acc -> 'acc) -> 'a list -> 'acc -> 'acc
```
- Most general: subsumes `map` and `filter`
- `fold_left`: tail-recursive, left-to-right
- `fold_right`: not tail-recursive, right-to-left

```ocaml
let sum = List.fold_left (+) 0
let product = List.fold_left ( * ) 1
let for_all p = List.fold_left (fun acc x -> acc && p x) true
```

### Pipeline Operator `|>`
```ocaml
let (|>) x f = f x
(* x |> f |> g |> h  ==  h (g (f x)) *)
```
- Write chains in order they happen
- Top-to-bottom reading order

### Function Composition
```ocaml
let compose f g = fun x -> f (g x)
let square_then_inc = compose (fun x -> x + 1) (fun x -> x * x)
```

### Folds on Other Data Structures
```ocaml
type 'a tree = Leaf | Node of 'a tree * 'a * 'a tree

let fold_pre f acc = function
  | Leaf -> acc
  | Node (l, v, r) -> fold_pre f (fold_pre f (f acc v) l) r
```
- Same pattern lifts to any recursive type
- Pre/in/post-order traversals

---

## Module 6 Quiz

### MCQs

**Q1.** What is `List.map (fun x -> x * 2) [1; 2; 3]`?
- A) `[2; 4; 6]`
- B) `[1; 4; 9]`
- C) `6`
- D) Error

**Answer: A** — Each element doubled

**Q2.** What does `List.fold_left (fun acc x -> acc + x) 0 [1; 2; 3]` return?
- A) 6
- B) `[1; 3; 6]`
- C) `0`
- D) Error

**Answer: A** — Sum: 0+1+2+3 = 6

**Q3.** What is the type of `List.filter`?
- A) `('a -> bool) -> 'a list -> 'a list`
- B) `('a -> 'b) -> 'a list -> 'b list`
- C) `('a -> 'a -> 'a) -> 'a -> 'a list -> 'a`
- D) `('a -> 'b) -> 'a -> 'b`

**Answer: A** — Predicate in, same type list out

**Q4.** What does `[1;2;3] |> List.map (fun x -> x+1) |> List.filter (fun x -> x > 3)` produce?
- A) `[4]`
- B) `[2; 3; 4]`
- C) `[3; 4]`
- D) `[]`

**Answer: A** — Map: `[2;3;4]`, Filter `>3`: `[4]`

### MSQs

**Q5.** Which are true about `fold_left` vs `fold_right`? (Select all that apply)
- [ ] A) `fold_left` is tail-recursive; `fold_right` is not
- [ ] B) `fold_left` processes left-to-right; `fold_right` right-to-left
- [ ] C) `fold_left` accumulator is first arg; `fold_right` accumulator is last
- [ ] D) Both can implement `map`
- [ ] E) `fold_right` is more efficient for large lists

**Answer: A, B, C, D** — E: false, `fold_right` uses O(n) stack

**Q6.** Which can be expressed using only `fold_left`? (Select all that apply)
- [ ] A) `map`
- [ ] B) `filter`
- [ ] C) `rev`
- [ ] D) `length`
- [ ] E) `sort`

**Answer: A, B, C, D** — E: sort needs comparison, not just fold

### NAT

**Q7.** What is `List.fold_left ( * ) 1 [2; 3; 4]`?
**Answer: 24**

**Q8.** How many arguments does `List.fold_left` take?
**Answer: 3** (function, initial accumulator, list)

### Code Analysis

**Q9.** Implement `count p xs` using `fold_left`:
```ocaml
let count p xs = List.fold_left (fun acc x -> if p x then acc + 1 else acc) 0 xs
```

**Q10.** What's the difference between these?
```ocaml
List.fold_left (fun acc x -> x :: acc) [] lst
List.fold_right (fun x acc -> x :: acc) lst []
```
**Answer:** Both reverse the list. `fold_left` is tail-recursive; `fold_right` is not.