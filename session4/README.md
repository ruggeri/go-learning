## Interfaces

How to define a regular method. Receiver is defined.
Can define on non-structs via alias.

Pointer recievers can mutate receiver. Typical not to mix having both
value and pointer recievers.

Interfaces: set of methods. Implicitly "inherited". Way it works is
`(value, type)`. When you call the method, it looks at type to know how
to call the method. Default value of an interface variable is
`(nil, nil)`.

Empty interface. Type assertion for concrete types.

Simplest example is `Stringer`.

`error` is an interface. Only method is `Error() string`. Show how you
can use this on an alias with sqrt function.

TODO: Think of some examples of interfaces to show.
