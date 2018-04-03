## Installation and Setup

* `brew install go`
* Make a `~/go` directory in your home directory.
    * Kinda annoying but just do it.
* Create `~/go/src/github.com/your_name/my_go_notes` directory
* This is where your Go notes repository will live for now.
* Create a Readme and `git init` and commit.
* Push to Github.

## Why Go?

* Many times faster than typical Python or Ruby code.
* Builds very fast. Starts up program very fast.
* Statically typed: prevents errors of returning the wrong kind of
  thing or accessing a instance variable or method that doesn't exist.
* Much friendlier than C or C++: fewer possibilities for errors.
* Garbage collected.
* Purpose: it easy to write server processes which handle many
  simultaneously connected clients.
* No inheritance. Simpler model.

## Go Competitors

* C/C++/Rust dominate for very low level performance imperative code.
* Python/Ruby dominate for rapid prototyping.
* Node.js is somewhat similar, but dynamically typed and async.
  * Golang has much better performance for some tasks
  * But for web servers similar scalability.
* Java is a competitor, but many developers find a lot of "boilerplate"
  code is necessary. Bureaucratic
  * Java has gotten much better.
  * Scala is a newer Java-like language. But considered to be very
    complex.
  * Any JVM language suffers from very slow startup (and compilation
    too).
* Elixir is another competitor like Node.js, but still niche and more
  different.

## First Go Program

* Create a `hello_world/cmd/hello_world` directory in your Go notes.
* Touch `hello_world/cmd/hello_world/main.go`

```
package main

import "fmt"

func main() {
  fmt.Println("Hello world!")
}
```

* Run `go build` in `hello_world/cmd/hello_world`.
* This builds a `hello_world` executable.

## Hello World Packaging

* `package main` at the top defines the "package" your code belongs to.
* The package is *normally* the name of the directory (`hello_world`).
* The exception is for `main.go` files.

## import

* You import libraries with `import "fmt"`.
* You can use functions from those libraries like so:
  `fmt.Println(...)`.
* Only functions defined with a leading capital letter are *exported*.
* You must always use the `fmt.` prefix.
