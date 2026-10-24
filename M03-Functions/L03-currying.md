# Currying and partial application

# Currying and Partial Application

## This Lecture: Currying
- A multi-argument function is secretly a *chain* of one-argument functions
- The shape is called *currying* (after Haskell Curry)
- It enables *partial application*: supply some arguments, get back a function wanting the rest
- Explains OCaml's API style: why `List.map` takes function first, why operators have prefix forms like `(+)`

## Unfolding a Two-Argument Function
```ocaml
let add x y = x + y
(* type: int -> int -> int *)
```
Two ways to read `int -> int -> int`:
1. `int -> (int -> int)` - a function taking `int` returning a function `int -> int`
2. The curried view: `add 5` is a function waiting for the second argument

## Partial Application in Everyday Use
```ocaml
let add_five = add 5          (* int -> int *)
let _ = add_five 3            (* = 8 *)

(* Operators have prefix forms too *)
let _ = List.map ((+) 10) [1; 2; 3]  (* = [11; 12; 13] *)
let _ = List.map (( * ) 2) [1; 2; 3]  (* = [2; 4; 6] *)
```

## Argument Order Matters
Because of currying, the order of arguments determines what you can partially apply:
```ocaml
(* Good: function first, data second *)
List.map f xs
List.filter p xs
List.fold_left f init xs

(* Bad: data first, function second - can't partial apply easily *)
```

## Eta-Reduction
```ocaml
let add_five = add 5
(* equivalent to *)
let add_five x = add 5 x
(* equivalent to *)
let add_five x = (fun y -> 5 + y) x
```
The `fun` keyword is direct syntax for lambda-calculus abstraction.

## The Tuple Alternative
```ocaml
let add_tuple (x, y) = x + y
(* type: int * int -> int *)
```
- Not curried; takes a single tuple argument
- Cannot partially apply
- Use when you genuinely want a single compound argument

## Activity
Given `let add x y = x + y`:
- What is the type of `add 5`? → `int -> int`
- What does `add 5` evaluate to? → a function waiting for second argument
- What is `(add 5) 3`? → `8`

**Concepts**: currying, partial application, function returning function, higher-order utilities
**Keywords**: OCaml, currying, partial application, higher-order functions
**Next**: M03 · L04 · Tail recursion and accumulators