`go mod init example.com/hello` - create module ‘hello’. The naming must correspond to the real address where the library can be downloaded, including the name of the library. That will be your code's repository. *pkg.go.dev* - go libraries repository viewer.

**package main func main** - mandatory package and func to run a go program.

`go mod tidy` - register declared modules, download them, synchronize dependencies, and update checksums.

`go run .` - run a program

**import** directive should specify the full module name with this *example.com* path.

`go mod edit -replace example.com/greetings=../greetings` - for production use, you’d fetch the example.com/greetings module from its repository (with a module path that is reflected its published location) where Go tools could find it to download. For development purposes, the example.com/hello module must be adjusted to find the example.com/greetings code on the local file system.

**funcs** and **vars** started with a Capital letter are visible in other modules, lowercase funcs and vars are not.

When you omit a size in the brackets declaring a slice, like this: []string, this tells Go that the size of the array underlying the slice can be dynamically changed.

`go test` - command executes test functions (whose names begin with Test) in test files (whose names end with `_test.go`). You can add the `-v` flag to get verbose output that lists all of the tests and their results.

`go build` - command compiles the packages, along with their dependencies, but it doesn't install the results.

`go install` - command compiles and installs the packages. More of it [here](https://go.dev/doc/tutorial/compile-install).

`go work` - set of commands for working with workspaces. [link](https://go.dev/doc/tutorial/workspaces).**

