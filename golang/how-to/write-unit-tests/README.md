# how-to/write-unit-tests

1. Write a testable code
2. Make the compiler pass
3. Run the test, see that it fails and check the error message is meaningful
4. Write enough code to make the test pass
5. Refactor
6. Write failing tests and ensure it sends back the errors as intended
7. Think & add tests for corner and edge cases

## Best Practices

- Practice test-driven development (TDD).
- Leverage table-driven tests effectively.
- Always test both Positive & Negative scenarios.


### Syntax

```go
import "testing"

func TestMyFunction(t *testing.T) {
        // Test logic here
        t.Error("Test failed") // Or t.Fatal for immediate failure
}

```

