## Features

1. package main, main function
2. Imports
3. fmt.Printf. Format `%g`.
4. Functions, arguments, types of arguments. Return type
    * Elision of uneeded argument types.
    * Named return (sounds idiotic)
5. Multiple return.
6. Variable declaration. You can use `:=` most places.
7. Types: bool, string, int, float32.
    * Good example of grumpiness: Math.sqrt needs a float.
8. `const` declarations.
9. `for` loop. 1-part `for` is the same as `while`. 0-part `for` loops
   forever.
10. `if`. Fancy version can declare a variable first which is in scope
    only for the `if`. Also `else` and `else if`.
11. `switch`. No need for `break`.
12. `defer`: like a `begin` and `ensure`. Stacking is weird...
13. Pointers using `*` and the take reference `&`.
14. `type Xyz struct` declares a new struct. Like a class. Field
    access with `.`. No need for `->`. Struct literals are nice with
    `Vertex{ X: 1, Y: 123 }` format.
15. Arrays cannot be resized. You declare one like `myArr
    [10]int`. Also nice literals.
16. Slices are `mySlice []int = myArr[1: 5]`. Basically like a pointer
    to an array. But knows its length. Can ommit start or end or both.
17. `len(slice)` makes sense. You can also consider the *capacity*
    (`cap(slice)`). Insane: you can extend a slice by reslicing *past
    the end*, provided there is enough capacity. Not sure why you
    would want to yet... I think it may be like when you pass an array
    of size 255 and want them to fill it out and tell you how many
    elements?
18. Zero value of a slice is `nil`. Same as for reference.
19. Dynamically sized structures are created via `make([]int,
    10)`. You can also give a capacity like: `make([]int, 0, 255)`.
20. For convenience there is an `append(slice, v1, v2, v3...)`
    function. It will use more capacity is possible, but otherwise
    will reallocate a new backing store. Looks to double the store
    size.
21. `for i, v := range sl { }`: index and value.
22. Map types. `delete`. Two value assignment gives you an `ok`.
23. Anonymous functions, closures.
24. Methods are like `func (v Vertex) Abs() float63 { }`.
25. Methods can have either pointer or value receivers. But only can
    mutate original if pointer receiver. Otherwise causes a copy.
26. Can declare an interface like `type Abser interface { ... }`. An
    interface variable can hold any kind of thing that implements the
    needed methods. Note that `*T` might implement the interface but
    not `T`. I think that if `T` implements then `*T` does too.
27. You can have primitive types implement an interface. An interface
    value is equivalent to `(value, type)`. So `value` can of course
    be a pointer. But it could even be a primitive. (You would need to
    do a typedef though).
28. Note: even if the value is `nil` (because of pointers), `(nil,
    type)` will still try to call the method; and often this should
    gracefully handle nil. Methods can be called on a `nil` value for
    `*T` generally; it's only value access which panics.
    * But you can't call a method on `(nil, nil)`; that is, the zero
      type for an interface. Because then it wouldn't know what to do.
29. `interface{}` is the empty interface and is like "Object", except
    any value can be so assigned. To downcast, you can say:
    `i.(string)`, and this will return an string (or panic). You can
    also say: `s, ok := i.(string)`.
30. Type switches.
31. `Stringer` interface is used by `fmt.Println`. But every type has
    a default enstringination.
32. `error` interface can be anything with an `Error() string`
    method. Includes type aliases of course like `type MyFileError
    int`.
33. They show an `io.Reader` interface. This serializes an
    object. Should return how many bytes written plus `io.EOF` when
    done.
