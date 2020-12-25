# day12
## Summary
## 单元测试
**要测试的代码**
```
func Fib(n int) int {
        if n < 2 {
                return n
        }
        return Fib(n-1) + Fib(n-2)
}
```
**测试代码**
```
func TestFib(t *testing.T) {
    var (
        in       = 7
        expected = 13
    )
    actual := Fib(in)
    if actual != expected {
        t.Errorf("Fib(%d) = %d; expected %d", in, actual, expected)
    }
}
```
**输出**
```
$ go test .
ok      chapter09/testing    0.007s
```
### table-driven test
```
func TestFib(t *testing.T) {
    var fibTests = []struct {
        in       int // input
        expected int // expected result
    }{
        {1, 1},
        {2, 1},
        {3, 2},
        {4, 3},
        {5, 5},
        {6, 8},
        {7, 13},
    }

    for _, tt := range fibTests {
        actual := Fib(tt.in)
        if actual != tt.expected {
            t.Errorf("Fib(%d) = %d; expected %d", tt.in, actual, tt.expected)
        }
    }
}
```
## 基准测试
**测试代码**
```
func BenchmarkFib10(b *testing.B) {
        for n := 0; n < b.N; n++ {
                Fib(10)
        }
}
```
**结果**
```
$ go test -bench=.
BenchmarkFib10-4        3000000           424 ns/op
PASS
ok      chapter09/testing    1.724s
```