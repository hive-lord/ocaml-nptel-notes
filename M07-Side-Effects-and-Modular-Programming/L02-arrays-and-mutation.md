# Mutable records and arrays

# Mutable records and arrays

## Mutable records and arrays
Beyond `ref`, OCaml has two more mutable building blocks:
- **Mutable record fields**: mark individual fields with `mutable`
- **Arrays**: fixed-size, O(1) indexed, in-place update

## Mutable Record Fields
```ocaml
type counter = { mutable count : int }
let c = { count = 0 }
c.count <- c.count + 1  (* mutation *)
```
`ref` is just the one-field special case.

## Arrays
```ocaml
let a = [| 10; 20; 30; 40; 50 |]
let _ = a.(0)      (* = 10 *)
let _ = a.(2)      (* = 30 *)
let () = a.(2) <- 999
let _ = a          (* = [|10; 20; 999; 40; 50|] *)
```
- Fixed-size, mutable sequence, all same type
- Literal syntax uses bar-brackets `[| ... |]`
- Access: `a.(i)`, update: `a.(i) <- v`

## Iterating Arrays
`Array` module mirrors higher-order functions on lists:
- `Array.iter`: side-effecting walk
- `Array.map`: returns new array
- `fold_left`, `fold_right`, `length`, `to_list`

## OCaml's for and while loops
```ocaml
for i = lo to hi do
  body
done

for i = hi downto lo do
  body
done

while condition do
  body
done
```
Body must be type `unit`. Loop returning other type triggers warning.

## Activity
Write a function to reverse an array in place using two-pointer swap.

**Concepts**: mutable record fields, arrays, in-place update, when to use mutation
**Keywords**: OCaml, mutable, array, record, in-place, mutation
**Next**: M07 · L03 · Exceptions