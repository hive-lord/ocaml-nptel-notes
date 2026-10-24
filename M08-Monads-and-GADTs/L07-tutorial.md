# Tutorial: a tiny well-typed evaluator

# Tutorial: a tiny well-typed evaluator

## Why This Tutorial
Capstone for the OCaml half (M01-M08). One small expression language tying together:
- The **monad pattern** (option, result, state)
- **GADTs** for type-indexed ASTs
- **Pattern matching** with type refinement

Builds the GADT-typed interpreter that Module 5 tutorial forward-pointed to. Adds an option-monad failure layer (division by zero), a pretty-printer, and an extension exercise (adding `<`).

## The Typed AST
```ocaml
type _ expr =
  | Int_lit : int -> int expr
  | Bool_lit : bool -> bool expr
  | Add : int expr * int expr -> int expr
  | Sub : int expr * int expr -> int expr
  | Mul : int expr * int expr -> int expr
  | Div : int expr * int expr -> int expr
  | Less : int expr * int expr -> bool expr
  | If : bool expr * 'a expr * 'a expr -> 'a expr
  | Var : string -> 'a expr
  | Let_in : string * 'a ty option * 'a expr * 'b expr -> 'b expr
```
Compiler turns OCaml type system into a type checker for the embedded language. Get embedded language's type safety for free.

## Combining GADTs with the Option Monad
GADT rules out type errors at compile time; option monad handles runtime "divide by zero".

```ocaml
let rec eval : type a. a expr -> a option = function
  | Int_lit n -> Some n
  | Bool_lit b -> Some b
  | Add (e1, e2) ->
      let* a = eval e1 in
      let* b = eval e2 in
      Some (a + b)
  | Div (e1, e2) ->
      let* a = eval e1 in
      let* b = eval e2 in
      if b = 0 then None else Some (a / b)
  | If (c, t, f) ->
      let* cond = eval c in
      if cond then eval t else eval f
  | Var x -> lookup x env
  | Let_in (x, _, e1, e2) ->
      let* v = eval e1 in
      eval (subst x v e2)
```

## With a GADT-typed AST, `bad1` is a Compile Error
Module 5's `bad1 = Add (Bool_lit true, _)` is now a compile error, not a runtime `None`.

**Concepts**: GADT-driven AST, type-safe evaluator, optional + result monad in evaluation
**Keywords**: OCaml, GADT, evaluator, AST, tutorial, capstone
**Next**: M08 · L08 · Practice: monads and GADTs