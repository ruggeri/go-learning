## Arrays

```
package main

import "fmt"

func main() {
	arr := [4]string{"these", "are", "my", "strings"}

	for i := 0; i < 4; i++ {
		fmt.Printf("Str: %v\n", arr[i])
	}
}
```

* You make an *array* like so.
* An array can contain only one kind of thing.
* You can leave out the `4` and put `...`.
* If you try `arr[5]` you will get an error.
* An array is **not** resizable.

## `for` with `range` and an Array

```
package main

import "fmt"

func main() {
	arr := [4]string{"these", "are", "my", "strings"}

	for idx, val := range arr {
		fmt.Printf("%v: %v\n", idx, val)
	}
}
```

## `for` as `while`

```
package main

import "fmt"

func main() {
	arr := [4]string{"these", "are", "my", "strings"}

  idx := 0
	for idx < 4 {
		fmt.Printf("%v: %v\n", idx, arr[idx])
    idx += 1
	}
}
```

* You can use `for` like a `while` loop.
* You can even write `for { ... }` to *infinite loop*.
* You can `break` out as usual (not shown).
