# `let` bindings and shadowing

# `let` Bindings and Shadowing

## Two Forms of `let`

### `let ... in` Expression (Local)
```ocaml
let y = 5 in y + 5  (* = 10 *)
```
- Introduces a name *local* to a specific expression
- The whole form is itself an expression
- Outside the `in`, the name is no longer in scope

### Top-Level `let` (Definition)
```ocaml
let a = "Hello"
let b = "World"
let c = a ^ " " ^ b  (* "Hello World" *)
```
- Introduces a name visible to the rest of the file
- Read as "let `x = e` in *the rest of the program*"
- Each top-level `let` is in scope for every binding that follows

## Shadowing
OCaml lets you reuse a name in a new binding without mutating anything:
```ocaml
let x = 1
let x = x + 1
let x = x * 10  (* x is now 20 *)
```
Each `let x = ...` introduces a **new binding**, scoped to the rest of the chain. The right-hand sides reference the *previous* binding.

### Idiom: Shadowing for Step-by-Step Transformations
```ocaml
let s = " Hello World " in
let s = String.trim s in
let s = String.lowercase_ascii s in
s  (* "hello world" *)
```

## Sequencing with `;`
```ocaml
let _ = print_endline "hi"; 6  (* prints "hi", evaluates to 6 *)
```
- `e1; e2`: evaluate `e1`, discard, then `e2`; result is `e2`
- Sugar for `let _ = e1 in e2`
- `e1` is expected to have type `unit` (warning if not)

**Concepts**: let bindings, let-in expressions, scope, shadowing, immutability, inference rules
**Keywords**: OCaml, let binding, shadowing, scope, immutability
**Next**: M02 · L03 · Static vs dynamic semantics, and type inference