# Memory safety by construction

# Memory safety by construction

## Memory safety by construction
Previous lecture catalogued the four canonical memory-safety bugs in C. This lecture makes the OCaml side precise: for each bug, which language feature rules it out, where in the runtime the rule is enforced, and what the cost is.

## Framing
- Not "trust us, it is safe": here is the *mechanism*
- One section per bug: which feature closes it, and how
- Then the runtime sketch: tagged pointers, block headers, the GC
- Small enough for one lecture, and runnable in your browser

## Use-after-free: Ruled Out by the GC
Two things rule it out, working together:
1. **No `free` primitive**: OCaml's surface language has no `free`, `delete`, `dispose`. The programmer never tells the runtime "this block is now garbage."
2. **Garbage collection**: Memory reclaimed automatically, but only after every reference to the block has gone out of scope. GC walks the roots (stack, registers, globals) and follows every pointer; any block unreachable after that walk is dead. By the time GC reclaims a block, no part of the program holds a pointer to it.

"OCaml has no use-after-free" = the specific UB pattern of UAF is eliminated.

## Buffer Overflow: Ruled Out by Bounds Checks
Every array/string/bytes access is bounds-checked:
```ocaml
let a = [| 1; 2; 3 |]
a.(5)  (* raises Invalid_argument "index out of bounds" *)
```
The check is mandatory, not optional, not a performance opt-in, not something a programmer can forget.

## Uninitialised Read: Ruled Out by Binding-Time Initialisation
```ocaml
let x = 5  (* must initialise at binding *)
```
In OCaml, every `let` binding requires an initialiser. No declaration without definition.

## Double-free: Ruled Out Trivially
OCaml has no `free`. There is no `free` to call once, let alone twice. The bug class is closed because the operation does not exist. The GC owns deallocation; the programmer cannot interfere.

## The Runtime Sketch
### Tagged Pointers
- 64-bit machine: `int` is 63 bits, 1 bit tags it as immediate (not a pointer)
- Pointers have LSB = 0; immediates have LSB = 1
- GC uses this to distinguish pointers from integers

### Block Headers
Every heap block has a header: size, tag (for variant constructor), GC colour

### The GC
- Incremental, concurrent in recent versions
- Walks roots, marks reachable, sweeps unreachable
- By the time a block is reclaimed, no pointer to it exists

## Honest Boundary
This story holds for the *safe fragment*. Outside it (`Obj.magic`, FFI, `Marshal` with wrong type, races across domains) guarantees weaken.

**Concepts**: garbage collection, bounds checking, initialisation, tagged pointers, block headers, runtime representation
**Keywords**: OCaml, GC, bounds check, initialisation, tagged pointers, block headers, runtime
**Next**: M10 · L03 · Data races are undefined behaviour