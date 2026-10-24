# Why functional programming?

# Why Functional Programming?

## Why Functional Programming?

If you can already write programs in C or Python or Java, why spend twelve weeks learning a new style? The answer: it changes how you think about programming, and the new habits transfer back to whatever language you write in tomorrow.

### The Argument in Three Steps
1. **Turing-completeness is a low bar** - The interesting question is not *what* a language can compute but *how easily* you can express what you want
2. **Functional programming thesis** - Programs built around pure functions and immutable data are easier to reason about, test, parallelise, and refactor
3. **Honest accounting** - FP is not a free lunch; OCaml is functional-*first* not functional-only

### The Functional Thesis

**Functions are values** - In OCaml, a function is a value of a particular type, exactly like an integer. You can pass functions to other functions, return them, store them in lists, bind them with `let`.

**Data is immutable** - When you bind a name to a value, that binding does not change. `xs = [1; 2; 3]` does not get modified; to get a longer list, you build a new one. The old list remains untouched.

### Equational Reasoning
- **Refactoring**: Replace any pure function call with its body, or extract a chunk into a helper, without worrying about intermediate state changes
- **Testing**: A pure function depends only on its inputs - unit tests are straightforward
- **Parallelisation**: No shared mutable state means no data races

### Immutability in Practice
```ocaml
let xs = [1; 2; 3]
let ys = 0 :: xs
let _ = xs (* = [1; 2; 3] *)
```
`@` (list append) does not modify `xs` - it builds a new list. OCaml gives you no way to mutate `xs` in place.

### When Functional Doesn't Work (Be Honest)
OCaml is **functional-first**: disciplined escape hatches, used only when they help.

**Concepts**: paradigms, pure functions, immutability, referential transparency, equational reasoning
**Keywords**: OCaml, functional programming, pure functions, immutability, referential transparency, equational reasoning
**Next**: M01 · L03 · A tour of OCaml