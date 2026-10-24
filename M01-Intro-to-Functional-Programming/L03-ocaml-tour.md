# A tour of OCaml: values, types, and the toplevel

# A Tour of OCaml

## This Lecture: A Whirlwind Tour

Every cell on this page is runnable. Click Run on every single cell as you read. Edit them. Try variations.

### The Toplevel
Most OCaml work happens in an editor (files to compile) and a *toplevel* (interactive REPL). The cells on this page are a toplevel.

### Integers
OCaml's `int` is a *machine integer*: 63 bits wide on 64-bit (one bit for GC tagging). Range: ~±4.6 × 10^18.

### Booleans and Comparison
`bool` with `true`/`false`. Comparison operators return `bool`.
- Ordering: `<`, `<=`, `>`, `>=`
- Equality: `=` (structural), `<>` (inequality)
- `=` compares values by content, recursively - works on strings, lists, records

### Let in Expressions
```ocaml
let x = 5 in x + x (* = 10 *)
```

### Strings
```ocaml
let greeting = "hello, " ^ "world" (* "hello, world" *)
```

### What's Next
- L04: Hello world walkthrough
- L05: Module 1 tutorial
- Module 2: Expressions (literals, let bindings, operators, type inference, if/then/else)

**Concepts**: toplevel, integers, booleans, comparison, structural equality, let expressions
**Keywords**: OCaml, toplevel, int, bool, string, structural equality
**Next**: M01 · L04 · Your first OCaml program