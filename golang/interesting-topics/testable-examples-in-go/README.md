## Testable Examples in Go

Go supports 'example functions' that can be optionally ran along the acutal unit tests. This is achieved by explicitly adding 'Output Comments' to the 'Example function'.

Lets see how it works!

add.go:

```go
func Add(x, y int) int {
	return x + y
}
```

For this simple `Add()` function, lets write unit tests.

```go
func TestAdder(t *testing.T) {
	sum := Add(3, 3)
	expected := 6

	if sum != expected {
		t.Errorf("expected: '%d' but got '%d'", expected, sum)
	}
}
```

In addition to this, we can also write an 'Example function' thats less verbose but also provides the functionality of supporting the package documentation. These do not take arguments and begin with the word '`Example`' instead of '`Test`'.

```go
// This is a Testable Example in Go.
// The "Output: 6" is an 'Output Comment' in Go.
// More info: https://go.dev/blog/examples
func ExampleAdd() {
	fmt.Println(strconv.Itoa(Add(3, 3)))
	// Output: 6
}
```

As the example function is executed, the testing framework captures data written to standard output and then compares the output against the example’s “Output:” comment. The test passes if the test’s output matches its output comment.
