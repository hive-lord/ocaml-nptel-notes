# Literals: integers, floats, booleans, strings

# Literals

## Literals
The simplest expressions are the ones that are *already values*: they need no evaluation at all. We call those *literals*. OCaml's primitive literal kinds are `int`, `float`, `bool`, `char`, `string`, and `unit`.

### Primitive Literal Kinds

| Type | Example Literal | What It Represents |
|------|----------------|-------------------|
| `int` | `42`, `-7`, `0` | Whole number, signed, 63-bit on 64-bit |
| `float` | `3.14`, `2.0` | IEEE-754 double-precision |
| `bool` | `true`, `false` | Boolean |
| `char` | `'a'`, `'\n'` | Single byte |
| `string` | `"hello"`, `""` | Byte string |
| `unit` | `()` | The single placeholder value |

### Negative Integer Division: Toward Zero
OCaml's integer division `/` *truncates toward zero*, not toward negative infinity.
- `(-7) / 2 = -3` (and `(-7) mod 2 = -1`)
- Python 3 and many other languages floor instead, giving `-4`

### Floating-Point Numbers
OCaml's `float` is exactly IEEE 754 double precision: 64 bits (1 sign, 11 exponent, 52 fraction). Range: ±10^308, ~15-17 significant digits.

### No Implicit `int` / `float` Promotion
```ocaml
1 + 2.0  (* Error! *)
```
Why? Two reasons:
1. **Practical**: Every operator has one fixed meaning determined by the symbol alone
2. **Principled**: When you see `a + b`, you know both are `int` without checking anything else

Explicit conversions: `float_of_int`, `int_of_float`, `Float.of_int`, `Float.to_int`

### Floating-Point Is Approximate
```ocaml
0.1 +. 0.2 = 0.300000000000000044  (* not 0.3! *)
```
This is a fundamental property of IEEE 754, same in Python, Java, JavaScript.

### Strings
Strings are byte sequences. Concatenation uses `^` (not `+`).

### Activity: Quick Check
- Why does `1 + 2.0` fail? Because `+` takes two `int`s, `2.0` is `float`
- Why does `(-7) / 2 = -3`? Truncation toward zero

**Concepts**: primitive types, literal syntax, OCaml number representation, string syntax
**Keywords**: OCaml, int, float, bool, string, literals, primitive types
**Next**: M02 · L02 · `let` bindings and shadowing