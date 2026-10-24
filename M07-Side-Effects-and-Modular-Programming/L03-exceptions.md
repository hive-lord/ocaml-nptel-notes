# Exceptions

# Exceptions

## Exceptions
The third member of the imperative trio (alongside refs and arrays). Exceptions let you signal "something went wrong" without threading an `option` or `result` through every layer.

## Declaration and Raising
```ocaml
exception MyError of string

let fail msg = raise (MyError msg)

let safe_div x y =
  if y = 0 then raise (Division_by_zero)
  else x / y
```

## Handling with `try ... with`
```ocaml
let handle_div x y =
  try Some (x / y)
  with Division_by_zero -> None
```

## Built-in Exceptions
- `Not_found` - raised by `List.assoc`, `Map.find`, etc.
- `Invalid_argument` - bounds checks, e.g., `String.get`
- `Failure msg` - generic failure (use `failwith msg`)
- `Division_by_zero` - integer division by zero
- `Match_failure` - non-exhaustive pattern match
- `Assert_failure` - failed `assert`

## When to Throw vs Return `option`
- **Exceptions**: unexpected failures, "should not happen" in correct programs
- **`option`/`result`**: predictable, expected outcomes (e.g., key not found)

**Concepts**: raise, try-with, exception declarations, when to throw vs return option
**Keywords**: OCaml, exception, raise, try, with, Failure, Not_found
**Next**: M07 · L04 · Streams and laziness