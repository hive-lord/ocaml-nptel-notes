# Static vs dynamic semantics, and type inference

# Static vs Dynamic Semantics, and Type Inference

## Static vs Dynamic Semantics
OCaml lands on the *more static* end: many errors that Python/JavaScript discover at runtime, OCaml rejects at compile time. The mechanism: *type inference* - safety without syntactic burden of annotations.

## Inference for a Simple Function
```ocaml
let mean x y = (x + y) / 2
```
- `+` has type `int -> int -> int`
- In `x + y`, both operands must be `int` → `x : int`, `y : int`
- `/` is integer division, type `int -> int -> int`, literal `2` is `int`
- Function body has type `int`
- Function takes `x:int`, `y:int`, returns `int` → `mean : int -> int -> int`

### Float Version Changes Everything
```ocaml
let mean_f x y = (x +. y) /. 2.0
(* val mean_f : float -> float -> float = <fun> *)
```
The operator *drives the inference*.

## A Trickier Example
```ocaml
let mag x y = sqrt (x *. x +. y *. y)
```
- `sqrt : float -> float`, argument must be `float`
- So `x *. x +. y *. y` must be `float`
- `*.` and `+.` force operands to `float`
- Hence `x : float`, `y : float`, result `float`

Inference chained four operator constraints. In Java/C++ you'd write `float mag(float x, float y)` with all types explicit.

## Activity
What type does OCaml infer for:
```ocaml
let f x y = x +. y *. 2.0
```
Answer: `float -> float -> float` (literal `2.0` is `float`, `*.` forces `y:float`, `+.` forces `x:float`)

**Concepts**: static typing, dynamic typing, type errors, type inference, type signatures
**Keywords**: OCaml, static typing, dynamic typing, type inference, Hindley-Milner, type errors
**Next**: M02 · L04 · Operators, precedence, and common pitfalls