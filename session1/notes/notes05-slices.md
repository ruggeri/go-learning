## Slices (kinda like dynamic arrays)

```
// Imaginary
type []YourTypeHere struct {
  len int
  capacity int
  storePtr *YourTypeHere
}
```

* You use slices much more than arrays.
* The slice type is `[]int` vs `[3]int` which is an array type.

## Appending to a slice

```
package main

import "fmt"

func main() {
  // Default empty slice has zero elements.
	var mySlice []int

  // append doesn't actually mutate the slice
  // necessary to reassign.
	mySlice = append(mySlice, 123)
	mySlice = append(mySlice, 456)
	mySlice = append(mySlice, 789)

  // Equivalent "slice literal"
	mySlice2 := []int{123, 456, 789}

	// %#v gives you further debugging info when printing.
	fmt.Printf("%#v\n", mySlice)
  fmt.Printf("%#v\n", mySlice2)
}
```

* `append` doesn't mutate the slice's length/capacity/store.
* Therefore you need to use the slice value returned by `append`. Most common is just to reassign it back to the original slice variable.
* If there is no more capacity, `append` will double the store for you.

## Two slices can refer to the same store

```
package main

import "fmt"

func main() {
	mySlice := []int{123, 456, 789}
  // Copies the slice; but not the store!
	mySlice2 := mySlice

	mySlice2[1] = -1

	fmt.Printf("%#v\n", mySlice)
	fmt.Printf("%#v\n", mySlice2)
}
```

* The two distinct slice variables both store different slices; but they both refer to the same store.

## For assignment, don't need `*[]int` for mutator functions

```
package main

import "fmt"

func mutatorFunction(sliceArgument []int) {
	sliceArgument[1] = -1
}

func main() {
	mySlice := []int{123, 456, 789}
	mutatorFunction(mySlice)

	fmt.Printf("%#v\n", mySlice)
}
```

* This wasn't the case for passing a struct.
* This is okay if you're just changing the backing store.

## If you `append` must pass `*[]int`

```
package main

import "fmt"

func mutatorFunction(sliceArgument *[]int) {
	*sliceArgument = append(*sliceArgument, 123)
	*sliceArgument = append(*sliceArgument, 456)
	*sliceArgument = append(*sliceArgument, 789)
}

func main() {
	// Empty slice
	mySlice := []int{}
	mutatorFunction(&mySlice)

	fmt.Printf("%#v\n", mySlice)
}
```

* That's because `append` wants to change the slice values like
	length.
* But also may change the store!
