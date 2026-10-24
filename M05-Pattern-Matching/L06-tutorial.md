# Tutorial: an interpreter for the OCaml AST

# Tutorial: an interpreter for the OCaml AST

## Tutorial: an interpreter for the OCaml AST
Reuse the OCaml AST from Module 4: `Int`, `Bool`, `Add`, `Var`, `Let_in`. Add **one** new constructor: `If` (so `Bool` earns its keep).

Three walkers, each a pattern match on the same type:
1. **`pretty`**: produce a string
2. **`depth`**: how tall is the tree
3. **`eval`**: the centrepiece; reduce an expression to a value

## The Type, Extended with `If`
```ocaml
type expr =
  | Int of int
  | Bool of bool
  | Add of expr * expr
  | If of expr * expr * expr
  | Var of string
  | Let_in of string * ty option * expr * expr
```

## Example Program
```ocaml
(* let x : int = 10 in if true then x + 5 else 0 *)
let example =
  Let_in ("x", Some T_int, Int 10,
    If (Bool true,
      Add (Var "x", Int 5),
      Int 0))
```
Should evaluate to `15`.

## Function 1: `pretty`
```ocaml
let rec pretty = function
  | Int n -> string_of_int n
  | Bool b -> string_of_bool b
  | Add (e1, e2) -> "(" ^ pretty e1 ^ " + " ^ pretty e2 ^ ")"
  | If (c, t, f) -> "if " ^ pretty c ^ " then " ^ pretty t ^ " else " ^ pretty f
  | Var x -> x
  | Let_in (x, ty_opt, e1, e2) ->
      "let " ^ x ^ (match ty_opt with None -> "" | Some t -> " : " ^ string_of_ty t) ^
      " = " ^ pretty e1 ^ " in " ^ pretty e2
```

## Function 2: `depth`
```ocaml
let rec depth = function
  | Int _ | Bool _ | Var _ -> 1
  | Add (e1, e2) | If (_, e1, e2) | Let_in (_, _, e1, e2) ->
      1 + max (depth e1) (depth e2)
```
Or-pattern `Int _ | Bool _ | Var _` collapses three leaves into one clause.

## Function 3: `eval`
```ocaml
type value = VInt of int | VBool of bool
type env = (string * value) list

let rec lookup x = function
  | [] -> None
  | (y, v) :: rest -> if x = y then Some v else lookup x rest

let rec eval env e =
  match e with
  | Int n -> Some (VInt n)
  | Bool b -> Some (VBool b)
  | Add (e1, e2) ->
      (match eval env e1, eval env e2 with
       | Some (VInt a), Some (VInt b) -> Some (VInt (a + b))
       | _ -> None)
  | If (c, t, f) ->
      (match eval env c with
       | Some (VBool true) -> eval env t
       | Some (VBool false) -> eval env f
       | _ -> None)
  | Var x -> lookup x env
  | Let_in (x, _, e1, e2) ->
      (match eval env e1 with
       | Some v -> eval ((x, v) :: env) e2
       | None -> None)
```

## Every Walker on `expr` Has the Same Shape
- One clause per constructor of `expr`
- Exhaustiveness checker tells you when you missed one
- Module 6 extracts this skeleton as a generic *fold* over `expr`

**Concepts**: worked AST walk, structural recursion, interpreter, environment, multi-purpose pattern matching
**Keywords**: OCaml, AST, expression, interpreter, eval, environment, pattern matching tutorial
**Next**: M05 · L07 · Practice: pattern matching, by hand