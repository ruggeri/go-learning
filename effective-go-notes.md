## `new`

Allocates zeroed space and returns a pointer. You can of course have
constructors.

Literal syntax: `File{ fd: 123, name: 'joey' }`. Can of course
take reference. Safe to take references on the stack.

So I don't see a big reason to ever use `new`?

## `make`

You need it for slices and for maps. You need to initialize
those. It's not enough to new them. Though technically you can do
that:

    ptr := new([]int) // makes the zero slice: nil.
    *ptr = make([]int, 100) // why do this?

You only use `make` for maps/slices/channels; it returns a value and
not a pointer.

## Arrays

Passed by value; length is part of the type. You can pass pointers to
not copy. But then why not just pass a slice which is idiomatic.

You have an `append` function for your convenience. Returns a new
slice.

## Maps

Declared like `m := map[string]int { "abc": 123 }`. Will return zero
type if key is not there. You need to use `ok` if worried. There is a
`delete` function for you.

## Printing

`%v` formats a default value. You can say `%+v` to include the field
names. And `%#v` gives full Go syntax.

You can change `%v` by defining `String() string` for your type.

## Variadic Functions

Functions can have variadic arguments like `min(vals ...int)`.

## Pointer method receivers

You can call any method defined for `T` on either a `T` or `*T`. But
only `*T` values can have a `*T` method called on them. BUT, the
language will take a reference for you. It can only do this when the
value is addressable, but it will.

How often is the value not addressable I wonder?

## Type switches and assertion

Type assertion `str = val.(String)` panics if assertion fails. But you
can also use an `ok`. You can also use a switch:

    switch str := val.(type) {
    case string:
      return str
    case Stringer:
      return str.String()
    }

## Concurrency

* Shows an interesting *semaphore* pattern:

    var sem = make(chan int, MaxOutstanding)

    func handle(r *Request) {
        sem <- 1    // Wait for active queue to drain.
        process(r)  // May take a long time.
        <-sem       // Done; enable next request to run.
    }

    func Serve(queue chan *Request) {
        for {
            req := <-queue
            go handle(req)  // Don't wait for handle to finish.
        }
    }

* A `chan Request` can have the `Request` struct include a channel *by
  which to send the response*. Interesting pattern.

## Panic/Recover

* Shows how you can use `recover` in a deferred function to capture a
  panic. But I don't think I'll want to do something like that, as it
  is not recommended.

## Embedding

* I haven't paid much attention to this. This allows you to do
  something like inheritance. But I don't think you have access to the
  other fields? Like if you have `type Xyz { *A; *B; }`, who is the
  receiver? There's no overriding is there?
