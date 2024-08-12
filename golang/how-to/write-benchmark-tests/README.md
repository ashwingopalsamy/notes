# how-to/write-benchmark-tests

Unlike unit tests, benchmark tests measure the performance of code over a specific duration. The testing package provides the Benchmark function for this purpose.

```go
import "testing"

func BenchmarkMyFunction(b *testing.B) {
        for i := 0; i < b.N; i++ {
                // Code to be benchmarked
        }
}
```
The `b.N` variable represents the number of iterations the benchmark will run. The testing package automatically adjusts `b.N` to get precise measurements.

When the benchmark code is executed, it runs `b.N` times and measures how long it takes.

The amount of times the code is run shouldn't matter to you, the framework will determine what is a "good" value for that to let you have some decent results.

To run the benchmarks do `go test -bench=.` (or if you're in Windows Powershell `go test -bench="."`)

This can be further analyzed with tools like `pprof`. Also supports parallel benchmark execution with `b.Parallel`.
