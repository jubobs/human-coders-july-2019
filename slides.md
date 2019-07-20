### Installation

* Go (`go version`: 1.12.7)
* Visual Studio Code (1.36)
* Go VSCode extension (0.11.4)
* Git (2.22.0)
* Wifi

---

### [A little bit about me](https://twitter.com/jub0bs)

(I'm jub0bs on Twitter and Stack Overflow)

---

### Who are you?

---

### What to expect from the course

* getting you started on Go
* emphasis on practice over theory
* building a small backend project
* interaction
* regular breaks

---

### Project: social-network username checker

* Motivation
* validity, availability of a prospective username
* HTTP client, server
* unit tests
* concurrency

---

### Go's history

* created at Google in 2009
* ...by Rob Pike, Robert Griesemer, Ken Thompson
* open-source (BSD license)
* Go 1.0 released in 2012
* Go 1.12.7 (current release)
* Go 1.13 to be released next week!
* powers Docker, Kubernetes, and many more

---

### What makes Golang special

* syntax reminiscent of C
* strong typing
* fast compilation
* garbage collector (fast & easy to tune)
* pointers, but no pointer arithmetic
* first-class functions
* object-oriented, but no inheritance
* built-in concurrency (channels, goroutines)
* no generics

---

### What makes Go's ecosystem special

* BSD licence
* an agenda of simplicity
* no centralised package repository
* rich standard library (incl. gofmt!)
* rich command-line interface
* playground: https://play.golang.org
* vibrant community
* hard to search for "go"!

---

### Teaser: A simple server

---

### Why learn Go now

* concurrency features
* package management (finally!)
* supported by all three major cloud providers
* slow-evolving language: never too late to get started

---

### Go playground

* acts as an online REPL
* permalinks playgrounds
* limitations
    * no networking (no HTTP requests, etc.)
    * no command-line arguments
    * no packages outside standard library

---

# Getting acquainted with Go

---

### Hello, World!

[playground](https://play.golang.org/p/xY2BPIIxBbk)

---

### Hello, World!

* `main` package: defines an executable program
* `main` function: program's entrypoint
* import declaration
* no semicolons!
* `go build main.go`
* `go run main.go`

---

### Keywords

```txt
break       default       func      interface   select
case        defer         go        map         struct
chan        else          goto      package     switch
const       fallthrough   if        range       type
continue    for           import    return      var
```

---

### Predeclared names

* Constants
```txt
false true nil iota
```
* Types (more about that later)
* Built-in functions
```txt
make    len     cap     new
append  copy    delete
complex real    imag
panic   recover
```

---

### Comments

* end-of-line comments: `// this is a comment`
* multiline comments (do not nest)
```go
/*
  this is
  a multiline comment
*/
```

---

### Imports

* `import "<package_path>"`
* package _name_: the last component of its path
* example: `io/ioutil`
* optionally, rename the package
* unused imported package => compilation error!
* factored form: possible to group multiple declarations under the same `import`

---

### The `fmt` package

* `fmt.Println`
* `fmt.Sprintf`
* `fmt.Printf`
* Useful format-string _verbs_: `%s`, `%d`, `%t`, `%c`, `%T`, `%v`, etc.

---

### Elements of style

* tabs, not spaces
* camel-case names
* keep identifiers short
* don't neglect error handling

---

# Variables

---

## Variable declaration

* an _addressable_ value
* `var` declaration
```
var <name> <type> = <expression>
```
* `<type>` and `= <expression>` are optional, but not both
* factored form: possible to group multiple declarations under the same `var`

---

### Short variable declaration

* short variable declaration
```
<name> := <expression>
```
* type is inferred from expression
* there must be new values on the left-hand side!
* unused declared variable => compilation error!
* use the _blank identifier_ (`_`) when needed
* shadowing (due to lexical scoping) can lead to bugs

---

### Zero value

* no uninitialised variable in Go!
* each type has a _zero value_
```go
var i int
fmt.Printf("value: %d\n", i)
```

---

### `var` or `:=`?

* `:=` only allowed for local variables
* style: favour `var` if the initial value is the zero value or doesn't matter

---

## Pointers

* reminder: variable is an addressable value
* pointer: a value that holds the address of a variable
* if variable `v` is of type `T`, a pointer to `v` is of type *T
* zero value: `nil`
* `&`: address-of operator
* `*`: dereference operator
* nil dereference: panic!
* demo in Go playground

---

# Basic types

---

## Numeric types

* signed integers: `int`, `int8`, `int16`, `int32` (aka `rune`), `int64`
* unsigned integers: `uint`, `uint8` (aka `byte`), `uint16`, `uint32`, `uint64`, `uintptr`
* word size (`int`, `uint`, `uintptr`) depends on architecture and compiler
* floating-pointer numbers: `float32`, `float64`
* complex numbers: `complex64`, `complex128`
* zero value: zero

---

### Runes

* `rune` is synonymous to `int32`
* represents a single Unicode code point
* UTF-8 encoding (1 to 4 bytes per rune)
* literal delimited by single quotes: `'a'`, `'汉'`
* escape sequences: '`\n'`, `\uhhhh`, `\Uhhhhhhhh`, etc.
* useful package: `unicode/utf8`
* number of bytes in rune `r`: `utf8.RuneLen(r)`

---

### Numeric operators

* arithmetic: `+`, `-`, `*`, `/`, `%`
* comparison: `==`, `!=`, `<`, `<=`, `>`, `>=`
* bitwise (integers): `&`, `|`, `^`, `&^`

---


### Integer increment/decrement statements

* increment: `++`
* decrement: `--`
* only postfix (no prefix increment/decrement in Go)
* a statement, not an expression!

---

### Booleans

* type `bool`
* values: `false` or `true`
* zero value: `false`
* not: `!`
* and: `&&`
* or: `||`

---

### Strings

* immutable data structure
* made up of a _pointer_ to a byte array and a length
* zero value: empty string (`""`)
* concatenation operator: `+`
* string literal delimited by double quotes
* useful packages: `strings`, `unicode/utf8`, `regexp`
* number of _bytes_ in string `s`: `len(s)`
* number of _runes_ in string `s`: `utf8.RuneCountInString(s)`

---

### Indexing a string

* 0-based indexing
* `s[i]` is the `i+1`th byte in string `s`
* if `i` out of bounds (`i < 0` or `i >= len(s)`), _panic_!

---

### Substring operator

* `s[lo:hi]`
* semi-open interval: bytes from index `lo` to index `hi` (exclusive)
* if _hi_ or _lo_ out of bounds or _hi < lo_, panic!
* `lo` and `hi` are optional
* `s[:hi]` is equivalent to `s[0:hi]`
* `s[lo:]` is equivalent to `s[lo:len(s)]`
* `s[:]` is equivalent to `s`
* structural sharing between string and its substrings

---

### Ranging over a string

* `range` operator allows iterating over the runes of a string
```
for i, r := range s {
 ...
}
```

---

## Named types

* user-defined types
* `type` declaration
```
type <name> <underlying_type>
```
* type safety!
* allow you to define _methods_

---

## Constants

* values fixed at compile time
* `const <name> = <expression>` (no function calls allowed in RHS)
* factored form: possible to group multiple declarations under the same `const`
* allow optimisations by the compiler
* two categories of constants: typed and untyped

---

### Typed constants

* basic types
* ... or named type whose ultimate underlying type is a basic type

---

### Untyped constants

* constants not committed to a particular type
* flexibility!
* six _kinds_ of untyped constants
* numeric untyped constants have at least 256 bits of precision
* [example from the `math` package](https://golang.org/pkg/math/#pkg-constants)

---

### Conversion

* no implicit casting in Go
* (almost) all conversions must be explicit
* example

---

### `iota` constant generator

* syntactic sugar for creating constants easily
* example in Go Playground

---

### Enum pattern

* no native enumeration types in Go :(
* convention: named type whose underlying type is `int`
* exercise: Rock, Paper, Scissors
* caveats: not very typesafe

---

# Basic control-flow structures

---

### `if` statement (basic form)
```
if <condition> {
  <body>
}
```
* no parentheses around condition
* braces are mandatory

---

### `if` statement (with optional declarations)
```
if [short-var-declarations;] <condition> {
  <body>
}
```
* variables scoped to the `if` statement
* examples in Go playground

---

### `if`/`else` statement
```
if <condition> {
  <if-body>
} else {
  <else-body>
}
```
* avoid long `if`-`else` chains
* `else` is seldom used in Go!


---

### `for` loop

```
for <condition> {
  <body>
}
```
* no `while` in Go

---

### `for` infinite loop

```
for {
  <body>
}
```

---

### C-style `for` loop

```
for <init>; <condition>; <post> {
  <body>
}
```

---

### `range`-based `for` loop

```
for <variables> := range <data-structure> {
  <body>
}
```
* possible with strings, arrays, slices, maps, channels
* exercise: print each rune of a string with its index
* more on that later

---

### Switch

```
switch <condition> {
case <expression>:
  ...
case <expression>:
  ...
...
default:
  ...
}
```

---

### Switch

* cases are evaluated from top to bottom
* the first match is executed
* if none match, the (optional) default case is executed
* `default` can occur at any place among the cases

---

### Tagless switch

```
switch {
case <condition>:
  ...
case <condition>:
  ...
...
default:
  ...
}
```
* style: favour tagless switch to `if`/`else` chains
* exercise: signum

---

# Functions

---

### Declaration syntax

```
func <name>(<param-list>) <result-list> {
  <body>
}
```
* possibly multiple results
* results can have names too!
* factored form of sequence of identically typed parameters / results
* examples

---

### Evaluations

---

### Functions are first-class values

* a function can be
    * another function's parameter
    * another function's result
* function literals

---

### Recursion

* functions can call themselves
* exercise: mutually recursive functions
    * `isEven(uint) bool`
    * `isOdd(uint) bool`

---

### Closures

* functions can capture variables in their environment
* example
* exercise: write a function that returns an increment

---

# Packages

---

# Error handling & reporting

---

### Signaling a failure to the caller

* a function that can fail returns an additional value
* this value comes last, by convention
* only one possible cause: `bool`
* multiple possible causes: `error` (an interface type)
* non-nil error: something bad happened

---

### Error-check idiom

```
if err := fou(); err != nil {
  // unhappy path :(
  // early return
}
// happy path
```
* convention: keep the happy path "on the left"
* (not nested in if-else statements)

---

### Beyond `err != nil`

* you might want to find out more about the nature of the error
* when possible, assert on the error's behaviour
* more on that later (when we know what interfaces are)

---

### Error handling

* always check for errors
* when appropriate, wrap errors in custom errors before reporting them
* few errors should be ignored

---

# Unit testing

---

### `testing` package

* tests for package `foo` should be written in `foo_test.go`
* `foo_test.go` must be placed inside folder `foo`
* the package declaration of `foo_test.go` matters:
    * `import foo`: white-box testing
    * `import foo_test`: black-box testing
* test methods start with prefix "Test" (e.g. `TestValidity`)
* Run `go test ./...` at the root of your module

---

### Code coverage
```
go test -coverprofile="coverprofile.tmp" ./...
go tool cover --html="coverprofile.tmp"
```

---

# Composite types

---

## Arrays

---

### Arrays

* fixed-length container of elements of the same type
* elements are contiguous in memory
* length is encoded in the array type (e.g. `[3]int`)
* default value: array full of zero values of the element type

---

### Array literals
```
words := [3]string{"foo", "bar", "baz"}
moreWords := [...]string { // specify the size is optional
  "qux",
  "quux", // the last comma is mandatory
}
```

---

### Operations on array

* indexing  (same as for strings)
* array elements are variables: `&arr[i]` is legal
* get a slice from an array: `words[...]`
* `copy(dst, src)`
* ranging over an array
```
for i, v := range arr {
  // ...
}
```

---


---

### Array are no reference types

* functions with array params get a full copy of them!
* for this reason, arrays are seldom used directly in Go

---

## Slices

---

### Slices

* fundamental, ubiquitous type in Go
* a resizable view inside an array
* a slice of `T` has type `[]T`
* composed of
    * a pointer to an array (inaccessible)
    * a length (`len`)
    * a capacity (`cap`): the size of the underlying array

---

### Slice is a reference type

* zero value: `nil`
* a function with a slice parameter gets a copy of it
* but the underlying array is not copied!

---

### Allocating a slice

* `s := make([]string, l)` // l: length
* `s := make([]string, l, c)` // c: capacity

---

### Slice literal

```
words := []string{"foo", "bar", "baz"}
moreWords := []string {
  "qux",
  "quux", // the last comma is mandatory
```

---

### Indexing into a slice

* same syntax as for strings
* indexing into a `nil` slice: panic!
* slice elements are variables: `&s[i]` is legal

---

### Slicing operator

* `s1 := s[i:j]`
* if `j` exceeds `cap(s)`, panic!
* yields a different view into the same array
* also: `s1 := s[i:j:k]` gives you more control over the resulting slice's capacity

---

### Ranging over a slice

```
for i, v := range s {
  // ...
}
```

---

### Appending an element to a slice

* use the built-in function `append`
* `s = append(s, elem)`
* built-in `append` copies elements to a larger underlying array if capacity has been reached

---

### Appending an element to a slice

* `s = append(s, s1...)`

---

### Aliasing

* multiple slices may use the same underlying array
* careful...


---

## Maps

---

### Maps

* collection of key-value pairs
* a map of `T`s to `U`s has type `map[T]U`
* the key type must be comparable
* implemented as a hash table

---

### Maps are reference types

* zero value: `nil`
* a function that takes a map argument gets a copy of the reference
* the underlying hash table does not get copied

---

### Allocating a map

```
m := make([string]int)
```

---

### Map literals

```
enToFr := map[string]string {
  "one": "un",
  "two": "deux",
  "three": "trois", // mandatory comma
}
```

---

### Accessing the elements of a map

* `v := m[k]`
* key `k` need not be in the map
* ... in which case `v` will be the zero value for the value type!

---

### Testing for membership

* `v := m[k] // then test that v is not the zero value?`

---

### Testing for membership

* `v := m[k] // then test that v is not the zero value?`
* no! (ambiguous)
```
v, ok := m[k]
if ok {
  // the key is present
}
```

---

### Ranging over a map
```
for k, v := range m {
  // ...
}
```
* iteration order is nondeterministic!
* if iteration order matters, first create a slice of sorted keys

---

### Sets

* No native set type in Go!
* Usually implement as `map[T]bool`
* ... or `map[T]struct{}`

---

## Structs

* Go playground

---

# Methods

---

# Interfaces

---

### Interfaces

* abstract type
* describes a type that has certain methods
* an interface is composed of
  * the name of a concrete type
  * a pointer to a value of that type
* THE mechanism for polymorphism in Go

---

### Interfaces are reference types

* zero value: `nil`
* an interface that holds a `nil` pointer to a concrete type is not itself `nil`!
* pointers to interfaces? rarely (if ever) useful

---

### Interface satisfaction

* verified at compile time
* implicit!
* compare to Java
```
class PokerHand implements Comparable<? super PokerHand>
```
* methods on the concrete type using a pointer receiver

---

### Interfaces as contracts

* some interfaces ask implementors more than satisfying a signature
* example: [`io.Reader`](https://golang.org/pkg/io/#Reader)
* not enforcible by the compiler!
* => write tests to check that the implementor is lawful!

---

### Keep interfaces small

* favour one-method interfaces
* many examples in the standard library
* naming convention for single-method interfaces: method + "er"
```
type Climber interface {
    Climb(int)
}
```
* easier to satisfy

---

## Notable interface examples

---

### The empty interface

* `interface{}`
* satisfied by _any_ type
* useful in some cases
```
go doc fmt.Println
```
* but not very typesafe
* avoid, if possible

---

### `Stringer` interface

```
type Stringer interface {
  String() string
}
```
* governs how `fmt` functions print values of a concrete type

---

### `error` interface

```
type error interface {
  Error() string
}
```
* `go doc errors`

---

### `Wrapper` interface

```
type Wrapper interface {
  // Unwrap returns the next error in the error chain.
  // If there is no next error, Unwrap returns nil.
  Unwrap() error
}
```
* useful for error-translation idiom
* i.e. creating high-level errors that refer to the underlying cause
* experimental in Go 1.12
* to be added to standard package `errors` in Go 1.13

---

### `Reader` interface

```
type Reader interface {
  Read(p []byte) (n int, err error)
}
```
* source of `[]byte`
* satisfied by `*os.File`, and many others

---

### `Writer` interface

```
type Writer interface {
  Write(p []byte) (n int, err error)
}
```
* sink of `[]byte`
* satisfied by `*os.File`, and many others

---

### Favour interface parameters

* if possible, take interface types as function parameters, rather than concrete types
* example
```
type Tree Struct {...}
func (t *Tree) Save(f *os.File) error
```

---

### Favour interface parameters

* if possible, take interface types as function parameters, rather than concrete types
* example
```
type Tree Struct {...}
func (t *Tree) Save(w io.Writer) error
```
* more flexible
* easier to test!

---

### Require no more, promise no less

* only ask for the required behaviour from interface parameters
```
func (f *Foo) Save(rw ReadWriter) error
```

---

### Require no more, promise no less

* only ask for the required behaviour from interface parameters
```
func (f *Foo) Save(w Writer) error // better!
```
* similar to DDD principle of "keeping ports small"

---

### Interface composition

```
type ReadWriteCloser {
  Reader
  Writer
  Closer
}
```
* small interfaces are convenient for composition!

---

### Asserting on behaviour

```
func isTimeout(err error) bool {
    type timeout interface {
        Timeout() bool
    }
    te, ok := err.(timeout)
    return ok && te.Timeout()
}
```
* if `err` satisfies interface `timeout`
  * `te` has type `timeout`
  * `ok` is `true`

---

### Type switch

```
func do(i interface{}) {
  switch v := i.(type) { // what's the type of i?
  case int:
    //...
  case string:
    //...
  default:
    fmt.Printf("I don't know about type %T!\n", v)
  }
}
```

* creates a strong dependence on the concrete types
* avoid type switches on types you don't own

---

### Mechanics of the `defer` keyword

* delays a function call to when the enclosing function terminates
* ... whether normally or not (panic)
* the arguments of the deferred function call are evaluated at the `defer` statement!
* `defer` statements are executed in reverse order in which they appear in the enclosing function

---

### `defer` is useful for cleaning up resources

```
func IsAvailable(username string) (bool, error) {
  resp, err := http.Get("https://twitter.com/" + username)
  if err != nil {
    return false, errors.New("Unknown availability")
  }
  if resp.StatusCode != http.StatusNotFound {
    resp.Body.Close() // <--- reclaim resource no longer needed
    return false, nil
  }
  resp.Body.Close() // <--- needed here too!
  return true, nil
}
```

---

### `defer` is useful for cleaning up resources

```
func IsAvailable(username string) (bool, error) {
  resp, err := http.Get("https://twitter.com/" + username)
  if err != nil {
    return false, errors.New("Unknown availability")
  }
  defer resp.Body.Close() // <--- once and for all!
  if resp.StatusCode != http.StatusNotFound {
    return false, nil
  }
  return true, nil
}
```

---

### Gotcha: using `defer` in a loop

* Can you spot the problem?

```
func foo() {
  for i := range ... {
    r, err := obtainResource(i)
    if err != nil {
      // deal with error
    }
    defer r.Close()
    // do something with r...
  }
}
```

---

### Gotcha: using `defer` in a loop

* this can lead to resource exhaustion
* favour

```
func foo() {
  for i := range is {
    if err := process(i); err != nil {
      // deal with error
    }
  }
}
func process(i int) error {
  r, err := obtainResource(i)
  if err != nil {
    // deal with error
  }
  defer r.Close()
  // do something with r...
}
```

---

### Gotcha: errors in deferred function calls

* Deferred statements return no result.
* What if the function call returns an error?

```
func foo() error {
  // ...
  defer badIfThisFails() // but any error occuring here is lost!
  // ...
}
```

---

### Gotcha: errors in deferred function calls

* workaround: use a named return and a func literal

```
func foo() (err error) {
  // ...
  defer func() {
    if err1 := gobadIfThisFails(); err1 != nil {
      err = err1
  }()
  // ...
}
```

* relevant blogpost: https://www.joeshaw.org/dont-defer-close-on-writable-files/

---

# Concurrency

---

### Concurrency

* the art of composing a programme in terms of independenly executing computations
* arguably Go's most fascinating aspect
* very important in this era of multicores

---

### Concurrency is not parallelism

* a way of writing a program
* concurrent programme has value even if ultimately executed on a single core

---

### The origin of Go's concurrency

* Tony Hoare's _Communicating Sequential Processes_
* two fundamentals mechanisms:
  * goroutines
  * channels

---

### goroutines

* a concurrent function execution
* an executable's `main` function is itself a goroutine
* you can spawn more goroutines using the `go` keyword
* Go scheduler is in charge of switching between goroutines
* metaphor: a chef and his (her?) assistants in a restaurant kitchen

---

### The `go` keyword

* precede a function call by `go` to spawn a new goroutine
```
go prepareSoufflé()
go peelPotatoes()
```
* golden rule: before launching a goroutine, understand exactly how it will terminate
* exercise

---

### Wait groups

* `main` does not wait for other goroutines to terminate before it itself terminates
* if tempted to use a sleep, step away from the keyboard and think again
* _wait groups_ (`sync.WaitGroup`) can be used for
  * keeping track of the number of executing goroutines
  * waiting for that count to go down to 0

---

### Wait-group example

```
func main() {
  var wg sync.WaitGroup // remark: usable zero value!
  wg.Add(1)             // one more goroutine created
  go func() {
    defer wg.Done()     // one done
    prepareSoufflé()
  }()
  wg.Add(1)             // one more goroutine created
  go func() {
    defer wg.Done()     // one done
    peelPotatoes()
  }()
  wg.Wait()             // ... for all goroutines to be done
```

---

### Gotcha: goroutine & function literal

```
func printTenIntsConcurrently() {
  var wg sync.WaitGroup
  for i := 0; i < 10; i++ {
    wg.Add(1)
    go func() {
      defer wg.Done()
      fmt.Println(i)
    }()
  }
	wg.Wait()
}
```

* what's the problem?

---

### Gotcha: goroutine & function literal

```
func printTenIntsConcurrently() {
  var wg sync.WaitGroup
  for i := 0; i < 10; i++ {
    wg.Add(1)
    go func(j int) {
      defer wg.Done()
      fmt.Println(j)
    }(i)
  }
	wg.Wait()
}
```

* what changed?

---

### Gotcha: goroutine & function literal

* rule of thumb: avoid using func literals with `go`
* (unless the goroutine needs too much of its environment)
* instead, define a dedicated function

```
func printTenIntsConcurrently() {
  var wg sync.WaitGroup
  for i := 0; i < 10; i++ {
    wg.Add(1)
    go printConcurrently(i, &wg)
  }
	wg.Wait()
}
func printConcurrently(i int, wg *sync.WaitGroup) {
  defer wg.Done()
  fmt.Println(i)
}
```

---

### Channels

* _Do not communicate by sharing memory; instead, share memory by communicating._
* channel: mechanism for passing values of a certain type from one goroutine to another
* a channel of `T`s has type `chan T`
* naming convention: "ch" (alone or as suffix)

---

### Creating channels

* `ch := make(chan T, c)`
* `c`: optional capacity (defaults to zero)
* reference type
* zero value: `nil`

---

### Channel capacity

* a channel has
  * a length: `len(ch)` (seldom used)
  * a capacity: `cap(ch)`
* a channel of
  * zero capacity is called an _unbuffered channel_
  * positive capacity is called a _buffered channel_

---

### Closing a channel

* a (non-`nil`) channel starts its life as open
* a channel can be closed: `close(ch)`
* useful operation when ranging over a channel
* a closed channel cannot be re-opened
* attempting to close a closed channel: panic!

---

### Closing a channel (cont'd)

* closing a channel does not drain it from the elements it contains
* channels are not resources
  * only close them if the program requires it
  * otherwise, simply let them get GC'd

---

### Sending to a channel

* sending value `v` to channel `ch`:
```
ch <- v
```
* different behaviours, depending on the state of `ch`:
  * nil: blocking
  * open
    * not at capacity: adds `v` to `ch`
    * at capacity: blocking
  * closed: panic!

---

### Receiving from a channel

* receiving a value from channel `ch`:
```
<-ch
```
* to store the received value in a variable:
```
v := <-ch
```
* different behaviours, depending on the state of `ch`:
  * nil: blocking
  * not empty: yields an element from `ch`
  * empty:
    * open: blocks
    * closed: yields the zero value of the element type

---

### Dispelling the ambiguity

* If you a receive a zero value from a channel, how do you know whether it's open or closed?
* Like this:
```
v, ok := <-ch
```
* `ok` is
  * true if `ch` was still open at the time of receiving `v` from it
  * false if `ch` is closed

---

### Ranging over a channel

* receiving from a channel in a loop
```
for v := range ch {
  // ...
}
```
* loop only terminates once channel is closed
* exercise

---

### Directional channels

* send-only, receive-only channels
* send-only channel of `T`s has type
```
chan<- T
```
* receive-only channel if `T`s has type
```
<-chan T
```

---

### Directional channels (cont'd)

* useful in function signatures, to restrict channel behaviour
* the compiler disallows
  * receiving from a send-only channel
  * sending to a receive-only channel
  * closing a receive-only channel

---

### `chan struct{}`

* can be useful for signaling that some unambiguous event took place!

```
type empty struct{}
// ...
ch := make(chan struct{})
// ...
ch <- empty{}
```

---

### `select` key word

* analogous to a switch in which the first case that is "ready" gets executed

```
select {
case v := <-ch1:
  // do something with v
case ch2 <- 42:
  // ...
default:
  // ...
}
```

* `default` case
  * is optional
  * executes if none of the other cases is ready
* exercise

---

### Event loop

```
for {
  select {
  case ...:
    // ...
  case ...:
    // ...
  default:
    // ...
  }
}
```

* exercise

---

### Concurrency patterns

* we're only scratching the surface of what's possible
* many, many different possibilities with goroutines and channels
* to get an idea, watch _Go Concurrency Patterns (Rob Pike, Google I/O 2012)_:
  https://www.youtube.com/watch?v=f6kdp27TYZs

---

### Communicating by sharing memory

* why? for performance reasons
* two alternatives to channels
  * atomics
  * mutexes

---

### Atomics

* synchronized operations on fixed-sized integers
* see `sync/atomic` package
* exercise

---

### Mutexes

* "mutual exclusion"
* allows you to acquire/release a lock on a variable
* see `sync` package

---

### Mutex example: concurrency-safe map

```
var mu sync.Mutex
m := map[string]int
// ...
// inside some goroutine
s := "foo"
mu.Lock()
{
  m[s]++
}
mu.Unlock()
```

* readability tip: enclose the critical section in braces to make it stand out

---

### What we haven't covered

* useful third-party libraries
* concurrency patterns
* profiling
* micro-benchmarks
* escape analysis
* mechanics of the garbage collector
* mechanics of the Go scheduler
* `context` package
* compiler options

---

# Learning resources

---

### Books

* Kernighan & Donovan - The Go Programming Language
* Brian Ketelsen & Erik St. Martin - Go in Action
* Matt Ryer - Go Programming Blueprints (2nd ed.)

---

### Blogs

* Official Golang blog: https://blog.golang.org
* Dave Cheney: https://dave.cheney.net
* JBD/Rakyll: https://rakyll.org
* Jack Lindamood: https://medium.com/@cep21
* jubobs.com (shameless plug)

---

### Talks

* Anything by Rob Pike, Dave Cheney, Sameer Ajmani, or Matt Ryer
* Marcus Olsson - Building an enterprise service in Go (Golang UK Conference 2016)
* Dave Cheney - Concurrency made easy (GopherCon SG 2017)

---

### Vidcasts

* Francesc Campoy Youtube channel (JustForFunc)

---

### Video courses

* William Kennedy - Ultimate Go v2 (O'Reilly 2018)
* John Graham-Cumming - Introduction to Go Programming (O'Reilly 2014)
* Mike van Sickle's Pluralsight courses

---

### Podcasts

* Go Time: https://changelog.com/gotime

---

# Thank you!
