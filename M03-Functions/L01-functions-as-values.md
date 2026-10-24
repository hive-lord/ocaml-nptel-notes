# Functions as values, and anonymous functions

# Functions as Values

## Functions as Values, and Anonymous Functions
In OCaml, a function is a *value*, the same way `42` and `"hello"` are values. You can name a function with `let`, pass it as an argument, return it from a function, and store it in a data structure. Functions are *first-class values*.

## Functions Are Values
- In C: functions are pointers to code, not really values
- In Python: functions are objects, but defining syntax is heavyweight
- In OCaml: this treatment is *the default*, not a bolt-on

## The Plan for Module 3
- L1 (today): functions as values; anonymous functions
- L2: recursion
- L3: currying and partial application
- L4: tail recursion and accumulators
- L5: local functions and mutual recursion
- L6: Tutorial: Fibonacci, powers of two, fast power, digits
- L7: Practice: recursion, currying, and tail recursion

## Multiple Parameters
All three define the same function:
```ocaml
let add x y = x + y
let add = fun x -> fun y -> x + y
let add = fun x y -> x + y
```
A "two-argument function" in OCaml is really a *one-argument function that returns another one-argument function*. This is **currying** (after Haskell Curry).

## Functions Are Values You Can Name
```ocaml
let make_adder n = fun x -> x + n
let plus_five = make_adder 5
let _ = plus_five 100  (* = 105 *)
```

### Thunks
A *thunk* is a function whose parameter is `()`. Its type is `unit -> 'a`. Body runs each time you call it with `()`.

## A Function Value Remembers Its Environment (Closures)
```ocaml
let make_adder n = fun x -> x + n
let plus_five = make_adder 5
let _ = plus_five 100  (* body is x + n; n=5 captured forever *)
```
OCaml functions are **closures**: they capture values at creation. `plus_five` remembers `n = 5` forever.

### Closures Are Not Magic
OCaml uses *static scoping with value capture*: the binding in scope when `f` was defined is the one `f` uses forever.

## Reading Function Types: `->` Is Right-Associative
```ocaml
int -> int -> int  means  int -> (int -> int)
val add : int -> int -> int
val apply_twice : ('a -> 'a) -> 'a -> 'a
```

## Activity
- What is the type of `fun x -> x +. 1.0`? → `float -> float`
- What does it mean for a function to be a 'first-class value'? Name three things you can do with an int; now name the corresponding three things for a function.

**Concepts**: first-class functions, anonymous functions, fun, lambda, higher-order, function values, closures
**Keywords**: OCaml, functions, first-class, anonymous functions, fun, lambda, higher-order
**Next**: M03 · L02 · Recursion