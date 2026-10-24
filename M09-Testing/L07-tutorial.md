# Tutorial: testing the expr evaluator with OUnit2 and QCheck

# Tutorial: testing the expr evaluator with OUnit2 and QCheck

## The Function Under Test
```ocaml
type expr =
  | Num of float
  | Add of expr * expr
  | Sub of expr * expr
  | Mul of expr * expr
  | Div of expr * expr

let rec eval = function
  | Num n -> n
  | Add (a, b) -> eval a +. eval b
  | Sub (a, b) -> eval a -. eval b
  | Mul (a, b) -> eval a *. eval b
  | Div (a, b) -> eval a /. eval b
```

## Part 1: The Specification
```ocaml
(** [eval e] is the value of the arithmetic expression [e],
    where [Num n] is [n] and [Add], [Sub], [Mul], [Div]
    denote IEEE-754 float [+.], [-.], [*], [/.] on the values
    of their sub-expressions.
    [eval] never raises: division by zero follows float
    semantics ([1/0] is [infinity], [0/0] is [nan]).
    Example: [eval (Mul (Add (Num 1., Num 2.),
                         Sub (Num 4., Num 0.5))) = 10.5]. *)
val eval : expr -> float
```

## The Spec, and the Cases It Implies
| Region | Representative case |
|--------|---------------------|
| the leaf (base case) | `Num 3.0` |
| each operator, once | `Add`, `Sub`, `Mul`, `Div` of two leaves |
| asymmetric operands | `Sub (Num 2., Num 3.)`, not `3 - 3` |
| recursion (glass-box) | a two-level nested tree |
| the div-by-zero clause | `Div (Num 1., Num 0.)` is `infinity` |

## Part 2: OUnit2 Unit Tests
```ocaml
let test_add _ =
  assert_equal ~printer:string_of_float 5.0
    (eval (Add (Num 2.0, Num 3.0)))

let test_sub _ =
  assert_equal ~printer:string_of_float (-1.0)
    (eval (Sub (Num 2.0, Num 3.0)))

let test_nested _ =
  (* (1 + 2) * (4 - 0.5) = 3 * 3.5 = 10.5 *)
  let expr =
    Mul (Add (Num 1.0, Num 2.0),
         Sub (Num 4.0, Num 0.5))
  in
  assert_equal ~printer:string_of_float 10.5 (eval expr)
```

## Part 3: QCheck Properties
### A Generator for `expr`
```ocaml
let rec gen_expr depth =
  let open QCheck.Gen in
  if depth <= 0 then gen_leaf
  else
    let sub = gen_expr (depth - 1) in
    oneof [
      gen_leaf;
      map2 (fun a b -> Add (a, b)) sub sub;
      map2 (fun a b -> Sub (a, b)) sub sub;
      map2 (fun a b -> Mul (a, b)) sub sub;
      map2 (fun a b -> Div (a, b)) sub sub;
    ]
```

### Readable Counterexamples
```ocaml
let rec expr_str = function
  | Num n -> Printf.sprintf "%g" n
  | Add (a, b) -> "(" ^ expr_str a ^ " + " ^ expr_str b ^ ")"
  | Sub (a, b) -> "(" ^ expr_str a ^ " - " ^ expr_str b ^ ")"
  | Mul (a, b) -> "(" ^ expr_str a ^ " * " ^ expr_str b ^ ")"
  | Div (a, b) -> "(" ^ expr_str a ^ " / " ^ expr_str b ^ ")"
```

### The Planted Bug
```ocaml
let rec bad_eval = function
  | Num n -> n
  | Add (a, b) -> bad_eval a +. bad_eval b
  | Sub (a, b) -> bad_eval b -. bad_eval a  (* SWAPPED! *)
  | Mul (a, b) -> bad_eval a *. bad_eval b
  | Div (a, b) -> bad_eval a /. bad_eval b
```
QCheck finds the bug in seconds.

## What You Should Be Able to Do Now
- Write OUnit2 unit tests for any module: `assert_equal`, `assert_raises`, `>::`, `TestList`, `dune` integration
- Write QCheck properties for invariants: generators, properties, shrinking
- Test stateful data structures against a reference implementation
- Build a generator for a recursive type of your own, with depth bound and `QCheck.make` wrap

**Concepts**: testing tutorial, specifications, test design, OUnit2, QCheck, properties, invariants, expression evaluator, debugging
**Keywords**: OCaml, testing, OUnit2, QCheck, properties, invariants, expression evaluator, debugging
**Next**: M09 · L08 · Practice: writing tests