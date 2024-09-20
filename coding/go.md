# Docs
[official docs](https://go.dev/)
[managing go versions](https://go.dev/doc/manage-install)
[effective-go](https://go.dev/doc/effective_go)

# Topics
[Go concurrency patterns](https://www.youtube.com/watch?v=f6kdp27TYZs)
[Advanced Go concurrency patterns](https://www.youtube.com/watch?v=QDDwwePbDtw)

# Why go?
- simplicity over expressiveness
- fast compilation speed (good for large code bases)
- concurrency first

# Installation

```bash
# if this is first install
wget https://go.dev/dl/go1.20.2.linux-amd64.tar.gz
# **Do not** untar the archive into an existing /usr/local/go tree. This is known to produce broken Go installations.
sudo tar -C /usr/local -xzf go1.20.2.linux-amd64.tar.gz
# for consecutive installs
sudo rm -rf /usr/local/go && tar -C /usr/local -xzf go1.20.2.linux-amd64.tar.gz
# add into PATH
export PATH=$PATH:/usr/local/go/bin
# and finally validate installaion
go version
```

# Basics

create a go module
```bash
go mod init <name>
```

resolve dependencies
```bash
go mod tidy
```

entry point
```go
package main // package declaration to group functions together

import "fmt" // importing fmt module from std package

import "rsc.io/quote" // external module

func main() {
	fmt.Println(quote.Go())
}
```

:= operator
>to declare and initialize, note that this operator cannot infer specific bits ie; int16 / float64
```go
// the long way
var message string
message = fmt.Println("Hello %v", name)
// the short form
mesage := fmt.Println("Hello %v", name)
```

strings
```go
s := "some string" // var string short form
strconv.Itoa(69) // properly convert 69 into "69"
string(69) // will convert to E as 69 is seen as a rune

// comparing runes
for _, v := range(s) {
	if v == rune('A') {
		return true
	}
}
```

arrays
```go
var a [4]int
a[0] = 1
i := a[0]

// array literals
b := [2]string{"Penn", "Teller"} 
// same as above
b = [...]string{"Penn", "Teller"}

// get length of array
len(b)

// indexing from a pointer array
some_array := [3]int{1, 2, 3}
p := &some_array
fmt.Println((*p)[0]) // 1

// loop over an array
for i, v := range(some_array) /* can omit the parentheses */ { 
	fmt.Println(i, v)
}

// appending to existing array
arr := [3]int{1, 2, 3}
arr = append(arr, []int{4, 5}...)
```

slices
>slice literal is declared just like an array literal, except you leave out the element count
```go
letters := []string{"a", "b", "c", "d"}

// slice from "slicing" elements
b := []byte{'g', 'o', 'l', 'a', 'n', 'g'}
// ranges are [a:b)
// b[1:4] -> []byte{'o', 'l', 'a'}
// b[:2] -> []byte{'g', 'o'}
// b[2:] -> []byte{'l', 'a', 'n', 'g'}
// b[:] -> b

// another way to create slices
x := []string{"Лайка", "Белка", "Стрелка"}
s := x[:] // a slice referencing the storage of x

// clear slice
clear(s)

// use slices for dynamically sized arrays
var dynamic = make([]int32, 1000)
```

structs
```go
// definition
type Bar struct {
	x           int64 // single declaration
	a, b, c, d  int32 // multiple declaration
}

type Foo struct {
	Bar      // embedded struct
	baz      int
}

// if struct points to same struct
type Node struct {
	Value   int32
	Next    *Node
}

// pointer receiver
func (bar *Bar) DoSomething(a int32, b int32, c int32, d int32) {
	bar.a = a
	bar.b = b
	bar.c = c
	bar.d = d
}

func main() {
	// initialize
	b := Bar{}

	// call method
	b.DoSomething(1, 2, 3, 4)

	// embedded structs can be accessed as if its the same member of the struct
	fmt.Println("%v", b.x)
}
```

interfaces
```go
type IPreference interface {
	doSomething(input string) string
}

type Like struct { }

func (f Like) doSomething(input string) string {
	return "I like " + input
}

type Hate struct { }

func (f Hate) doSomething(input string) string {
	return "I hate " + input
}

func main() {
	// declare variable interface
	var a IPreference
	// pass struct to interface
	a = Like {}
	a.doSomething("Ice cream");
	a = Hate {}
	a.doSomething("Potatoes");
}

```

maps
```go
	// instantiate a map
    messages := make(map[string]string) // or map[string]string{}

	// setting/updating value in map
	messages["key"] = "value"

	// getting value from map
	// will return the zero type if doesn't exist, in this case its ""
	val := messages["key"]

	// length of keys in a map
	length := len(messages)

	// deleting a key-value pair
	delete(messages, "key")

	// looping key, values in a map
	for k, v := range messages {
		fmt.Println(k, v)
	}

	// check if key exists in map
	someMap = make(map[string]int)
	if val, ok := someMap["key"]; ok {
		fmt.Println("Key exists", val)
	}
```

loops
```go
func hellos(names []string) (map[string]string, error) {
    // A map to associate names with messages.
    messages := make(map[string]string)

    // Loop through the received slice of names, calling
    // the Hello function to get a message for each name.
    for _, name := range names {
        message, err := Hello(name)
        if err != nil {
            return nil, err // WTF is this trash
        }
        // In the map, associate the retrieved message with
        // the name.
        messages[name] = message
    }
    return messages, nil
}
```

defer
```go
package main

import "fmt"

func main() {
	var x int32;
	// defer will wait until everything underneath it finishes then executes
	defer delayedAddition(&x, 4)
	fmt.Println(x) // 0

	// if there are two defers like this, then the bottom one will execute first
	defer delayedAddition(&x, 6)
	fmt.Println(x) // 0
	x += 5
	fmt.Println(x) // 5
	// x will be 11 and then 15 after this
}

func delayedAddition(x *int32, y int32) {
	*x += y
}

// defer already evaluates the function
func main() {
	x := 0
	x++
	defer fmt.Println(x) // prints 1 even though it is executed at the end of the function 
	x++
	fmt.Println(x) // 2
}
```

generics
```go
// declare generic struct, K comparable is used for generic map
type Foo[K comparable, V any] struct {
	dict  map[K]V
}

// generic method
func (f *Foo[K, V]) Insert(key K, value V) {
	f.dict[key] = value
}

func (f *Foo[K, V]) Get(key K) V{
	v := f.dict[key]

	if v == nil {
		// return default zero generic
		return *new(V)
	}
}

func main() {
	// initilialized generic struct
	foo := Foo[string, string]{}
	foo.Insert("cat", "meow")

	fmt.Println(foo)
}
```

closures
```go
func closure(a int) func(int) int {
    sum := a
    return func(b int) int {
        sum += b
        return sum
    }
}

func main() {
    c := closure(1)
    fmt.Println(c(2))
}

```
## Modules

file structure
```
├── parent
    └── main.go
    └── go.mod
├───── child
	   └── sub.go
```

go.mod
```go
module parent

go 1.20
```

sub.go
```go
package child

// need to caps first letter to make it public
func Moo() string {
	return "Moo"
}
```

main.go
```go
package main

import (
	"fmt"
	"parent/child"
)

func main() {
	fmt.Println(child.Moo()) // Moo
}
```

## Error Handling

greetings.go
```go
package greetings

import (
    "errors" // import errors module
    "fmt"
)

func Hello(name string) string {
    if name == "" {
        return "", errors.New("empty name") // return error
    }

    message := fmt.Sprintf("Hi, %v. Welcome!", name)
    return message, nil // nil means no error
}
```

hello.go
```go
package main

import (
    "fmt"
    "log" // import log

    "example.com/greetings"
)

func main() {
    log.SetPrefix("greetings: ") // log errror
    log.SetFlags(0)

    message, err := greetings.Hello("") // this should return error

    if err != nil {
        log.Fatal(err) // stop program
    }

    fmt.Println(message)
}
```

output
```bash
greetings: empty name
exit status 1
```

## Concurrency

go routines
```go
package main

import (
	"fmt"
	"time"
)

func main() {
	x := 0
	go concurrent(&x)
	go concurrent(&x)
	x++
	fmt.Println(x)
	time.Sleep(100 * time.Millisecond)
	fmt.Println(x)
}

func concurrent(x *int) {
	*x++
}
```

channels
```go
package main

import (
	"fmt"
	"time"
)

func main() {
	// unbuffered channel (without capacity)
	// requires a receiver as soon as a message is emitted
	// this will only succeed when both sender and receiver are ready
	c := make(chan int)
	// buffered channel
	// at most one receiver
	bc := make(chan int, 1)

	go fillChan([4]int{0, 1, 2, 3}, c)
	go fillChan([4]int{5, 6, 7, 8}, c)
	go fillChan([4]int{9, 10, 11, 12}, c)
	go closeChan(c) // need to close channel if iterating over it
	closeChan(bc)
	// to consume from channel directly can do this x := <-c
	for i := range c {
		fmt.Println(i)
	}
}

func fillChan(arr [4]int, c chan int) {
	for _, v := range arr {
		c <- v // send value to channel
	}
}

func closeChan(c chan int) {
	time.Sleep(100 * time.Millisecond)
	close(c) // close channel
}
```

## Test

>Ending a file's name with _test.go tells the go test command that this file contains test functions.

greetings_test.go
```go
package greetings

import (
    "testing"
    "regexp"
)

// TestHelloName calls greetings.Hello with a name, checking
// for a valid return value.
func TestHelloName(t *testing.T) {
    name := "Gladys"
    want := regexp.MustCompile(`\b`+name+`\b`)
    msg, err := Hello("Gladys")
    if !want.MatchString(msg) || err != nil {
        t.Fatalf(`Hello("Gladys") = %q, %v, want match for %#q, nil`, msg, err, want)
    }
}

// TestHelloEmpty calls greetings.Hello with an empty string,
// checking for an error.
func TestHelloEmpty(t *testing.T) {
    msg, err := Hello("")
    if msg != "" || err == nil {
        t.Fatalf(`Hello("") = %q, %v, want "", error`, msg, err)
    }
}
```

running the test
```bash
go test
PASS
ok      example.com/greetings   0.364s

$ go test -v
=== RUN   TestHelloName
--- PASS: TestHelloName (0.00s)
=== RUN   TestHelloEmpty
--- PASS: TestHelloEmpty (0.00s)
PASS
ok      example.com/greetings   0.372s
```

## Build and Install

>The `go build` command compiles the packages, along with their dependencies, but it doesn't install the results.

>The `go install` command compiles and installs the packages.

changing install target
```bash
go env -w GOBIN=/path/to/your/bin
```
# Use-case

rand
```go
import (
	"math/rand"
	"time"
)

func main() {
	// first seed the rand generator
	// dynamic value for different results
	r := rand.New(rand.NewSource(time.Now().UnixNano()))

	// generate random int32
	v := r.Int31()
}
```

sort
```go
package main

import (
    "fmt"
    "strings"
    "sort"
)

func main() {
    input := "<user1> hello\n<user2> hi\n<user3> hola amigos!\n<user1> how are you guys today?"

    m := make(map[string]int)
    users := make([]string, 0)

    for _, v := range strings.Split(input, "\n") {
        msg := strings.Split(v, " ")

        if _, ok := m[msg[0]]; !ok {
            users = append(users, msg[0])
        }

        m[msg[0]] += len(msg[1:])
    }

    sort.SliceStable(users, func(i, j int) bool {
        return m[users[i]] > m[users[j]]
    })

    fmt.Println(users) // [user1, user3, user2]
}

```

casting
```go
package main

import "strconv"

func main() {
	// works for numericals
	var x int32 = 10
	var y float32 = (float32) x

	// to conv str to int or vice versa
	val := 100
	valString := strconv.Itoa(val)
	val = strconv.Atoi(valString)
}
```

[command line flags](https://gobyexample.com/command-line-flags)
```go
// flags.go
package main

import (
	"flag"
	"fmt"
)

var foo string

func main() {
	flag.StringVar(&foo, "f", "", "foo value")
	flag.Parse()
	fmt.Println(foo)
}
```

```bash
go run flags.go -foo abc # abc
```

concat string from string slice
```go
package main

import (
	"strings"
	"fmt"
)

func main() {
	input := []string{"abc", "def", "ghi"}
	output := strings.Join(input, " ")
	fmt.Println(output) // abc def ghi
}
```