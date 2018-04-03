## Go is statically typed

```
package main

import "fmt"

func main() {
	var sum int = 0
	var i int
	for i = 0; i < 5; i++ {
		sum += i
	}
	fmt.Printf("Hello sum: %v!\n", sum)
}
```

* `var sum int` *declares* a variable named `sum` of type `int`.
* Every variable has a type. That variable can *only* hold values of that type.
  * `var sum int = 0.5` gives an error.
* You don't need to give a value for a variable declaration. Uses a *zero value*.
  * For ints and float32, this is 0 or 0.0.

## For Loops

```
	for i = 0; i < 5; i++ {
		sum += i
	}
```

* Look basically identical to JavaScript loops.
* Except no need for parens.

## Simplified Variable Assignment

```
package main

import "fmt"

func main() {
	sum := 0
	for i := 0; i < 5; i++ {
		sum += i
	}
	fmt.Printf("Hello sum: %v!\n", sum)
}
```

* Often don't need to explicitly use `var`.
* The type will be figured out by Go.
* You can declare a variable this way in a for loop.
* `%v` is how you do interpolation.
* You need to terminate the string with `\n`.

## Functions

```
package main

import "fmt"

func f(i int, j int) int {
	return i + j
}

func main() {
	sum := 0
	for i := 0; i < 5; i++ {
		sum = f(sum, i)
	}
	fmt.Printf("Hello sum: %v!\n", sum)
}
```

* Functions work mostly like JavaScript.
* Explicitly returns.
* Must declare types of arguments.
* The `int` in between `)` and `{` is the *return type*. It's the type of the thing which is returned.

## Basic types

* `int`
* `float32`
* `bool`
* `string`

## `if` and `else if` and `else`

```
package main

import "fmt"

func main() {
	if 3 < 4 {
		fmt.Printf("3 < 4\n")
	} else if 4 < 5 {
		fmt.Printf("4 < 5\n")
	} else {
		fmt.Printf("what else?\n")
	}
}
```

* As usual.
