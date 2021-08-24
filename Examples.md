# Illegal pointer-addressing
```go
var i int
p := &i
v := &p
{
	var n int
	*v = &n  /// illegal
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

# Builtin Tree example
```go
var t tree[int]
l := <-t
r := t->
l, r = <-t->
/// l, r := <-t->
var a int = *t   /// get current node/root value.
var b int = *<-t /// get left child value.
var c     = *t-> /// get right child value.

x, y := *<-t->   /// get both left + right child value together.

/// nil-check 'r's left node.
if <-r==nil {
	new_left := tree[int]{66}
	<-r = new_left
}

```

# Builtin Graph example
```go
/// int vertices, bool edges.
var g graph[int]bool

/// setting & getting weight between two vertices.
g[vert1, vert2] = weight
w := g[vert1, vert2]

/// adding vertice value.
g = append(g, vert)
```

# How Generics look in Golang.
```go
func F[T any](p T) { ... }

// ListHead is the head of a linked list.
type ListHead[T any] struct {
	head *ListElement[T]
}

// ListElement is an element in a linked list with a head.
// Each element points back to the head.
type ListElement[T any] struct {
	next *ListElement[T]
	val  T
	// Using ListHead[T] here is OK.
	// ListHead[T] refers to ListElement[T] refers to ListHead[T].
	// Using ListHead[int] would not be OK, as ListHead[T]
	// would have an indirect reference to ListHead[int].
	head *ListHead[T]
}

// SignedInteger is a type constraint that permits any
// signed integer type.
type SignedInteger interface {
	type int, int8, int16, int32, int64
}

// Set is a set of values.
type Set[T comparable] map[T]struct{}

// Make returns a set of some element type.
func Make[T comparable]() Set[T] {
	return make(Set[T])
}

// Add adds v to the set s.
// If v is already in s this has no effect.
func (s Set[T]) Add(v T) {
	s[v] = struct{}{}
}


// NamedInt is an int with a name. The name can be any type with
// a String method.
type NamedInt[Name fmt.Stringer] struct {
	Name
	val int
}

// Name returns the name of a NamedInt.
func (ni NamedInt[Name]) Name() string {
	// The String method is promoted from the embedded Name.
	return ni.String()
}


type S struct {
	T[int] // field name is T
}

func F(v S) int {
	return v.T // not v.T[int]
}
```

# Unrolled Linked List example
```go
type Node[T any] struct {
	next, prev *Node[T]
	value     []T
}

/// index read.
func (n *Node[T]) [](index uint) T {
	return n.value[index]
}
/// index write.
func (n *Node[T]) []=(index uint, value T) {
	n.value[index] = value
}

/// get next node.
func (n *Node[T]) ->() *Node[T] {
	return n.next
}
/// set next node.
func (n *Node[T]) ->=(x *Node[T]) {
	n.next = x
}

/// get prev node.
func (n *Node[T]) <-() *Node[T] {
	return n.prev
}
/// set prev node.
func (n *Node[T]) <-=(x *Node[T]) {
	n.prev = x
}

/// get both nodes.
func (n *Node[T]) <-->() (*Node[T], *Node[T]) {
	return n.next, n.prev
}
/// set both nodes.
func (n *Node[T]) <-->=(x, y *Node[T]) {
	n.next, n.prev = x, y
}

...

func main() {
	node := new(Node[int])
	<-node-> = new(Node[int]), new(Node[int])
}
```
