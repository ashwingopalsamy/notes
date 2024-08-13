# There is no 'pass by reference' in Go

Go does not have 'reference variables', so Go does not have pass-by-reference function call semantics.

Unlike C++, each variable defined in Go program occupies a unique memory location.

It is possible to create two variables whose **contents point to the same storage location** (#1) but **not possible** to create a Go program where **two variables share the same storage location** (#2) in memory.

_Example:_

```go
func main() {
    var a int
    var b, c = &a, &a
    fmt.Println(b, c)   // #1: 0x1040a124 0x1040a124
    fmt.Println(&b, &c) // #2: 0x1040c108 0x1040c110
}
```

## What about 'Maps' & 'Channels'?

Maps and Channels in Go are actually not of 'reference types'.

> *A `map` value is a pointer to a `runtime.hmap` structure.*

A 'Map' takes the same size as a `uintptr` type.

```go
func main() {
	var m map[int]int
	var p uintptr
	fmt.Println(unsafe.Sizeof(m), unsafe.Sizeof(p)) // 8 8 (linux/amd64)
}
```

### Then, why doesn't we express it via *map[K]V ?

As Go evolved, it was decided to express a 'Map' as `map[K]V` instead of `*map[K]V`, as Maps cannot be dereferenced.