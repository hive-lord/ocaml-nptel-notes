# Local functions and mutual recursion

# Local Functions and Mutual Recursion

## This Lecture: Local Helpers and Mutual Recursion
Two related topics:
1. **Local function definitions**: helpers inside another function with `let ... in`
2. **Mutual recursion**: two or more functions that call each other, glued with `and`

## Local Helpers: Definitions Inside `let ... in`
```ocaml
let factorial n =
  let rec go acc n =
    if n = 0 then acc
    else go (acc * n) (n - 1)
  in
  go 1 n
```
- `go` defined *inside* `factorial` with `let rec ... in`
- In scope only for the rest of that expression
- Right place for an implementation-detail helper

### Why Hide the Helper?
- Keeps the top-level namespace clean
- The accumulator is an implementation detail
- API is clean: `factorial : int -> int`

## Why `and` Has to Exist
```ocaml
let rec is_even n =
  if n = 0 then true
  else is_odd (n - 1)
and is_odd n =
  if n = 1 then true
  else is_even (n - 1)
```
Without `and`, OCaml rejects: `Unbound value is_odd`. A `let rec` brings the name being defined into scope inside its own body, but not other names not yet defined.

## Mutual Recursion Can Also Be Local
```ocaml
let demo () =
  let rec ping () = print_endline "ping"; pong ()
  and pong () = print_endline "pong"; ping ()
  in ping ()
```
Both `ping` and `pong` are local to `demo`; outside, neither exists.

### Important Property
Every recursive call is in tail position. OCaml's TCO handles tail calls *between* mutually recursive functions, not just self-calls. So `mod3_eq_0 1_000_000` runs in constant stack space.

**Concepts**: local functions, let rec ... in, mutual recursion, and keyword, tail calls across mutual recursion
**Keywords**: OCaml, local functions, mutual recursion, tail recursion, and
**Next**: M03 · L06 · Tutorial: Fibonacci, powers of two, fast power, digits