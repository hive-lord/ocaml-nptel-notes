# Streams and laziness

# Streams and laziness

## Streams and laziness
We've built *finite* data: lists with `n` elements, trees with so many nodes. But many problems are naturally described by *infinite* sequences: primes, Fibonacci numbers, input lines from a socket.

We don't want all at once (can't fit in memory); just want the first few, on demand.

## Thunks
A *thunk* is a function `unit -> 'a`. Body runs when called with `()`.
```ocaml
let thunk = fun () -> expensive_computation ()
```

## Lazy Values (`Lazy.t`)
```ocaml
let lazy_val = lazy (expensive_computation ())
let _ = Lazy.force lazy_val  (* computes on first force *)
```
Memoised: subsequent forces return cached result.

## Streams
```ocaml
type 'a stream = Cons of 'a * (unit -> 'a stream)
```
A stream is a head value + a thunk for the rest.

### Sieve of Eratosthenes
```ocaml
let rec sieve = function
  | Cons (p, rest) ->
      Cons (p, fun () -> sieve (filter (fun x -> x mod p <> 0) (rest ())))
```

### Fibonacci Stream
```ocaml
let fib_stream =
  let rec go a b = Cons (a, fun () -> go b (a + b))
  in go 0 1
```

**Concepts**: recursive values, infinite data, thunks, streams, lazy values, lazy streams, sieve of Eratosthenes, lazy Fibonacci
**Keywords**: OCaml, stream, thunk, lazy, Lazy.force, lazy_t, sieve, Fibonacci
**Next**: M07 · L05 · Memoization