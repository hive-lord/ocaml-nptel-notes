# Course introduction: what you'll learn, how it's run

# Course Introduction

## Course Introduction: What You'll Learn, How It's Run

Welcome to **Functional Programming with OCaml**. This first lecture is short and almost entirely logistical: what we will cover over the twelve weeks, who the course is aimed at, how it is graded, and how you will run OCaml code directly in your browser without installing anything.

### Functional Programming with OCaml
- A 12-week NPTEL course on functional programming, taught through OCaml, with a strong second half on secure systems software.
- Instructor: KC Sivaramakrishnan, IIT Madras (known as "KC")

### The Course Map
12-module course roadmap: Eight FP modules, then four secure-systems modules.

### Why OCaml?
1. **Functional-first**, with a serious type system
2. **Practical**: Used at Jane Street, Bloomberg, Meta (Hack, Flow), Docker, Tarides, Ahrefs, Semgrep. WebAssembly reference interpreter, CompCert verified C compiler, Rust's first compiler was written in OCaml.
3. **Fast**: Native code close to C; incremental/concurrent GC; no UB in the safe fragment
4. **Great teaching language**: Ideas reappear in Rust, Scala, Haskell, Swift, Kotlin, TypeScript
5. **Small**: Core language fits in a long afternoon - handful of expression forms, type constructors, module system

### Run Code Right in This Page
The toplevel embedded in these pages is **OCaml 5.4.0**. Use `opam` to create a `5.4.0` switch locally.

```ocaml
let greeting who = "hello, " ^ who
let () = print_endline (greeting "NPTEL")
```

### How the Course Is Graded
- Assignments released weekly, submitted through SWAYAM portal
- Strict deadlines - NPTEL does not allow late submissions
- Final exam: in-person at NPTEL proctored exam centres across India
- Questions test real understanding, not memorised syntax

### The Structure of Each Week
- Videos are deliberately short (engagement falls off past 30 minutes)
- Watch one a day over the working week
- Chapter pages elaborate on each video as read-after-watching companion

### Reading
- OCaml language home page: https://ocaml.org/
- Cornell CS3110 textbook: https://cs3110.github.io/textbook/
- Real World OCaml: https://dev.realworldocaml.org/prologue.html
- John Whitington, *OCaml from the Very Beginning*

**Concepts**: course logistics, syllabus overview, in-browser dev environment, grading
**Keywords**: OCaml, NPTEL, functional programming, course intro, syllabus, FP
**Next**: M01 · L02 · Why functional programming?