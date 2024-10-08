# notes/go

worth a skim through to refresh '**_Go_**' in one go.

---

## Basics

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

### 📝 Pointers

- Unlike C, Go does not have 'pointer arithmetic'.
- As mathematical operations on memory addresses may potentially cause security vulnerabilities, Go intentionally omitted the use of pointer arithmetics.
- Instead those operations can be achieved in Go via in-built functions with 'Slices' etc.
- W.r.t to 'Structs',
  - Go provides the ability to access a struct's fields via its pointer, without an explicit dereference.
  - i.e For a struct 'X' having its pointer 'p', to access the struct using pointer we can use `p.X` instead of `(*p).X`
- Go copies values when you pass them to functions/methods, so if you're writing a function that needs to mutate state you'll need it to take a pointer to the thing you want to change.
- When a function returns a pointer to something, you need to make sure you check if it's nil or you might raise a runtime exception - the compiler won't help you here.
- While pointers can point to the same memory location, the pointers themselves are stored in different locations.



### 📝 Slices

- `Slices` are like references to `arrays`, when you create a slice from an existing array - you are essentially interacting wit the underlying array.
- Zero value of a `slice` is `nil`.
- When attempted to change a slice's capacity with a higher value, we get `slice bounds of range` error.

### 📝 Maps

- A Map will not throw an error if the value already exists. Instead, they will go ahead and overwrite the value with the newly provided value. 
- A `nil` map is essentially an empty map with a crucial limitation: you cannot add or modify any key-value pairs to it.
- Attempting to do so will result in a `panic` with `runtime error`.
- Map literals require `key-value pairs`, unlike 'Struct Literals'.
- When you pass a map to a function/method, you are indeed copying it, but just the pointer part, not the underlying data structure that contains the data.
- When iterating over a map with a range loop, the iteration order is not specified and is not guaranteed to be the same from one iteration to the next. 
- If you require a stable iteration order you must maintain a separate data structure that specifies that order.
- Maps are not safe for concurrent use:
  - If you need to read from and write to a map from concurrently executing goroutines, the accesses must be mediated by some kind of synchronization mechanism. 
  - One common way to protect maps is with `sync.RWMutex`.

### 📝 Struct Literals

- A struct literal denotes a newly allocated struct value by listing the values of its fields.
- The order of fields in a struct literal doesn't matter.
- You can omit fields in a struct literal, and their values will be the zero value for their respective types.
- You can create a pointer to a struct literal using the `&` operator.

## Methods and Interfaces

### 📝 Methods

- In Go, a method is just a function with receiver argument.
- One cannot declare a method with a receiver whose type is defined in another package (including internal packages).

### 📝 Interfaces

- Interfaces are implemented implicitly in Go. There is no `implements` keyword.

## Errors, Panic & Defer

### 📝 Errors
- Errors are the way to signify failure when calling a function/method.
- More on errors at: [Don’t just check errors, handle them gracefully](https://dave.cheney.net/2016/04/27/dont-just-check-errors-handle-them-gracefully)

### 📝 Defer

- A deferred function’s arguments are evaluated when the defer statement is evaluated.
- Deferred function calls are executed in Last In First Out order after the surrounding function returns.
- Deferred functions may read and assign to the returning function’s named return values.
- Defer calls are evaluated immediately but the function call is not executed until the surrounding function returns.

### 📝 Panic

- Panic is a built-in function that stops the ordinary flow of control and begins panicking.
- Panics can be initiated by invoking panic directly.
- They can also be caused by runtime errors, such as out-of-bounds array accesses.

### 📝 Recover

- Recover is a built-in function that regains control of a panicking goroutine.
- Recover is only useful inside deferred functions. During normal execution, a call to recover will return nil and have no other effect.
- If the current goroutine is panicking, a call to recover will capture the value given to panic and resume normal execution.