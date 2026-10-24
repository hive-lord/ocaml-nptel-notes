# Pattern matching on lists and trees

# Pattern Matching on Lists and Trees

## From Flat Values to Recursive Structure
- Last lecture: patterns dispatched on *flat* values (`int`, `bool`)
- Real data is *built up*: lists from `[]` and `::`, trees from `Leaf` and `Node`
- Patterns mirror the constructors

## List Patterns
```ocaml
let rec length = function
  | [] -> 0
  | _ :: t -> 1 + length t

let rec append xs ys =
  match xs with
  | [] -> ys
  | h :: t -> h :: append t ys
```

## Tree Patterns
```ocaml
type 'a tree = Leaf | Node of 'a tree * 'a * 'a tree

let rec inorder = function
  | Leaf -> []
  | Node (l, v, r) -> inorder l @ [v] @ inorder r
```

## Common Pitfalls
1. **Forgetting `let rec`** - compiler will tell you
2. **Missing the base case** - infinite recursion + non-exhaustive warning
3. **Swapping `h :: t`** - head is on the left, tail on the right

**Concepts**: pattern matching, list, tree, structural recursion, recursive variant, option
**Keywords**: OCaml, pattern matching, list, tree, recursion, cons, nil, Leaf, Node
**Next**: M05 · L03 · Nested patterns and or-patterns