# Module 10 Revision Notes: Memory Safety and Security

## Key Concepts

### Four UB Categories in C
1. **Memory**: Read/write memory you don't own (most CVEs)
2. **Integer/Arithmetic**: Signed overflow, divide by zero
3. **Aliasing/Concurrency**: Wrong-type reads, data races
4. **Lifetime**: Use-after-free, dangling pointers

### Four Canonical Memory Bugs

#### Use-After-Free (Temporal)
```c
free(ptr);
*ptr = 42;  // UB: reads freed memory
```
- Doesn't crash immediately — reads attacker-controlled data

#### Buffer Overflow (Spatial)
```c
char buf[10];
strcpy(buf, "AAAAAAAAAAAAAAAA");  // Overwrites adjacent memory
```
- Can overwrite return addresses, function pointers

#### Uninitialised Read (Spatial)
```c
char leak[8];
putchar(leak[0]);  // Reads garbage/leftovers
```
- Kernel info-leaks expose secrets

#### Double-Free
```c
free(ptr);
free(ptr);  // Corrupts allocator bookkeeping
```

### How OCaml Rules Them Out

| Bug | OCaml Mechanism |
|-----|-----------------|
| Use-after-free | **No `free`** + **GC** (reclaims only when no refs remain) |
| Buffer overflow | **Bounds checks** on every array/string access |
| Uninitialised read | **Binding-time initialisation** required |
| Double-free | **No `free`** primitive exists |

### OCaml Runtime Mechanisms
- **Tagged pointers**: LSB distinguishes immediates (ints) from pointers
- **Block headers**: Size, tag, GC colour on every heap block
- **Incremental/concurrent GC**: Walks roots, marks reachable, sweeps unreachable

### Data Races
- **C/C++**: Data race = **undefined behaviour** (optimiser assumes no races)
- **OCaml 5**: Data race = **weakly defined but bounded**, still **memory-safe**
  - Racy read returns value genuinely written at some point
  - Damage bounded in **space** (can't corrupt unrelated location)
  - Damage bounded in **time** (can't affect race-free parts of run)
- **DRF-SC**: Data-race-free programs are sequentially consistent (both languages)

### Honest Boundary: Where OCaml Has UB
- `Obj.magic` (unsafe coercion)
- FFI (C interop)
- `Marshal` with wrong type
- Races across domains (without proper synchronisation)

### Heartbleed Case Study
**Bug**: OpenSSL heartbeat handler reads `payload` bytes without checking if packet actually contains that many bytes.
```c
// Attacker sends: payload=65535, actual data=1 byte
memcpy(buffer, packet, payload);  // Reads 64KB of server memory
```

**Fix**: One length check
```c
if (1 + 2 + payload + 16 > record_length) return 0;
```

**OCaml Equivalent**: Structurally impossible
```ocaml
let payload = Bytes.sub record 3 payload_len in
(* Bytes.sub raises Invalid_argument if bounds invalid *)
```
- Bounds check is **mandatory**, part of API, not optional

---

## Module 10 Quiz

### MCQs

**Q1.** Which memory bug is impossible in safe OCaml because there is no `free`?
- A) Buffer overflow
- B) Use-after-free
- C) Uninitialised read
- D) Signed overflow

**Answer: B** — No `free` means no use-after-free or double-free

**Q2.** What does `Bytes.sub s 3 100` do when `s` has length 10?
- A) Returns 100 bytes from position 3
- B) Returns 7 bytes (what's available)
- C) Raises `Invalid_argument`
- D) Returns empty bytes

**Answer: C** — Bounds check is mandatory

**Q3.** In C, a data race is:
- A) Defined behaviour
- B) Undefined behaviour
- C) A compiler error
- D) A runtime exception

**Answer: B** — C/C++ classify data races as UB

**Q4.** In OCaml 5, a data race on an `int` ref:
- A) Causes a segfault
- B) Returns a random bit pattern
- C) Returns some value that was actually written
- D) Is prevented at compile time

**Answer: C** — Weakly defined but bounded, memory-safe

### MSQs

**Q5.** Which are ruled out by construction in safe OCaml? (Select all that apply)
- [ ] A) Use-after-free
- [ ] B) Buffer overflow
- [ ] C) Uninitialised read
- [ ] D) Double-free
- [ ] E) Data races

**Answer: A, B, C, D** — E: data races are bounded but not ruled out in safe fragment

**Q6.** What mechanisms does OCaml use for memory safety? (Select all that apply)
- [ ] A) Garbage collection
- [ ] B) Bounds checking on array/string access
- [ ] C) Mandatory initialisation at binding
- [ ] D) No `free` primitive
- [ ] E) Reference counting

**Answer: A, B, C, D** — E: OCaml uses tracing GC, not ref counting

### NAT

**Q7.** How many canonical memory bug categories did the lecture list?
**Answer: 4**

**Q8.** What year was Heartbleed (CVE-2014-0160) disclosed?
**Answer: 2014**

### Code Analysis

**Q9.** Why is this OCaml code safe from Heartbleed-style bugs?
```ocaml
let handle record =
  let len = get_length record in
  Bytes.sub record 3 len
```
**Answer:** `Bytes.sub` raises `Invalid_argument` if `len` exceeds available bytes — bounds check is part of the API, not optional

**Q10.** What is the "honest boundary" where OCaml's safety guarantees weaken?
**Answer:** `Obj.magic`, FFI, `Marshal` with wrong type, cross-domain races without synchronisation