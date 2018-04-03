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
