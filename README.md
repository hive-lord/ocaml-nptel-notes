# Functional Programming with OCaml — NPTEL Course Notes

Comprehensive markdown notes for the **Functional Programming with OCaml** NPTEL course (12 weeks, 11 modules, 72 lectures) by **KC Sivaramakrishnan, IIT Madras**.

Course website: https://fplaunchpad.org/ocaml_nptel/
NPTEL Portal: https://onlinecourses.nptel.ac.in/noc26_cs90/preview

---

## Structure

```
ocaml-nptel-notes/
├── M01-Intro-to-Functional-Programming/
│   ├── L01-course-intro.md
│   ├── L02-why-fp.md
│   ├── L03-ocaml-tour.md
│   ├── L04-hello-world.md
│   ├── M01-revision-notes.md
│   └── M01-quiz.md
├── M02-Expressions/
│   ├── L01-literals.md
│   ├── L02-let-bindings.md
│   ├── L03-types-and-inference.md
│   ├── L04-operators.md
│   ├── M02-revision-notes.md
│   └── M02-quiz.md
├── M03-Functions/
│   ├── L01-functions-as-values.md
│   ├── L02-recursion.md
│   ├── L03-currying.md
│   ├── L04-tail-recursion.md
│   ├── L05-local-and-mutual.md
│   ├── L06-tutorial.md
│   ├── L07-practice.md
│   ├── M03-revision-notes.md
│   └── M03-quiz.md
├── M04-Data-Types/
│   ├── L01-tuples.md
│   ├── L02-records.md
│   ├── L03-variants.md
│   ├── L04-recursive-types.md
│   ├── L05-tutorial.md
│   ├── M04-revision-notes.md
│   └── M04-quiz.md
├── M05-Pattern-Matching/
│   ├── L01-basic-patterns.md
│   ├── L02-recursive-patterns.md
│   ├── L03-nested-and-or-patterns.md
│   ├── L04-guards.md
│   ├── L05-exhaustiveness.md
│   ├── L06-tutorial.md
│   ├── L07-practice.md
│   ├── M05-revision-notes.md
│   └── M05-quiz.md
├── M06-Higher-Order-Programming/
│   ├── L01-functions-revisited.md
│   ├── L02-map.md
│   ├── L03-filter.md
│   ├── L04-fold.md
│   ├── L05-pipelines.md
│   ├── L06-tutorial.md
│   ├── L07-practice.md
│   ├── M06-revision-notes.md
│   └── M06-quiz.md
├── M07-Side-Effects-and-Modular-Programming/
│   ├── L01-references.md
│   ├── L02-arrays-and-mutation.md
│   ├── L03-exceptions.md
│   ├── L04-streams-and-laziness.md
│   ├── L05-memoization.md
│   ├── L06-module-basics.md
│   ├── L07-signatures.md
│   ├── L08-functors.md
│   ├── L09-tutorial.md
│   ├── L10-practice.md
│   ├── M07-revision-notes.md
│   └── M07-quiz.md
├── M08-Monads-and-GADTs/
│   ├── L01-option-monad.md
│   ├── L02-laws-list-result.md
│   ├── L03-state-monad.md
│   ├── L04-gadts-basics.md
│   ├── L05-gadts-use-cases.md
│   ├── L06-hlists-witnesses.md
│   ├── L07-tutorial.md
│   ├── L08-practice.md
│   ├── M08-revision-notes.md
│   └── M08-quiz.md
├── M09-Testing/
│   ├── L01-why-test-typed-code.md
│   ├── L02-specifications-invariants.md
│   ├── L03-test-design.md
│   ├── L04-unit-testing.md
│   ├── L05-property-based-testing.md
│   ├── L06-model-based-testing.md
│   ├── L07-tutorial.md
│   ├── L08-practice.md
│   ├── M09-revision-notes.md
│   └── M09-quiz.md
├── M10-Memory-Safety-and-Security/
│   ├── L01-memory-safety-and-security.md
│   ├── L02-memory-safety-by-construction.md
│   ├── L03-data-races-are-ub.md
│   ├── L04-where-ocaml-has-ub.md
│   ├── L05-tutorial.md
│   ├── M10-revision-notes.md
│   └── M10-quiz.md
├── M11-OxCaml-Type-Level-Extensions-of-Safety/
│   ├── L01-locality.md
│   ├── L02-uniqueness.md
│   ├── L03-linearity.md
│   ├── L04-contention.md
│   ├── L05-portability.md
│   ├── L06-tutorial.md
│   ├── L07-practice.md
│   ├── M11-revision-notes.md
│   └── M11-quiz.md
└── README.md
```

---

## Module Overview

| Module | Title | Lectures | Key Topics |
|--------|-------|----------|------------|
| **M01** | Intro to Functional Programming | 4 | Course logistics, why FP, OCaml tour, hello world |
| **M02** | Expressions | 4 | Literals, `let` bindings, type inference, operators |
| **M03** | Functions | 7 | First-class functions, recursion, currying, tail recursion, local/mutual recursion |
| **M04** | Data Types | 5 | Tuples, records, variants, recursive types, AST tutorial |
| **M05** | Pattern Matching | 7 | Basic patterns, recursive patterns, nested/or-patterns, guards, exhaustiveness, interpreter tutorial |
| **M06** | Higher-Order Programming | 7 | `map`, `filter`, `fold`, pipelines, composition, fold on trees |
| **M07** | Side Effects & Modular Programming | 10 | Refs, arrays, exceptions, streams, memoization, modules, signatures, functors |
| **M08** | Monads and GADTs | 8 | Option/result/list/state monads, monad laws, GADTs, typed interpreters |
| **M09** | Testing | 8 | Specs, unit testing (OUnit2), property-based (QCheck), model-based testing |
| **M10** | Memory Safety and Security | 5 | Memory bugs in C, OCaml safety mechanisms, data races, Heartbleed case study |
| **M11** | OxCaml: Type-Level Extensions | 7 | Locality, uniqueness, linearity, contention, portability, resource API |

**Total: 11 modules, 72 lectures**

---

## Per-Module Contents

Each module folder contains:
- **Lecture notes** (`LXX-topic.md`): Structured notes with code examples, key concepts, and activities
- **Revision notes** (`MXX-revision-notes.md`): Concise summary of key concepts for quick review
- **Quiz** (`MXX-quiz.md`): NPTEL-format questions (MCQs, MSQs, NATs, code analysis)

---

## Quiz Format (NPTEL Style)

Each module quiz includes:
- **MCQs** (Multiple Choice Questions) — single correct answer
- **MSQs** (Multiple Select Questions) — multiple correct answers
- **NATs** (Numerical Answer Type) — numeric answers
- **Code Analysis** — predict output, fix bugs, complete implementations

---

## How to Use

1. **Study order**: Follow module numbers (M01 → M11)
2. **Per lecture**: Read lecture notes → try code in browser toplevel → do activities
3. **Per module**: Complete tutorial/practice → read revision notes → take quiz
4. **Reference**: Use revision notes for quick review before exams

---

## Key OCaml Resources Referenced

- OCaml Manual: https://ocaml.org/manual/
- Cornell CS3110 Textbook: https://cs3110.github.io/textbook/
- Real World OCaml: https://dev.realworldocaml.org/
- OCaml Stdlib API: https://v2.ocaml.org/api/

---

## License

Notes derived from the NPTEL course "Functional Programming with OCaml" by KC Sivaramakrishnan, IIT Madras. Course materials are used for educational purposes. See course page for original licensing.