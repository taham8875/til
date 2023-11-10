# Chapter 1 - Go Environment Configuration

Go is a fast compiled, garbage-collected, concurrent system programming language. It has the following advantages:

- Compile large projects quickly
- Easy to reason about
- Static typing
- Garbage collection
- Multi-core concurrency

# Package Directory

Create package source file in the directory `$GOPATH/src/mymath/sqrt.go` (So my math is the package name) and type the following code:

```go
package mymath // package name

func Sqrt(x float64) float64 { // Sqrt first letter is capitalized, so it is exported and can be used by other packages
    z := 0.0
    for i := 0; i < 1000; i++ {
        z -= (z*z - x) / (2 * x)
    }
    return z
}
```

## Compile and Install

```bash
go install mymath
```

This will create a file `$GOPATH/pkg/${GOOS}_${GOARCH}/mymath.a` which is the compiled package file.

The extension of the package file is `.a`, it is the binary file of the package. It is not executable, but it can be imported by other packages.

We need to create a new application to use the package.

Create a new file `$GOPATH/src/mymathapp/main.go` and type the following code:

```go
package main

import (
    "fmt"
    "mymath"
)

func main() {
    fmt.Printf("Hello, world. Sqrt(2) = %v\n", mymath.Sqrt(2))
}
```

Then compile and run the application:

```bash
$ go run mymathapp
Hello, world. Sqrt(2) = 1.414213562373095
```

## Install remote packages

To install remote packages

```bash
go get github.com/astaxie/beedb
```

This will download the package source code into the `src` directory and run `go install` to compile and install the package.

To update the package

```bash
go get -u github.com/astaxie/beedb
```

To use the package

```go
import (
    "github.com/astaxie/beedb"
)
```

# Go Commands

## Go Commands

To see the list of go commands

```bash
$ go
Go is a tool for managing Go source code.

Usage:

        go <command> [arguments]

The commands are:

        bug         start a bug report
        build       compile packages and dependencies
        clean       remove object files and cached files
        doc         show documentation for package or symbol
        env         print Go environment information
        fix         update packages to use new APIs
        fmt         gofmt (reformat) package sources
        generate    generate Go files by processing source
        get         add dependencies to current module and install them
        install     compile and install packages and dependencies      
        list        list packages or modules
        mod         module maintenance
        work        workspace maintenance
        run         compile and run Go program
        test        test packages
        tool        run specified go tool
        version     print Go version
        vet         report likely mistakes in packages

Use "go help <command>" for more information about a command.

Additional help topics:

        buildconstraint build constraints
        buildmode       build modes
        c               calling between Go and C
        cache           build and test caching
        environment     environment variables
        filetype        file types
        go.mod          the go.mod file
        gopath          GOPATH environment variable
        gopath-get      legacy GOPATH go get
        goproxy         module proxy protocol
        importpath      import path syntax
        modules         modules, module versions, and more
        module-get      module-aware go get
        module-auth     module authentication using go.sum
        packages        package lists and patterns
        private         configuration for downloading non-public code
        testflag        testing flags
        testfunc        testing functions
        vcs             controlling version control with GOVCS

Use "go help <topic>" for more information about that topic.

```

## go build

This command is used for compiling tests, packages and its dependencies.

- If package is not the `main` package, it will not generate executable file. If you need it run `go install` to generate the executable file.
- If package is the `main` package, it will generate executable file in the current directory. If you want to generate it in `$GOPATH/bin` directory, run `go install` instead or run `go build -o ${PATH_HERE}` to specify the output directory.
- If there are many files and you want to compile one of them, you can specify the file name after the `go build` command.
- To compile all files in the current directory, run `go build`
- You can assign the name of the executable file by using `-o` option. For example, `go build -o myapp` will generate `myapp` executable file.

(According to The Go Programming Language Specification, package names should be the name after the word package in the first line of your source files. It doesn't have to be the same as the folder name, and the executable file name will be your folder name by default.)

- Go build ignores the files with the suffix of `_` or `.`
-If you want to have different source files for different file systems, name the files with the os as suffix like `file_windows.go` or `file_linux.go`. Then run `go build` to compile the files and it will choose the right file to compile according to the current file system.

## go clean

This command is used for cleaning the compiled files.

## go fmt and

Although it is not used very much because the IDEs can do it automatically, `go fmt` is used to format the source code. It will format the source code according to the Go programming language specification.

```bash
go fmt <file_name>
```

## go get

This command is used for downloading and installing packages and dependencies.

```bash
go get <package_name>
```

Make sure that git is installed on your computer.

## go install

This command is used for compiling and generating packages and dependencies.

```bash
go install <package_name>
```

## go run

This command is used for compiling and running the source code in one step.

```bash
go run <file_name>
```

## go test

This command loads all files with the suffix of `_test.go` and runs the test functions in these files.

## go doc

This command is used for generating documentation for Go source code.

For example, if you want to read the documentation for `net/http` package, run the following command:

```bash
go doc net/http
```

If you want to read the documentation in the browser, run the following command:

```bash
go doc -http=:6060
```

Then open the browser and type `http://localhost:6060` to see the documentation.
