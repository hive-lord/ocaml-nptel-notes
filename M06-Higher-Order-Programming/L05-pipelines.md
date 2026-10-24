# Function composition and pipelines

# Function Composition and Pipelines

## Function Composition and Pipelines
We have `map`, `filter`, `fold`, and `fun x -> ...`. How to *chain* them? Nested calls read inside-out; logic flows wrong way.

Two pieces of plumbing:
1. The pipeline operator `|>`
2. Function composition

## The Pipeline Operator `|>`
```ocaml
let ( |> ) x f = f x
```
That's the entire definition. `x |> f` is exactly `f x`.

```ocaml
let _ =
  [1; 2; 3; 4; 5]
  |> List.map (fun x -> x * x)
  |> List.filter (fun y -> y > 5)
  |> List.fold_left (+) 0  (* = 50 *)
```
Write chains in the order they happen, instead of inside-out.

## Function Composition
```ocaml
let compose f g = fun x -> f (g x)
let square_then_inc = compose (fun x -> x + 1) (fun x -> x * x)
let _ = square_then_inc 4  (* = 17 *)
```
Packages two functions into one without naming an intermediate argument.

## `@@` Operator
```ocaml
let ( @@ ) f x = f x
```
Reverse of `|>`; useful for `f @@ g @@ h x`.

## Putting It Together
```ocaml
let average_adult_age people =
  people
  |> List.filter (fun p -> p.age >= 18)
  |> List.map (fun p -> float_of_int p.age)
  |> (fun ages ->
       if ages = [] then None
       else Some (List.fold_left (+.) 0. ages /. float_of_int (List.length ages)))
```
Pipeline pulls out ages of adults in a clean chain; final aggregation needs length so lives outside.

**Concepts**: function composition, pipeline operator, point-free style, readability
**Keywords**: OCaml, function composition, pipeline, |>, @@, point-free
**Next**: M06 · L06 · Tutorial: fold across data structures