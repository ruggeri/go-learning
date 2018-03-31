One workspace at ~/go.

You've got src, bin, and pkg directories.

You often put your code in `src/github.com/ruggeri/my_project_name`.

The package declaration should be:

(1) main if the program is executable
(2) the last part of the directory otherwise. E.g., "crypto/rot13"
    imports a package named "rot13".

Package names don't need to be globally unique.

`go get` fetches code from github.

`go fmt` anywhere formats.

`go dep` is for dependency management. Btw, you prolly want a cmd
package with each file having its own `package main` and main
function. Otherwise you get yelled at for having multiple mains. In
fact, just call them `main.go`.
