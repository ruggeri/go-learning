```
package main

import "fmt"

func main() {
	myMap := map[string]int{
		"abc": 123,
		"xyz": 456,
	}

	fmt.Printf("abc: %v\n", myMap["abc"])
	fmt.Printf("xyz: %v\n", myMap["xyz"])

	myMap["qqq"] = -1
	fmt.Printf("myMap: %#v\n", myMap)
}
```

* Type is `map[keyType]valueType`
* You can make an empty map like:
    * `map[string]int{}`
    * `make(map[string]int)`
    * No practical difference. Second is more common.

## Testing For Presence

```
package main

import "fmt"

func main() {
	myMap := map[string]int{
		"abc": 123,
		"xyz": 456,
	}

	_, ok := myMap["abc"]
	if ok {
		fmt.Printf("We have key abc!\n")
	}

  delete(myMap, "abc")
  fmt.Printf("Zero value: %v.\n", myMap["abc"])
}
```

* This uses the fact that `myMap` can do a *multiple return*.
* You can `delete` keys. It isn't an error to look up a non-present key.

## `map`s are sorta like passed by reference

```
package main

import "fmt"

func addKeyAndValue(m map[string]int, s string, i int) {
	m[s] = i
}

func main() {
	m := make(map[string]int)

	addKeyAndValue(m, "abc", 123)
	addKeyAndValue(m, "xyz", 456)
	fmt.Printf("Value: %v\n", m)
}
```

* You hardly ever need to pass a pointer to a map.
* Maps are already defined as pointers.
	* They are a *pointer* to a *header* which contains:
		1. Count of number of items.
		2. Store pointer.
* They are *unlike* slices:
	* A slice *is* a header with a:
		1. Length
		2. Capacity
		3. Store pointer
* For that reason, when you pass a slice by value, a new header is
	built.
		* Operations like `append` that mutate the header will not mutate
			the original.
* When you pass a map by value, you pass a copy of the pointer value
	to the original header.
		* No new header is built.
		* Therefore operations like `[]=` mutate the store in the original
			and only header.
* Exciting!
* [Gory Explanation](https://dave.cheney.net/2017/04/30/if-a-map-isnt-a-reference-variable-what-is-it)
