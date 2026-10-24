# Why test a type-safe program?

# Why test a type-safe program?

## Why test a type-safe program?
Eight modules of OCaml safety: types catch type errors; exhaustive matches force case handling; abstraction enforces interfaces. Common impression: "if it type-checks, it's mostly correct." That's *almost* right, but the gap matters.

This module: what types do NOT catch, and how testing fills the gap. Types and tests are complementary.

## Where We Are
- Eight modules of OCaml safety:
  - types catch type errors
  - exhaustive matches force case handling
  - abstraction enforces interfaces
- Common impression: "if it type-checks, it's mostly correct"
- That impression is *almost* right, but the gap matters
- This module: what types do NOT catch, and how testing fills the gap

## What Types Do NOT Catch
OCaml has tools that close some gap (labelled arguments, records-with-field-names, units of measure via newtype wrappers), but the type system does not, on its own, prevent:
- Wrong output for a known input
- Off-by-one in a loop bound
- Swapped same-typed arguments
- Performance regressions

You catch these the same way C programmers catch off-by-one errors: by writing tests that drive the function on examples whose answers you know.

## Types and Tests, Complementary
| Catches | Types | Tests |
|---------|-------|-------|
| Type errors | yes | (incidentally) |
| Exhaustiveness | yes (warnings) | (incidentally) |
| Parametricity-forced shape | yes | no |
| Wrong output for known input | no | yes |
| Off-by-one in a loop bound | no | yes |
| Swapped same-typed arguments | no | yes |
| Performance regressions | no | yes (benchmarks) |

## Two Kinds of Test, Plus a Side Door
- **Unit tests**: example-based, one input → expected output
- **Property-based tests (PBT)**: write a *property* that should hold for *any* input, plus a way to generate random inputs. Library runs property on hundreds of inputs, reports failures. Library: **QCheck** (OCaml's QuickCheck).
- PBT is particularly natural in OCaml: pure functions and equational reasoning give a clean way to state properties.

## What Types DO Buy You, Even with Tests
- Dynamic-language suites full of malformed-input tests (wrong type, missing field, `None` where a list goes)
- In OCaml the type checker rules those inputs out → those tests simply do not exist
- Tests target *behaviour on well-typed input*
- Fewer tests, each more informative
- RWO: types *raise* the value of every test you write

## What's Next: The Seven Lectures of M9
Deciding what to test:
- L2: **Specifications and invariants**. Contracts, preconditions, rep invariants.
- L3: **Designing test cases**. Black-box, glass-box, coverage.
Mechanizing the checks:
- L4: **Unit testing** with OUnit2.
- L5: **Property-based testing** with QCheck.
- L6: **Model-based testing** of stateful data structures.
- L7: **Tutorial**. The full toolkit on a small arithmetic evaluator.

**Concepts**: testing, type safety, behaviour, validation, complementary roles of types and tests, faults and failures
**Keywords**: OCaml, testing, type safety, behaviour, validation, QCheck, OUnit2
**Next**: M09 · L02 · Specifications and invariants