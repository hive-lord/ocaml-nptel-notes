# Operators, precedence, and common pitfalls

# Operators, Precedence, and Common Pitfalls

## Arithmetic by Type
OCaml has separate operators for `int` and `float` (float versions carry trailing dot):

| Operation | `int` | `float` |
|-----------|-------|---------|
| Add | `a + b` | `a +. b` |
| Subtract | `a - b` | `a -. b` |
| Multiply | `a * b` | `a *. b` |
| Divide | `a / b` (truncating) | `a /. b` |
| Remainder | `a mod b` | `Float.rem a b` |

### Power, Negate, Abs
| Operation | `int` | `float` |
|-----------|-------|---------|
| Power | (no built-in) | `a ** b` |
| Negate | `-a` | `-. a` |
| Absolute | `abs a` | `Float.abs a` |

- `**` is float-only; no integer `pow` in stdlib
- `-.` is the only prefix-operator-with-a-dot
- `abs_float` deprecated; prefer `Float.abs`

## Comparison and Equality
- `=` = **structural** equality (same contents, recursively, polymorphic)
- `<>` = negation of `=`
- `==` = **physical** equality (same object in memory)

## Function Application Is Its Own "Operator"
Function application = juxtaposition (no parentheses or commas):
```ocaml
succ 5        (* = 6 *)
max 3 7       (* = 7 *)
String.length "hello"  (* = 5 *)
```
- Function application is **left-associative**: `f x y` = `(f x) y`
- Parentheses only for grouping: `succ (max 3 7)`

## Operator Precedence (Tightest to Loosest)
| Level | Operators | Notes |
|-------|-----------|-------|
| 1 | `.` | record/module access |
| 2 | `f x` | function application |
| 3 | `*`, `/`, `mod`, `*.`, `/.` | multiplicative |
| 4 | `+`, `-`, `+.`, `-.` | additive |
| 5 | `^`, `@` | string/list concat |
| 6 | `<`, `=`, `>`, `<=`, `>=`, `<>` | comparisons |
| 7 | `&&` | logical and |
| 8 | `||` | logical or |
| 9 | `,` | tuple constructor |
| 10 | `;` | sequence |

## Common Pitfalls
1. **`+` instead of `+.`** - Most common beginner type error
2. **Comparison chains aren't a thing** - `0 < x < 10` parses as `(0 < x) < 10` → type error
3. **`mod` precedence** - Same as `*` and `/`, so `10 mod 3 * 2` = `(10 mod 3) * 2`

**Concepts**: operator precedence, type-specific operators, common type errors, function application
**Keywords**: OCaml, operators, precedence, type errors, function application
**Next**: M03 · L01 · Functions as values, and anonymous functions