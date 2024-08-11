# notes/go

worth a skim through to refresh '**_Go_**' in one go.

---

## Basics

### 📝 Slices

- `Slices` are like references to `arrays`, when you create a slice from an existing array - you are essentially interacting wit the underlying array.
- Zero value of a `slice` is `nil`.

### 📝 Zero Values of Data Types

| Data Type | Zero Value |
|---|---|
| bool      | false     |
| string    | ""        |
| int, uint, float32, float64 | 0       |
| complex64, complex128 | 0i      |
| array     | all elements initialized to their zero values |
| struct    | all fields initialized to their zero values |
| pointer   | nil       |
| slice     | nil       |
| map       | nil       |
| channel   | nil       |
| function  | nil       |

### 📝 Control Statements

#### Order of Execution

- `if/else`:
  - The condition is evaluated first. If true, the if block executes; otherwise, the else block (if present) executes.
  - Variables declared inside an `if short` statement will also be available in `else` blocks.
- `for`:
  - The initialization statement runs once, then the condition is checked.
  - If true, the body executes, followed by the post-iteration statement.
  - This repeats until the condition is false.
- `switch`:
  - The expression is evaluated once, and then the cases are checked sequentially until a match is found.
  - Go runs only the first successful case and inserts the `break` automatically, unlike C, C++, Java & JS.
- `defer`:
  - Deferred function calls are pushed onto a stack. When a function returns, its deferred calls are executed in **last-in-first-out** order.
  - Defer calls are evaluated immediately but the function call is not executed until the surrounding function returns.

#### Order of Evaluation

- In Go, function arguments are evaluated before the function is called.
- Go determines the order of evaluation of function arguments by evaluating them from **left to right** before the function call, as specified in the language specification.
- In languages like C, C++, Java there is no definite specification that mentions on the order of evaluation of function arguments.
- In Python, the order of evaluation of function arguments is similar to Go.

## 📝 Pointers

- Unlike C, Go does not have 'pointer arithmetic'.
- As mathematical operations on memory addresses may potentially cause security vulnerabilities, Go intentionally omitted the use of pointer arithmetics.
- Instead those operations can be achieved in Go via in-built functions with 'Slices' etc.
- W.r.t to 'Structs',
  - Go provides the ability to access a struct's fields via its pointer, without an explicit dereference.
  - i.e For a struct 'X' having its pointer 'p', to access the struct using pointer we can use `p.X` instead of `(*p).X`

## 📝 Methods and Interfaces

- In Go, a method is just a function with receiver argument.
- One cannot declare a method with a receiver whose type is defined in another package (including internal packages).
- Interfaces are implemented implicitly in Go. There is no `implements` keyword.
