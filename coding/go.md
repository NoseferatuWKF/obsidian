## Docs

[official docs](https://go.dev/)
[managing go versions](https://go.dev/doc/manage-install)
[effective-go](https://go.dev/doc/effective_go)

## Why go?
- simplicity over expresiveness
- fast compilation speed (good for large codebases)
- handling error during runtime instead of compilation time
- you don't care about adding support for Windows

## Installation

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

## Basics

### let's go

create a go module
```bash
go mod init <name>
```

main entrypoint
>external deps can be resolved using go mod tidy
```go
package main // package declaration to group functions together

import "fmt" // importing fmt module from std package

import "rsc.io/quote" // external module

func main() {
	fmt.Println(quote.Go())
}
```

### setting variables

:= operator
```go
// the long way
var message string
message = fmt.Println("Hello %v", name)
// the short form
mesage := fmt.Println("Hello %v", name)
```

arrays
```go
var a [4]int
a[0] = 1
i := a[0]
// i == 1

// array literals
b := [2]string{"Penn", "Teller"} 
// same as above
b := [...]string{"Penn", "Teller"}
```

slices
> slice literal is declared just like an array literal, except you leave out the element count
```go
letters := []string{"a", "b", "c", "d"}

// slice from "slicing" elements
b := []byte{'g', 'o', 'l', 'a', 'n', 'g'}
// b[1:4] == []byte{'o', 'l', 'a'}, sharing the same storage as b
// b[:2] == []byte{'g', 'o'}
// b[2:] == []byte{'l', 'a', 'n', 'g'}
// b[:] == b

// another way to create slices
x := [3]string{"Лайка", "Белка", "Стрелка"}
s := x[:] // a slice referencing the storage of x
```

### loops
```go
func Hellos(names []string) (map[string]string, error) {
    // A map to associate names with messages.
    messages := make(map[string]string)
    // Loop through the received slice of names, calling
    // the Hello function to get a message for each name.
    for _, name := range names {
        message, err := Hello(name)
        if err != nil {
            return nil, err
        }
        // In the map, associate the retrieved message with
        // the name.
        messages[name] = message
    }
    return messages, nil
}
```

### modules

file structure
```
├── home
├── greetings
│   ├── go.mod
│   └── greetings.go
└── hello
    ├── go.mod
    └── hello.go
```

greetings.go
```go
package greetings

import "fmt"

func Hello(name string) string {
    message := fmt.Sprintf("Hi, %v. Welcome!", name)
    return message
}
```

hello.go
```go
package main

import (
    "fmt"

    "example.com/greetings"
)

func main() {
    message := greetings.Hello("Human")
    fmt.Println(message)
}
```

using module
>technically packages should be placed in an external repo, to make things work locally, go can redirect the package

```bash
go mod edit --replace example.com/greetings
go mod tidy
```

hello/go.mod
```go
module example.com/hello

go 1.20

replace example.com/greetings => ../greetings

require example.com/greetings v0.0.0-00010101000000-000000000000

```

### error handling

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

### test

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

### build and install

>The `go build` command compiles the packages, along with their dependencies, but it doesn't install the results.

>The `go install` command compiles and installs the packages.

changing install target
```bash
go env -w GOBIN=/path/to/your/bin
```