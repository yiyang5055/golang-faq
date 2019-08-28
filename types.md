#### Go是面向对象编程吗？

Yes and no. Although Go has types and methods and allows an object-oriented style of programming, there is no type hierarchy. The concept of “interface” in Go provides a different approach that we believe is easy to use and in some ways more general. There are also ways to embed types in other types to provide something analogous—but not identical—to subclassing. Moreover, methods in Go are more general than in C++ or Java: they can be defined for any sort of data, even built-in types such as plain, “unboxed” integers. They are not restricted to structs (classes).

Also, the lack of a type hierarchy makes “objects” in Go feel much more lightweight than in languages such as C++ or Java.

### How do I get dynamic dispatch of methods?

The only way to have dynamically dispatched methods is through an interface. Methods on a struct or any other concrete type are always resolved statically.

### Why is there no type inheritance?

Object-oriented programming, at least in the best-known languages, involves too much discussion of the relationships between types, relationships that often could be derived automatically. Go takes a different approach.

Rather than requiring the programmer to declare ahead of time that two types are related, in Go a type automatically satisfies any interface that specifies a subset of its methods. Besides reducing the bookkeeping, this approach has real advantages. Types can satisfy many interfaces at once, without the complexities of traditional multiple inheritance. Interfaces can be very lightweight—an interface with one or even zero methods can express a useful concept. Interfaces can be added after the fact if a new idea comes along or for testing—without annotating the original types. Because there are no explicit relationships between types and interfaces, there is no type hierarchy to manage or discuss.

It's possible to use these ideas to construct something analogous to type-safe Unix pipes. For instance, see how `fmt.Fprintf` enables formatted printing to any output, not just a file, or how the `bufio` package can be completely separate from file I/O, or how the `image` packages generate compressed image files. All these ideas stem from a single interface (`io.Writer`) representing a single method (`Write`). And that's only scratching the surface. Go's interfaces have a profound influence on how programs are structured.

It takes some getting used to but this implicit style of type dependency is one of the most productive things about Go.

### Why is `len` a function and not a method?

We debated this issue but decided implementing `len` and friends as functions was fine in practice and didn't complicate questions about the interface (in the Go type sense) of basic types.

### Why does Go not support overloading of methods and operators?

Method dispatch is simplified if it doesn't need to do type matching as well. Experience with other languages told us that having a variety of methods with the same name but different signatures was occasionally useful but that it could also be confusing and fragile in practice. Matching only by name and requiring consistency in the types was a major simplifying decision in Go's type system.

Regarding operator overloading, it seems more a convenience than an absolute requirement. Again, things are simpler without it.

### Why doesn't Go have "implements" declarations?

A Go type satisfies an interface by implementing the methods of that interface, nothing more. This property allows interfaces to be defined and used without needing to modify existing code. It enables a kind of [structural typing](https://en.wikipedia.org/wiki/Structural_type_system) that promotes separation of concerns and improves code re-use, and makes it easier to build on patterns that emerge as the code develops. The semantics of interfaces is one of the main reasons for Go's nimble, lightweight feel.

See the [question on type inheritance](https://golang.org/doc/faq#inheritance) for more detail.

### How can I guarantee my type satisfies an interface?

You can ask the compiler to check that the type `T` implements the interface `I` by attempting an assignment using the zero value for `T` or pointer to `T`, as appropriate:

```go
type T struct{}
var _ I = T{}       // Verify that T implements I.
var _ I = (*T)(nil) // Verify that *T implements I.
```

If `T` (or `*T`, accordingly) doesn't implement `I`, the mistake will be caught at compile time.

If you wish the users of an interface to explicitly declare that they implement it, you can add a method with a descriptive name to the interface's method set. For example:

```go
type Fooer interface {
    Foo()
    ImplementsFooer()
}
```

A type must then implement the `ImplementsFooer` method to be a `Fooer`, clearly documenting the fact and announcing it in [go doc](https://golang.org/cmd/go/#hdr-Show_documentation_for_package_or_symbol)'s output.

```go
type Bar struct{}
func (b Bar) ImplementsFooer() {}
func (b Bar) Foo() {}
```

Most code doesn't make use of such constraints, since they limit the utility of the interface idea. Sometimes, though, they're necessary to resolve ambiguities among similar interfaces.

### Why doesn't type T satisfy the Equal interface?

Consider this simple interface to represent an object that can compare itself with another value:

```go
type Equaler interface {
    Equal(Equaler) bool
}
```

and this type, `T`:

```go
type T int
func (t T) Equal(u T) bool { return t == u } // does not satisfy Equaler
```

Unlike the analogous situation in some polymorphic type systems, `T` does not implement `Equaler`. The argument type of `T.Equal` is `T`, not literally the required type `Equaler`.

In Go, the type system does not promote the argument of `Equal`; that is the programmer's responsibility, as illustrated by the type `T2`, which does implement `Equaler`:

```go
type T2 int
func (t T2) Equal(u Equaler) bool { return t == u.(T2) }  // satisfies Equaler
```

Even this isn't like other type systems, though, because in Go *any* type that satisfies `Equaler` could be passed as the argument to `T2.Equal`, and at run time we must check that the argument is of type `T2`. Some languages arrange to make that guarantee at compile time.

A related example goes the other way:

```go
type Opener interface {
   Open() Reader
}

func (t T3) Open() *os.File
```

In Go, `T3` does not satisfy `Opener`, although it might in another language.

While it is true that Go's type system does less for the programmer in such cases, the lack of subtyping makes the rules about interface satisfaction very easy to state: are the function's names and signatures exactly those of the interface? Go's rule is also easy to implement efficiently. We feel these benefits offset the lack of automatic type promotion. Should Go one day adopt some form of polymorphic typing, we expect there would be a way to express the idea of these examples and also have them be statically checked.

### Can I convert a []T to an []interface{}?

Not directly. It is disallowed by the language specification because the two types do not have the same representation in memory. It is necessary to copy the elements individually to the destination slice. This example converts a slice of `int` to a slice of `interface{}`:

```go
t := []int{1, 2, 3, 4}
s := make([]interface{}, len(t))
for i, v := range t {
    s[i] = v
}
```

### Can I convert []T1 to []T2 if T1 and T2 have the same underlying type?

This last line of this code sample does not compile.

```go
type T1 int
type T2 int
var t1 T1
var x = T2(t1) // OK
var st1 []T1
var sx = ([]T2)(st1) // NOT OK
```

In Go, types are closely tied to methods, in that every named type has a (possibly empty) method set. The general rule is that you can change the name of the type being converted (and thus possibly change its method set) but you can't change the name (and method set) of elements of a composite type. Go requires you to be explicit about type conversions.

### Why is my nil error value not equal to nil?

Under the covers, interfaces are implemented as two elements, a type `T` and a value `V`. `V` is a concrete value such as an `int`, `struct` or pointer, never an interface itself, and has type `T`. For instance, if we store the `int` value 3 in an interface, the resulting interface value has, schematically, (`T=int`, `V=3`). The value `V` is also known as the interface's *dynamic* value, since a given interface variable might hold different values `V` (and corresponding types `T`) during the execution of the program.

An interface value is `nil` only if the `V` and `T` are both unset, (`T=nil`, `V` is not set), In particular, a `nil` interface will always hold a `nil` type. If we store a `nil` pointer of type `*int` inside an interface value, the inner type will be `*int`regardless of the value of the pointer: (`T=*int`, `V=nil`). Such an interface value will therefore be non-`nil` *even when the pointer value V inside is* `nil`.

This situation can be confusing, and arises when a `nil` value is stored inside an interface value such as an `error`return:

```go
func returnsError() error {
	var p *MyError = nil
	if bad() {
		p = ErrBad
	}
	return p // Will always return a non-nil error.
}
```

If all goes well, the function returns a `nil` `p`, so the return value is an `error` interface value holding (`T=*MyError`, `V=nil`). This means that if the caller compares the returned error to `nil`, it will always look as if there was an error even if nothing bad happened. To return a proper `nil` `error` to the caller, the function must return an explicit `nil`:

```go
func returnsError() error {
	if bad() {
		return ErrBad
	}
	return nil
}
```

It's a good idea for functions that return errors always to use the `error` type in their signature (as we did above) rather than a concrete type such as `*MyError`, to help guarantee the error is created correctly. As an example,[`os.Open`](https://golang.org/pkg/os/#Open) returns an `error` even though, if not `nil`, it's always of concrete type [`*os.PathError`](https://golang.org/pkg/os/#PathError).

Similar situations to those described here can arise whenever interfaces are used. Just keep in mind that if any concrete value has been stored in the interface, the interface will not be `nil`. For more information, see [The Laws of Reflection](https://golang.org/doc/articles/laws_of_reflection.html).

### Why are there no untagged unions, as in C?

Untagged unions would violate Go's memory safety guarantees.

### Why does Go not have variant types?

Variant types, also known as algebraic types, provide a way to specify that a value might take one of a set of other types, but only those types. A common example in systems programming would specify that an error is, say, a network error, a security error or an application error and allow the caller to discriminate the source of the problem by examining the type of the error. Another example is a syntax tree in which each node can be a different type: declaration, statement, assignment and so on.

We considered adding variant types to Go, but after discussion decided to leave them out because they overlap in confusing ways with interfaces. What would happen if the elements of a variant type were themselves interfaces?

Also, some of what variant types address is already covered by the language. The error example is easy to express using an interface value to hold the error and a type switch to discriminate cases. The syntax tree example is also doable, although not as elegantly.

### Why does Go not have covariant result types?

Covariant result types would mean that an interface like

```go
type Copyable interface {
	Copy() interface{}
}
```

would be satisfied by the method

```go
func (v Value) Copy() Value
```

because `Value` implements the empty interface. In Go method types must match exactly, so `Value` does not implement `Copyable`. Go separates the notion of what a type does—its methods—from the type's implementation. If two methods return different types, they are not doing the same thing. Programmers who want covariant result types are often trying to express a type hierarchy through interfaces. In Go it's more natural to have a clean separation between interface and implementation.