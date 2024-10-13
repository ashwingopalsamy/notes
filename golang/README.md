# notes/go

worth a skim through to refresh '**_Go_**' in one go.

---

## Table of Contents
- [Go Data Types](#go-data-types)
  - [1. Basic (Primitive) Types](#1-basic-primitive-types)
    - [a. Numeric Types](#a-numeric-types)
    - [b. Boolean Type](#b-boolean-type)
    - [c. String Type](#c-string-type)
  - [2. Composite (Complex) Types](#2-composite-complex-types)
    - [a. Arrays](#a-arrays)
    - [b. Slices](#b-slices)
    - [c. Structs](#c-structs)
    - [d. Maps](#d-maps)
    - [e. Pointers](#e-pointers)
    - [f. Functions](#f-functions)
    - [g. Channels](#g-channels)
  - [3. Interface Types](#3-interface-types)
  - [4. Special Types](#4-special-types)
    - [a. Nil](#a-nil)
    - [b. Constants](#b-constants)
  - [5. User-Defined Types](#5-user-defined-types)
    - [a. Type Alias](#a-type-alias)
    - [b. Custom Types](#b-custom-types)
  - [Zero Values of Data Types](#zero-values-of-data-types)
- [Control Statements](#control-statements)
  - [Order of Execution](#order-of-execution)
  - [Order of Evaluation](#order-of-evaluation)
- [Composite Data Types](#composite-data-types)
  - [Pointers](#pointers)
  - [Slices](#slices)
  - [Variadic Arguments](#variadic-arguments)
  - [Maps](#maps)
  - [Struct Literals](#struct-literals)
- [Methods and Interfaces](#methods-and-interfaces)
  - [Methods](#methods)
  - [Interfaces](#interfaces)
- [Errors, Panic \& Defer](#errors-panic--defer)
  - [Errors](#errors)
  - [Defer](#defer)
  - [Panic](#panic)
- [Types, Type Assertion, Type Switches](#types-type-assertions-type-switches)
  - [Type Assertions](#type-assertions)

---

## Go Data Types

### 1. Basic (Primitive) Types
These are Go's fundamental types.

#### a. Numeric Types
- **Integer Types**: Store whole numbers.
  - **Signed Integers**: Hold positive and negative values.
    - `int`, `int8`, `int16`, `int32`, `int64`
  - **Unsigned Integers**: Hold non-negative values.
    - `uint`, `uint8`, `uint16`, `uint32`, `uint64`, `uintptr`
  
- **Floating-point Types**: Store numbers with decimals.
  - `float32`, `float64`
  
- **Complex Numbers**: Include real and imaginary parts.
  - `complex64`, `complex128`
  
- **Other Numeric Types**:
  - `byte` (alias for `uint8`)
  - `rune` (alias for `int32`, represents Unicode code points)

#### b. Boolean Type
- `bool`: Represents `true` or `false`.

#### c. String Type
- `string`: Sequence of UTF-8 encoded text.

---

### 2. Composite (Complex) Types
These types are built using other types.

#### a. Arrays
- Fixed-size collection of elements of the same type.
  ```go
  var arr [5]int
  ```

#### b. Slices
- Dynamically-sized arrays.
  ```go
  var s []int = []int{1, 2, 3}
  ```

#### c. Structs
- Group related data with different types.
  ```go
  type Person struct {
      Name string
      Age  int
  }
  ```

#### d. Maps
- Collection of key-value pairs with unique keys.
  ```go
  var m map[string]int = make(map[string]int)
  ```

#### e. Pointers
- Store memory addresses of variables.
  ```go
  var p *int = &x
  ```

#### f. Functions
- Functions can be passed as variables.
  ```go
  var f func(int) int
  ```

#### g. Channels
- Facilitate communication between goroutines.
  ```go
  ch := make(chan int)
  ```

---

### 3. Interface Types
Interfaces define method signatures a type must implement, supporting polymorphism.
```go
type Animal interface {
    Speak() string
}
```

---

### 4. Special Types

#### a. Nil
- Zero value for pointers, interfaces, maps, slices, channels, and function types.
  ```go
  var p *int = nil
  ```

#### b. Constants
- Declared with `const`, can be basic types like `int`, `float`, `string`, or `bool`.
  ```go
  const Pi = 3.14
  ```

---

### 5. User-Defined Types

#### a. Type Alias
- New names for existing types.
  ```go
  type Age int
  ```

#### b. Custom Types
- Create types that behave differently from their underlying type.
  ```go
  type Kilometers float64
  ```

---

### Zero Values of Data Types

| Data Type                   | Zero Value                                    |
| --------------------------- | --------------------------------------------- |
| bool                        | false                                         |
| string                      | ""                                            |
| int, uint, float32, float64 | 0                                             |
| complex64, complex128       | 0i                                            |
| array                       | all elements initialized to their zero values |
| struct                      | all fields initialized to their zero values   |
| pointer                     | nil                                           |
| slice                       | nil                                           |
| map                         | nil                                           |
| channel                     | nil                                           |
| function                    | nil                                           |

- **Definition**: Zero values are the default values assigned to variables of a type when no explicit value is given.
- **Use Cases**: Useful for initializing variables without needing to set them to a specific value.
- **Tips**: Always be aware of zero values when working with types to prevent unexpected behavior in your programs.
- **Lesser-known facts**: Structs and arrays have zero values where each field/element is set to its respective zero value, while slices, maps, channels and pointers have a zero value of `nil`.

---

## Control Statements

### Order of Execution

- `if/else`:
  - The condition is evaluated first. If true, the if block executes; otherwise, the else block (if present) executes.
  - Variables declared inside an `if short` statement will also be available in `else` blocks.
- `for`:
  - The initialization statement runs once, then the condition is checked.
  - If true, the body executes, followed by the post-iteration statement.
  - This repeats until the condition is false.
- `switch`:
  - The expression is evaluated once and then the cases are checked sequentially until a match is found.
  - Go runs only the first successful case and inserts the `break` automatically, unlike C, C++, Java & JS.
  - Note: A switch statement does not fall through to subsequent cases unless `fallthrough` is explicitly used.
- `defer`:
  - Deferred function calls are pushed onto a stack. When a function returns, its deferred calls are executed in **last-in-first-out** order.
  - Defer calls are evaluated immediately but the function call is not executed until the surrounding function returns.

- **Definition**: Control statements direct the flow of execution in a program.
- **Use Cases**: Managing conditional logic, loops, and deferred execution.
- **Tips**: Use `defer` for resource cleanup and to ensure execution order in a function.
- **Lesser-known facts**: In `switch` statements, if no case matches, the default case (if present) will execute.

### Order of Evaluation

- In Go, function arguments are evaluated before the function is called.
- Go determines the order of evaluation of function arguments by evaluating them from **left to right** before the function call, as specified in the language specification.
- In languages like C, C++, Java there is no definite specification that mentions the order of evaluation of function arguments.
- In Python, the order of evaluation of function arguments is similar to Go.

- **Definition**: The order in which expressions and function arguments are evaluated before the execution of the function.
- **Use Cases**: Important for understanding side effects and dependencies in function calls.
- **Tips**: Be mindful of the evaluation order to avoid unexpected results, especially with mutable types.
- **Lesser-known facts**: The evaluation order is consistent across Go functions, reducing ambiguity compared to other languages.

---

## Composite Data Types

### Pointers

- Unlike C, Go does not have 'pointer arithmetic'.
- As mathematical operations on memory addresses may potentially cause security vulnerabilities, Go intentionally omitted the use of pointer arithmetics.
- Instead those operations can be achieved in Go via in-built functions with 'Slices' etc.
- W.r.t to 'Structs',
  - Go provides the ability to access a struct's fields via its pointer, without an explicit dereference.
  - i.e For a struct 'X' having its pointer 'p', to access the struct using pointer we can use `p.X` instead of `(*p).X`
- Go copies values when you pass them to functions/methods, so if you're writing a function that needs to mutate state you'll need it to take a pointer to the thing you want to change.
- When a function returns a pointer to something, you need to make sure you check if it's nil or you might raise a runtime exception - the compiler won't help you here.
- While pointers can point to the same memory location, the pointers themselves are stored in different locations.

- **Definition**: A pointer is a variable that holds the memory address of another variable.
- **Use Cases**: Used to reference large structs without copying them and to modify values directly.
- **Tips**: Always check for nil pointers to avoid runtime panics when dereferencing.
- **Lesser-known facts**: Pointers to structs allow access to fields without explicit dereferencing, simplifying syntax.

---

### Slices

- Internally, slices are described as a **struct** containing:
  - A pointer to the array that holds the elements.
  - The length of the slice.
  - The capacity of the slice (maximum number of elements it can hold without reallocating).
- Slices are like references to arrays. When you create a slice from an existing array, you are interacting with the underlying array.
- Zero value of a slice is `nil`.
- Slices have a length and capacity. When you attempt to change a slice's capacity beyond its limit, you get a `slice bounds out of range` error.
- Slices can grow as needed by using the `append` function, which may result in a new array being allocated.
- When you append to a slice beyond its capacity, Go automatically reallocates the underlying array and adjusts the slice's pointer.
- **Definition**: Slices are dynamically-sized, flexible views into the elements of an array.
- **Use Cases**: Ideal for handling collections of items where the size may change.
- **Tips**: Use the built-in `append` function to safely add elements to slices.
- **Lesser-known facts**: Slices can share the same underlying array, allowing for efficient memory usage and modifications.

---

### Variadic Arguments

- A variadic function accepts a variable number of arguments of a specified type.
- Syntax: `func example(nums ...int)` allows the function to receive zero or more integers as arguments.
  
- The variadic parameter is internally represented as a slice of the specified type.
  - You can pass a slice to a variadic function using the `...` symbol: `example(slice...)`.

- **Definition**: A mechanism in Go to pass a variable number of arguments to a function.
- **Use Cases**: Useful for functions like `fmt.Println`,`append` where the number of arguments isn't fixed, and when building flexible APIs.
- **Tips**: For performance-critical code, consider using regular slices instead of variadic arguments, as each function call involves the creation of a new slice.
- **Lesser-known facts**: The variadic parameter must be the last in the function signature. If no arguments are passed, the slice is nil or empty, avoiding the need for explicit checks.

---

### Maps


- A Map will not throw an error if the value already exists. Instead, it overwrites the value with the newly provided value.
- A `nil` map is essentially an empty map with a crucial limitation: you cannot add or modify any key-value pairs to it.
- Attempting to do so will result in a `panic` with `runtime error`.
- Map literals require `key-value pairs`, unlike 'Struct Literals'.
- When you pass a map to a function/method, you are copying just the pointer to the underlying data structure(`runtime.hmap`), not the data itself. This allows modifications to the map within the function to affect the original map.
- **Iteration Order**: 
  - A key difference between slices and maps is that **maps are not ordered**. When you iterate over a map using a `range` loop, the order of the keys is random and changes from one iteration to the next.
  - If you require a consistent iteration order, you need to maintain a separate data structure to manage the order (e.g., a sorted slice of keys).
- When looking up keys, Go returns two values: the value associated with the key and a boolean `ok` indicating if the key exists in the map.
  - **Use Cases:**
     - Key Doesn’t Exist: If the key is not present in the map, `ok` will be false, and the returned value will be the zero value for that type (e.g., an empty string).
     - Key Exists But No Definition: If the key exists but has no meaningful value (e.g., `dictionary["existingWord"]` returns ""), `ok` will be true, indicating the key was found, but the value is empty.


- **Definition**: Maps are unordered collections of key-value pairs, where each key is unique.
- **Use Cases**: Efficiently store and retrieve data based on keys, such as counting occurrences or grouping items.
- **Tips**: Always initialize maps before use, as `nil` maps cannot be modified.
- **Lesser-known facts**: 
	- The iteration order over maps is randomized; thus, a consistent order must be managed externally if required.
	- Maps are not safe for concurrent use:
		- If you need to read from and write to a map from concurrently executing goroutines, the accesses must be mediated by some kind of synchronization mechanism. 
		- One common way to protect maps is with `sync.RWMutex`.

---

### Struct Literals

- Go allows initializing structs using struct literals.
- Struct literals can be defined in two ways:
  - **Positional**: Assigning values in the order of struct fields.
    ```go
    p := Person{"Alice", 30}
    ```
  - **Named**: Assigning values by field names.
    ```go
    p := Person{Name: "Alice", Age: 30}
    ```
- Named fields allow flexibility in initialization order and improve code readability.
- **Definition**: A way to create and initialize structs at the same time.
- **Use Cases**: Creating instances of structs easily and clearly.
- **Tips**: Prefer named literals for clarity, especially with large structs.
- **Lesser-known facts**: The order of fields in a struct literal can be changed when using named fields.

---

## Methods and Interfaces

### Methods

- In Go, methods are functions with a special receiver argument.
- The receiver can be a value or a pointer type.
- One cannot declare a method with a receiver whose type is defined in another package (including internal packages).
- Methods allow you to define behaviors for types.
- **Definition**:  A method is a function that is associated with a specific type, allowing it to operate on the data contained in that type.
- **Use Cases**: Encapsulation of behavior related to data structures.
- **Tips**: Use pointer receivers for large structs to avoid copying.
- **Lesser-known facts**: 
  - Methods can be defined for any type, including built-in types.
  - Go does not support method overloading based on parameter types.

### Interfaces

- Interfaces define a set of method signatures.
- A type implements an interface by implementing its methods.
- Go does not require explicit declaration of intent to implement an interface.
- **Definition**: An interface is a type that specifies a contract of methods without implementing them, allowing different types to satisfy that interface.
- **Use Cases**: Enable polymorphism and allow functions to operate on different types through a common interface.
- **Tips**: Use interfaces to define common behavior without tightly coupling types.
- **Lesser-known facts**: An empty interface (`interface{}`) can hold values of any type, making it a powerful but potentially risky tool for type assertions.

---

## Errors, Panic & Defer

### Errors

- Go uses the `error` type to represent errors.
- Functions can return an error value along with the result.
- The idiomatic way to handle errors in Go is to check the error value immediately after calling a function that returns an error.
- More on errors at: [Don’t just check errors, handle them gracefully](https://dave.cheney.net/2016/04/27/dont-just-check-errors-handle-them-gracefully)
- **Definition**: An error in Go is represented by the `error` type, used to signal an issue that occurred during a function's execution.
- **Use Cases**: Returning errors allows functions to communicate failure and handle it gracefully.
- **Tips**: Always check for errors after a function call that can return one.
- **Lesser-known facts**: Custom error types can provide additional context beyond the basic error string. You can create custom error types by implementing the `Error()` method on a struct.

### Defer

- A deferred function’s arguments are evaluated when the defer statement is evaluated.
- The `defer` statement defers the execution of a function until the surrounding function returns.
- Defer statements are executed in **last-in-first-out** order.
- Deferred functions may read and assign to the returning function’s named return values.
- Defer calls are evaluated immediately but the function call is not executed until the surrounding function returns.
- **Definition**: The `defer` statement schedules a function call to be run after the function completes, useful for cleanup tasks.
- **Use Cases**: Closing files, unlocking mutexes, and managing other cleanup tasks.
- **Tips**: Use `defer` to ensure that cleanup happens even if a function exits unexpectedly.
- **Lesser-known facts**: Deferred functions are executed in reverse order of their declaration, which can be useful in resource management.

### Panic

- Panic is a built-in function that stops the ordinary flow of control and begins panicking.
- Panics can be initiated by invoking panic directly.
- They can also be caused by runtime errors, such as out-of-bounds array accesses.

- **Definition**: A panic is a state in which the program stops normal execution, typically due to an unrecoverable error.
- **Use Cases**: Useful for signaling serious errors that the program cannot or should not recover from.
- **Tips**: Use panic sparingly and only for unrecoverable situations; prefer error returns for regular error handling.
- **Lesser-known facts**: Panics can be caught and handled using `recover`, allowing the program to regain control.

### Recover

- Recover is a built-in function that regains control of a panicking goroutine.
- Recover is only useful inside deferred functions. During normal execution, a call to recover will return nil and have no other effect.
- If the current go-routine is panicking, a call to recover will capture the value given to panic and resume normal execution.

- **Definition**: Recover is a mechanism to regain control after a panic, allowing the program to continue execution.
- **Use Cases**: Useful for graceful error handling and cleanup in critical sections of code.
- **Tips**: Always use recover within a deferred function to safely regain control from a panic.
- **Lesser-known facts**: Recover can also be used to capture the panic message for logging or debugging purposes.

## Types, Type Assertions, Type Switches

### Type Assertions

- A type assertion provides access to an interface value's underlying concrete value.
- Syntax: `t := i.(T)` asserts that the interface value `i` holds the concrete type `T` and assigns the underlying value to the variable `t`.
- If `i` does not hold a `T`, this will trigger a panic.
  
- To safely check the type, use: `t, ok := i.(T)`.
  - If `i` holds a `T`, `t` will be the underlying value and `ok` will be true.
  - If not, `ok` will be false, and `t` will be the zero value of type `T`, with no panic occurring.

- **Definition**: A mechanism in Go to retrieve the underlying concrete value of an interface type.
- **Use Cases**: Useful in scenarios where you need to assert the specific type of an interface, such as when handling different types through a common interface.
- **Tips**: Use the two-value form of type assertion to avoid panics and to handle cases where the type may not match.
- **Lesser-known facts**: Type assertions are similar in syntax to map lookups, making them intuitive for Go developers. Additionally, an empty interface (`interface{}`) can hold any type, allowing for flexible design but also requiring careful type assertions.

