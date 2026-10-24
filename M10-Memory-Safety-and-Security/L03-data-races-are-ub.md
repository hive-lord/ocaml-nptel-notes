# Data races are undefined behaviour

# Data races are undefined behaviour

## Data races are undefined behaviour
First lecture listed four UB categories; spent two lectures on memory and lifetime. This lecture: the concurrency category, specifically *data races*.

## Framing
- So far: single-threaded code, one step at a time
- Real systems use many cores at once
- What happens when two cores touch the same memory?
  - in C/C++: undefined behaviour
  - in OCaml: weaker than sequential, but still memory-safe
- First job: define a *data race* precisely

## What a Data Race Is
Two threads race when they make two memory accesses (each a read or a write) such that:
- They target the **same location**
- At least **one is a write**
- **No synchronisation** orders them (no join, no lock, no atomic)

## In C and C++, a Data Race Is Undefined Behaviour
The optimiser may assume races do not happen and transform code on that assumption. A race on one variable can make two reads of a completely unrelated variable disagree, with no concurrent write to that variable anywhere in the program.

## OCaml's Position: Defined, and Bounded
OCaml 5's memory model: same definition of data race. Synchronisation from `Domain.spawn`/`Domain.join`, `Mutex`, `Atomic`.

If your program *does* have a race, OCaml still makes a promise C/C++ refuse: the behaviour is *weakly defined but bounded*, and the program stays **memory-safe**.
- A racy read of an `int` returns some value that was actually written to that location at some point, not a random bit pattern
- A racy read of a boxed value returns a valid pointer
- Damage is bounded in space and time

### Sequential consistency, and DRF-SC
- **Sequential consistency (SC)**: all domains' steps interleave into one global order
- **DRF-SC**: a data-race-free program is sequentially consistent (C, C++, OCaml all promise this)
- With a race, the languages part:
  - C/C++: **undefined behaviour**
  - OCaml: **weakly defined but bounded**, still memory-safe

## C UB vs OCaml bounded
| C / C++ | OCaml 5 |
|---------|---------|
| Race-free program | sequentially consistent | sequentially consistent |
| Racy program | **undefined behaviour** | weakly defined but **bounded** |
| Can a race segfault or leak OS memory? | yes | no |
| Can a race on `x` corrupt `y`? | yes | no: bounded in **space** |
| Can a race corrupt the race-free rest of the run? | yes | no: bounded in **time** |

## Activity
C and C++ classify any data race as UB. OCaml keeps a racy program memory-safe: reads return values genuinely written, never wild pointers or OS secrets, damage bounded in space and time.

**Concepts**: data race, undefined behaviour, domains, parallelism, memory model, happens-before, DRF-SC, local DRF, Atomic, Mutex, memory safety
**Keywords**: OCaml, data race, undefined behaviour, domains, parallelism, memory model, happens-before, DRF-SC, local DRF, Atomic, Mutex
**Next**: M10 · L04 · Where OCaml itself has UB