# Module 1 Revision Notes: Intro to Functional Programming

## Key Concepts

### Why OCaml?
1. **Functional-first** with serious type system
2. **Practical**: Jane Street, Bloomberg, Meta, Docker, Tarides, WebAssembly ref interpreter, CompCert, Rust's first compiler
3. **Fast**: Native code close to C; incremental/concurrent GC; no UB in safe fragment
4. **Great teaching language**: Ideas reappear in Rust, Scala, Haskell, Swift, Kotlin, TypeScript
5. **Small**: Core language fits in a long afternoon

### Course Structure
- 12 modules: 8 FP modules + 4 secure-systems modules
- Short videos (engagement falls off past 30 min)
- Chapter pages = read-after-watching companions
- Grading: Weekly assignments on SWAYAM, strict deadlines, in-person proctored final exam

### The Toplevel
- Interactive REPL for experimentation
- Embedded in browser via x-ocaml (OCaml 5.4.0)
- Click Run on every cell as you read

### Basic Syntax
```ocaml
let x = 5          (* top-level definition *)
let y = x + 1      (* can refer to previous bindings *)
let () = print_endline "hello"  (* unit-returning expression *)
```

### Key Takeaways
- OCaml program = sequence of `let` bindings
- Order matters (later bindings see earlier ones)
- `print_endline` for output
- `^` for string concatenation
- `()` = `unit` value (like void but a real value)

---

## Module 1 Quiz (NPTEL Format)

### Multiple Choice Questions (MCQs)

**Q1.** What is the type of the expression `print_endline "hello"`?
- A) `string`
- B) `int`
- C) `unit`
- D) `bool`

**Answer: C** — `print_endline` returns `unit` (written `()`)

**Q2.** In OCaml, what does the `^` operator do?
- A) Integer addition
- B) Float addition
- C) String concatenation
- D) List concatenation

**Answer: C** — `^` concatenates strings; `@` concatenates lists

**Q3.** Which of the following is true about OCaml's `let` bindings at the top level?
- A) They are mutable by default
- B) Later bindings cannot refer to earlier ones
- C) Each `let` introduces a new binding visible to the rest of the file
- D) They must be terminated with semicolons

**Answer: C** — Top-level `let` = definition visible to rest of file

**Q4.** What is the OCaml equivalent of `void` in C/Java?
- A) `null`
- B) `none`
- C) `unit` (written `()`)
- D) `empty`

**Answer: C** — `unit` has exactly one value: `()`

### Multiple Select Questions (MSQs)

**Q5.** Which of the following are reasons given for choosing OCaml as the course language? (Select all that apply)
- [ ] A) Functional-first with serious type system
- [ ] B) Used in production at major companies
- [ ] C) Native code performance close to C
- [ ] D) Largest standard library of any language
- [ ] E) Small core language that fits in an afternoon

**Answer: A, B, C, E** — D is false; OCaml's stdlib is modest

**Q6.** Which statements about the course structure are correct? (Select all that apply)
- [ ] A) 12 modules total
- [ ] B) 8 FP modules followed by 4 secure-systems modules
- [ ] C) Assignments submitted through SWAYAM portal
- [ ] D) Late submissions accepted with penalty
- [ ] E) Final exam is online and open-book

**Answer: A, B, C** — D: NPTEL does not allow late submissions; E: Final exam is in-person proctored

### Numerical Answer Type (NAT)

**Q7.** How many modules are in the FP (functional programming) half of the course?
**Answer: 8**

**Q8.** What version of OCaml is used in the in-browser toplevel?
**Answer: 5.4.0**

### Conceptual / Code Analysis

**Q9.** What does the following code print?
```ocaml
let x = 10
let x = x + 5
let () = print_int x
```
**Answer: 15** — Shadowing: second `let x` creates new binding referring to first `x`

**Q10.** Which of the following is the correct way to write a sequence of expressions where the first is evaluated for side effect?
- A) `e1; e2`
- B) `e1, e2`
- C) `e1 then e2`
- D) `e1 and e2`

**Answer: A** — `;` sequences expressions, discarding the first's value (which should be `unit`)