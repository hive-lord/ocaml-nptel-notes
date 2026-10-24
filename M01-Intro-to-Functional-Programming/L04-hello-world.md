# Your first OCaml program: hello, world (and beyond)

# Your First OCaml Program

## A Program Is a Sequence of `let` Bindings

```ocaml
let greeting = "hello, "
let name = "NPTEL"
let () = print_endline (greeting ^ name)
```
Three `let` lines: two value bindings, one print. Order matters - each `let` can refer to names bound above it.

### Comparison with Java
```java
public class Hello {
    public static void main(String[] args) {
        String greeting = "hello, ";
        String name = "NPTEL";
        System.out.println(greeting + name);
    }
}
```

### Key Points
- OCaml program = sequence of `let` bindings
- Top-level `let` = definition visible to rest of file
- `print_endline` prints string + newline
- `^` is string concatenation
- `()` is the `unit` value (like `void` but a real value)

**Concepts**: print_endline, top-level evaluation, let bindings as statements, the unit type
**Keywords**: OCaml, hello world, let binding, print_endline, unit type
**Next**: M01 · L05 · Tutorial: temperature conversions and small expressions