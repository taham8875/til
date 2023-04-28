# Chapter 1: Getting Started

Go is a general-purpose language designed with systems programming in mind. It is strongly typed and garbage-collected and has explicit support for concurrent programming. Programs are constructed from packages, whose properties allow efficient management of dependencies. The existing implementations use a traditional compile/link model to generate executable binaries.

## Machine Setup

### Install Visual Studio Code

You can download Visual Studio Code from [here](https://code.visualstudio.com/).

Install the Go extension for Visual Studio Code.

### Install Go

You can download Go from [here](https://golang.org/dl/).

run the installer, to make sure Go is installed correctly, open a new terminal window and type the following command:

```bash
$ go version
go version go1.20.3 windows/amd64
```

The Go toolset is made up of several different commands and subcommands. You can pull up a list of those commands by typing:

```bash
$ go help
```

## Your First Program

To keep things organized, create a folder named `go-workspace` wherever you like. This is where you will store all your Go code, inside it, create your first Go file, `hello.go`:

You can do this via the file explorerm but i prefere you try doing it via the terminal: (for sake of learning)

```bash
$ mkdir go-workspace
$ cd go-workspace
$ touch hello.go
$ code .
```

The last command will open Visual Studio Code in the current directory.

Now, type the following code into `hello.go`:

```go
package main

import "fmt"

// This is a comment

func main(){
    fmt.Println("Hello world")
}
```

To run the program, type the following command in the terminal (to open the terminal in vscode press `ctrl + ~`):


```bash
$ go run hello.go
Hello world
```

## How to Read Go Program

```go
package main
```

Every Go program must start with a package declaration. Packages are Go's way of organizing and reusing code. There are two types of Go programs: executables and libraries. Executable applications are the kinds of programs that we can run directly from the terminal. Libraries are collections of code that we package together so that we can use them in other programs.

```go
import "fmt"
```

The import keyword is how we include code from other packages to use with our program. The `fmt` package (shorthand for format) implements formatting for input and output. Given what we just learned about packages, what do you think the `fmt` package’s files would contain at the top of them?

Files in the `fmt` package start with package `fmt`

```go
// This is a comment
```

Comments in Go start with `//` and go until the end of the line. Comments are ignored by the Go compiler.

For multi-line comments, we use `/*` and `*/` to start and end the comment block.

```go
/*
This is a multi-line comment
I can write as much as I want here 
*/
```

```go
func main(){
    fmt.Println("Hello world")
}
```

The `func` keyword is used to define functions. A function is a collection of statements that execute sequentially, they are the building blocks of any go program.

All functions start with the keyword func followed by the name of the function (`main`, in this case), a list of zero or more parameters surrounded by parentheses, an optional return type, and a body which is surrounded by curly braces. This function has no parameters, doesn’t return anything, and has only one statement. The name main is special because it’s the function that gets called when you execute the program

Every Go program has a `main` function. In this case, the `main` function calls the `Println` function from the `fmt` package. `Println` is a function that takes a string as an argument and displays it to the screen.

The Println function does the real work in this program. You can find out more about it by typing the following in your terminal:

```bash
$ go doc fmt.Println

func Println(a ...any) (n int, err error)
    Println formats using the default formats for its operands and writes to
    standard output. Spaces are always added between operands and a newline
    is appended. It returns the number of bytes written and any write error
    encountered.

```

# Chapter 2: Types

Go is a statically typed language. This means that variables always have a specific type and that type cannot change. Go is also a strongly typed language, which means that variables retain their type throughout their lifetime. The compiler will throw an error if you try to assign a value of the wrong type to a variable.

I can't explain this better than this one here [What is the difference between a strongly typed language and a statically typed language?](https://stackoverflow.com/questions/2690544/what-is-the-difference-between-a-strongly-typed-language-and-a-statically-typed#:~:text=Strongly%20typed%20means%20that%20there,once%20it%20has%20been%20created.) 

## Numbers

### Integers

I suppose you know what integers are, if not use google or read this section from the book.

Go's integer types are:

- `uint8` (unsigned 8-bit integers)
- `uint16` (unsigned 16-bit integers)
- `uint32` (unsigned 32-bit integers)
- `uint64` (unsigned 64-bit integers)
- `int8` (signed 8-bit integers)
- `int16` (signed 16-bit integers)
- `int32` (signed 32-bit integers)
- `int64` (signed 64-bit integers)


### Floating-Point Numbers

I suppose you know what floating-point numbers are, if not use google or read this section from the book.

Go's floating-point types are :
- `float32` (32-bit floating-point numbers)
- `float64` (64-bit floating-point numbers)

It also has two additional types for representing complex numbers (numbers with imaginary parts): `complex64` and `complex128`. Generally, we should stick with `float64` when working with floating point numbers.

### Arithmetic Operators

Go supports the following arithmetic operators:

- `+` (addition)
- `-` (subtraction)
- `*` (multiplication)
- `/` (division)
- `%` (remainder)

For more math operations, we can use the `math` package.

## Strings

String literals can be created using double quotes `"Hello, World"` or backticks `` `Hello, World` ``. The difference between these is that double-quoted strings cannot contain newlines and they allow special escape sequences. For example, `\n` gets replaced with a newline and `\t` gets replaced with a tab character.

The following are some common operations on strings:

```go
len("Hello, World") // returns 12
"Hello, " + "World" // returns "Hello, World"
"Hello, World"[1] // returns 101 (the ASCII decimal value for "e"), Notice that indexing starts at 0
```

## Booleans

A boolean value is either true or false. Go has a bool type that has two possible values, true and false. Boolean expressions are used in control flow statements to decide whether to execute certain blocks of code or not, depending on whether the expression evaluates to true or false.

Three basic logical operators are used with boolean
values:

- `&&` (AND)
- `||` (OR)
- `!` (NOT)
