# Types & Function
```go
type Struct struct {
	...
}

type (
	Numeric interface {
		int | float32
	}
	Vec3[T Numeric] struct { x,y,z T }
	Vec2[T Numeric] struct { x,y   T }
)

func Add[T Numeric](a, b T) T {
	return a + b
}

func AddV3[T Numeric](a, b Vec3[T]) Vec3[T] {
	return Vec3{
		x: a.x + b.x,
		y: a.y + b.y,
		z: a.z + b.z,
	}
}
```


# Illegal pointer addressing
```go
var i int
p := &i /// type of 'p' is *int
{
	var n int
	p = &n  /// illegal, p is an immutable pointer and cannot be changed.
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
