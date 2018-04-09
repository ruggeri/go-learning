## Anonymous Functions Are A Thing

```
package main

import "fmt"

func main() {
	value := 0

	myFunction := func (increment int) {
		value += increment
	}

	myFunction(123)
	myFunction(456)
	myFunction(789)
	fmt.Printf("Value: %v\n", value)
}
```

## Fibonacci Closure

```
package main

import "fmt"

// fibonacci is a function that returns
// a function that returns an int.
func fibonacci() func() int {
  prev, curr := 0, 1

  return func() int {
    result := prev
    next := prev + curr
	prev, curr = curr, next

	return result
  }
}

func main() {
	f := fibonacci()
	for i := 0; i < 10; i++ {
		fmt.Println(f())
	}
}
```

* Notice the type of the return value: `func() int`.
