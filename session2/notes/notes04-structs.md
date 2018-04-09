## Structs are like Ruby classes w/o methods

```
package main

import "fmt"

type Cat struct {
	name string
	age int
}

func main() {
	gizmo := Cat{ name: "Gizmo", age: 99 }

	// Makes a cat with default name ("") and age (0).
	emptyCat := Cat{}
	// Another way.
	var emptyCat2 Cat

	emptyCat2.name = "Curie"
	emptyCat2.age = 4

	// %#v gives you further debugging info when printing.
	fmt.Printf("%#v\n", gizmo)
	fmt.Printf("%#v\n", emptyCat)
	fmt.Printf("%#v\n", emptyCat2)
}
```

* Structs just have instance variables. Not really any methods.
* They don't have "constructor" functions.
* You must *declare* a struct type before using it. You must declare
	all the variables needed.

## Structs are copied like everything else

```
package main

import "fmt"

type Cat struct {
	name string
	age int
}

func main() {
	gizmo := Cat{ name: "Gizmo", age: 99 }
	// Make a second Cat instance by copy the same attributes.
	gizmo2 := gizmo

	// Won't affect gizmo.
	gizmo2.name = "Curie"
	gizmo2.age = 4

	// %#v gives you further debugging info when printing.
	fmt.Printf("%#v\n", gizmo)
	fmt.Printf("%#v\n", gizmo2)
}
```

## To mutate a struct; function must get a pointer

```
package main

import "fmt"

func curieMutator(cat *Cat) {
	// You can assign to fields of a Cat pointer just like you can
	// assign to fields of a regular Cat. This changes the
	// underlying Cat that lives at the memory address.
	cat.name = "Curie"
	cat.age = 4
}

type Cat struct {
	name string
	age int
}

func main() {
	gizmo := Cat{ name: "Gizmo", age: 99 }
	curieMutator(&gizmo)

	// %#v gives you further debugging info when printing.
	fmt.Printf("%#v\n", gizmo)
}
```
