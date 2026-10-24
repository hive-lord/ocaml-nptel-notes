# Module 2 Revision Notes: Expressions

## Key Concepts

### Primitive Literal Kinds
| Type | Example | Notes |
|------|---------|-------|
| `int` | `42`, `-7` | 63-bit signed on 64-bit |
| `float` | `3.14`, `2.0` | IEEE-754 double precision |
| `bool` | `true`, `false` | |
| `char` | `'a'`, `'\n'` | Single byte |
| `string` | `"hello"` | Byte string |
| `unit` | `()` | Single placeholder value |

### Integer Division
- `/` truncates **toward zero**: `(-7) / 2 = -3`, `(-7) mod 2 = -1`
- Python floors: `(-7) // 2 = -4`

### No Implicit int/float Promotion
```ocaml
1 + 2.0  (* Error! *)
1 +. 2.0 (* OK *)
float_of_int 1 +. 2.0  (* OK *)
```
- Every operator has **one fixed meaning** determined by symbol alone
- `+` for ints, `+.` for floats, etc.

### Float Arithmetic is Approximate
```ocaml
0.1 +. 0.2 = 0.300000000000000044  (* not 0.3! *)
```

### Two Forms of `let`
1. **`let ... in`** (local): `let x = 5 in x + x` — scoped to expression
2. **Top-level `let`** (definition): `let x = 5` — visible to rest of file

### Shadowing
```ocaml
let x = 1
let x = x + 1  (* new binding, old unchanged *)
let x = x * 10 (* x = 20 *)
```
- Not mutation — new binding shadows old
- Idiom for step-by-step transformations

### Sequencing with `;`
```ocaml
print_endline "hi"; 6  (* prints "hi", evaluates to 6 *)
```
- `e1; e2` = evaluate `e1`, discard, then `e2`
- `e1` expected to have type `unit` (warning if not)

### Type Inference
- Operators **drive inference**: `x + y` forces `x,y : int`
- `x +. y` forces `x,y : float`
- One operator changes everything: `mean` vs `mean_f`

---

## Module 2 Quiz

### MCQs

**Q1.** What is the result of `(-7) / 2` in OCaml?
- A) -4
- B) -3.5
- C) -3
- D) Error

**Answer: C** — Truncation toward zero

**Q2.** Which operator concatenates strings?
- A) `+`
- B) `+.` 
- C) `^`
- D) `@`

**Answer: C** — `^` for strings, `@` for lists

**Q3.** What is the type of `fun x -> x +. 1.0`?
- A) `int -> int`
- B) `float -> float`
- C) `int -> float`
- D) `float -> int`

**Answer: B** — `+.` forces `x : float`, result `float`

**Q4.** What does the following evaluate to?
```ocaml
let x = 5 in
let x = x * 2 in
x + 1
```
- A) 11
- B) 10
- C) 6
- D) Error (duplicate binding)

**Answer: A** — Shadowing: inner `x` = 10, result = 11

### MSQs

**Q5.** Which statements about OCaml arithmetic are true? (Select all that apply)
- [ ] A) `+` works on both ints and floats
- [ ] B) `+.` is float addition
- [ ] C) `1 + 2.0` is a type error
- [ ] D) `mod` works on floats
- [ ] E) `/` on ints truncates toward zero

**Answer: B, C, E** — A: false (separate operators); D: false (use `Float.rem`)

**Q6.** Which are valid ways to convert between int and float? (Select all that apply)
- [ ] A) `float_of_int 5`
- [ ] B) `int_of_float 5.0`
- [ ] C) `Float.of_int 5`
- [ ] D) `Float.to_int 5.0`
- [ ] E) `(5 :> float)` (coercion)

**Answer: A, B, C, D** — E: OCaml has no implicit coercion syntax

### NAT

**Q7.** What is `(-7) mod 2` in OCaml?
**Answer: -1**

**Q8.** How many bits is an `int` on a 64-bit machine?
**Answer: 63**

### Code Analysis

**Q9.** What type does OCaml infer for:
```ocaml
let f x y = x *. y +. 1.0
```
**Answer: `float -> float -> float`** — `*.` and `+.` force float

**Q10.** Fix the type error:
```ocaml
let area r = 3.14159 * r * r
```
**Answer:** Use float operators: `3.14159 *. r *. r` or `float_of_int 314159 /. 100000. *. r *. r`