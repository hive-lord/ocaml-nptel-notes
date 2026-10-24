# What memory safety is, and why it is a security story

# What memory safety is, and why it is a security story

## What memory safety is, and why it is a security story
Nine modules of OCaml have built up an intuition that, when your program type-checks, certain whole classes of bugs simply cannot happen. No segfaults, no use-after-free, no buffer overflows. Not because we were careful; because the language rules them out by construction.

This module asks: what exactly are those classes of bugs, and what are they worth? To answer, look at the canonical unsafe language: C.

## Where We Are
- Eight modules of OCaml: types catch type errors; matching forces case-handling; GC removes lifetime questions
- Now ask the two questions that leaves open:
  - what exactly does OCaml rule out?
  - what is it worth to rule those out?
- To answer, look at the canonical unsafe language, **C**

## What This Module Covers (5 lectures)
- **L1** (this one): what memory safety is, and why it is a security story
- **L2**: how OCaml rules the bugs out by construction (GC, bounds checks, value representation)
- **L3**: data races are undefined behaviour
- **L4**: where OCaml itself has UB, and resource safety beyond memory
- **L5**: tutorial. Walking Heartbleed end to end

## Four UB Categories
1. **Memory**: Read or write memory you do not own. *Most CVEs.*
2. **Integer and arithmetic**: Signed overflow, divide by zero
3. **Aliasing and concurrency**: Wrong-type reads; data races
4. **Lifetime**: Use-after-free, dangling pointers

## The Four Canonical Memory Bugs

### Use-after-free (temporal)
```c
// fill heap-allocated record, free it, then read it again through the old pointer
```
Does not crash immediately - reads whatever was written into the freed slot.

### Buffer overflow (spatial)
```c
char buf[10];
strcpy(buf, "AAAAAAAAAAAAAAAAAAAAAAAAAAAA");  // off the end
```
Overwrites adjacent memory (canary, return address, function pointers).

### Uninitialised read (spatial)
```c
char leak[8];  // storage allocated, contents unspecified
putchar(leak[0]);  // BUG: read before any write
```
Leftover bytes are whatever the last caller left there - kernel info-leaks expose secrets.

### Double-free
```c
char *buf = malloc(64);
free(buf);
free(buf);  // UB: the block is already on the free list
```
Corrupts the allocator's bookkeeping. No `free` in OCaml, so whole family closes together.

## How OCaml Stands, and Safety by Design
Every bug in this lecture is impossible in safe OCaml, by construction:
- No `free` → use-after-free and double-free cannot be written
- Indexing `string`, `bytes`, `array` is bounds-checked → raises `Invalid_argument`
- Every binding is initialised at point of binding → no uninitialised read

This is the security-flavoured version of the "why functional programming" argument: pure functions and immutable data eliminate whole categories of bugs not by being careful but by removing the means to write them.

## Activity
Why does the use-after-free exploit chain work? free → dangling pointer → heap spray → type confusion → ROP → payload.

**Keywords**: OCaml, undefined behaviour, UB, memory safety, CVE, security, C, Heartbleed, ASLR, ROP, White House, CISA
**Next**: M10 · L02 · Memory safety by construction