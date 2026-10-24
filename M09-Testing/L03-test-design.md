# Designing test cases: black-box and glass-box

# Designing test cases: black-box and glass-box

## You Cannot Test Everything
Take `Rational.add`: two rationals, each made from two `int`s. OCaml `int` has 63 bits. Input space of `add` has `(2^63)^4 = 2^252` points. Checking one addition per nanosecond → ~10^59 years. Universe is ~10^10 years old. Exhaustive testing is not "expensive"; it's not happening.

## Black-box Testing
Derive test cases from the *specification only*, without looking at implementation.
- Input space partitioning
- Boundary cases
- Example: `pad n s`
  - `n <= 0`: return `s` (boundary)
  - `0 < n <= len(s)`: return `s` (boundary)
  - `n > len(s)`: pad with spaces
  - Empty string
  - String with existing leading spaces

## Glass-box (White-box) Testing
Derive test cases from the *implementation* to cover all paths.
- Each `if`/`match` branch is a path
- Goal: a test for every path
- Example: `longest_streak` with nested `if`s → 4 paths through code

## Measuring Coverage: `bisect_ppx`
Glass-box promises "a test for every path" but who keeps score on a 20-module project?
**Code coverage** answers the question. OCaml tool: `bisect_ppx`.
Three moves:
1. **Instrument** code at build time
2. **Record** which expressions ran under tests
3. **Report** each as green (ran) or red (never ran)

## Activity
`max3` compares three ints with nested ifs, giving four paths through the code. Give four inputs `[(x, y, z)]`, one per path. How would you check that your set really is path-complete?

**Concepts**: test-case design, input-space partitioning, boundary cases, black-box testing, glass-box testing, paths, code coverage, bisect_ppx
**Keywords**: OCaml, testing, black-box, glass-box, boundary case, partition, path coverage, code coverage, bisect_ppx
**Next**: M09 · L04 · Unit testing