# Illegal pointer-addressing
```go
var i int
p := &i /// &int
{
	var n int
	p = &n  /// illegal
}
```

# Simplified Typeswitch example
```go
var x any
a, b := any(1.0), any(float32(1.0))
switch typeof(a) {
	case float32:
}

func printfmt(fmt string, a ...any) int {}
/// equivalent to:
func print_array(fmt string, a []any) int {}
```
