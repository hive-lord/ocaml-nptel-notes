# Memoization

# Memoization

## Memoization
A memoized version of `f` behaves just like `f` on every input. On *first* call with a given argument, it computes and stashes the answer; on subsequent calls with same argument, returns stashed answer without recomputing.

## The `memo` Combinator
```ocaml
let memo f =
  let table = Hashtbl.create 16 in
  fun x ->
    try Hashtbl.find table x
    with Not_found ->
      let y = f x in
      Hashtbl.add table x y; y
```

## Memoizing Recursive Functions
Need `memo_rec` that ties the recursive knot:
```ocaml
let memo_rec f =
  let table = Hashtbl.create 16 in
  let rec g x =
    try Hashtbl.find table x
    with Not_found ->
      let y = f g x in
      Hashtbl.add table x y; y
  in g
```

## Memoization Presumes Purity
OCaml does not track purity in the type system. Memoizing in OCaml is a *you-the-author* judgement: you must know your function is pure before wrapping it. Compiler won't catch a memoized side-effecting function.

## Examples
- Fibonacci: `memo_rec fib` turns exponential into linear
- Edit distance: dynamic programming via memoization

**Concepts**: memoization, caching, hash tables, tying the recursive knot, dynamic programming, purity
**Keywords**: OCaml, memoization, Hashtbl, fib, edit distance, dynamic programming, purity
**Next**: M07 · L06 · Module basics