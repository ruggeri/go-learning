## Go stores values like Ruby

```
package main

import "fmt"

func main() {
	x := 123
	y := x

	y = 456

	fmt.Printf("x: %v = 123\n", x)
	fmt.Printf("y: %v = 456\n", y)

	xs := [...]int{123, 456, 789}
	ys := xs

	ys[0] = -1

	fmt.Printf("xs: %v\n", xs)
}
```

* Even array values are copied. This is *unlike* Ruby.

## Pointers

```
package main

import "fmt"

func main() {
  x := 123
  // Could have written y := &x. Just wrote this way for explicitness.
  var y *int = &x
  *y = 456

  fmt.Printf("x: %v\n", x)
  fmt.Printf("y: %v\n", *y)
}
```

* Pointers store an *address in memory*.
* The type of a pointer to an integer is `*int`.
* You get the memory address of a variable by writing
	`&myFavoriteInteger`. The `&` is called the *reference operator*.
* You can store a value by writing `*myFavoriteInteger = 456`. The `*`
	is called the *dereference operator*.
* You can load and use the value by writing `456 + *myFavoriteInteger`.

## Pointers Allow Mutation By Functions

```
package main

import "fmt"

func myIncrementerFunction(x *int) {
	*x += 1
}

func main() {
	x := 123
	y := &x

	myIncrementerFunction(&x)
	myIncrementerFunction(y)

	fmt.Printf("x: %v\n", x)
	fmt.Printf("y: %v\n", *y)
}
```

* Without pointers there is no way to modify a passed in value.
