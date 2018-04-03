## Reslicing and "Popping"

```
package main

import "fmt"

func main() {
	// Empty slice
	mySlice := []int{123, 456, 789}

	// "pops" (sorta) the last value off the slice.
	mySlice2 := mySlice[:2]
	fmt.Printf("%#v\n", mySlice2)

	// len is 3
	fmt.Printf("len(mySlice) = %v\n", len(mySlice))
	// len is 2
	fmt.Printf("len(mySlice2) = %v\n", len(mySlice2))
}
```

* Assignment to `mySlice2` does a *reslice*.
* It just creates another slice, with the *same* backing store, but
	with a shorter length.
* It does *not* remove 789 from the backing store.
* `mySlice` continues to have length 3.

## New Slice Will "Grow Into" Old Store

```
package main

import "fmt"

func main() {
	// Empty slice
	mySlice := []int{123, 456, 789}

	// "pops" (sorta) the last value off the slice.
	mySlice2 := mySlice[:2]
	fmt.Printf("%#v\n", mySlice2)

	mySlice2 = append(mySlice2, -1)

	fmt.Printf("%#v\n", mySlice)
	fmt.Printf("%#v\n", mySlice2)
}
```

* mySlice2's backing store still has capacity for growth.
* So mySlice2 puts the appended item at position 2.
* But position 2 is also referenced already by mySlice.
* So this change to mySlice2 mutates the original.
