# Where OCaml itself has UB

# Where OCaml itself has UB

## Where OCaml itself has UB
The earlier lectures made the safe story precise: in the *safe fragment*, the four canonical memory bugs are ruled out by construction, and even data races stay memory-safe. This lecture is the honest boundary, in two parts.

(Content from this lecture was brief - mainly a transition to the tutorial)

## What's next
The tutorial closes the module by walking one famous CVE, Heartbleed, end to end: the bug in OpenSSL, the exploit, the one-line fix, and the OCaml equivalent where the same bug class is structurally impossible.

**Keywords**: OCaml, undefined behaviour, UB, escape hatches, Obj.magic, FFI, Marshal
**Next**: M10 · L05 · Tutorial: walking Heartbleed end to end