# Go



An Introduction to Programming in Go (educative)



## The Basics



### Variables & inferred typinig



**Variables Declaration**

The `var` statement declares a list of variables.

```go

var (
		name			string
  	age				int
  	location	string
)

// or

var (
		name, location 	string
  	age							int
)

// declare one by one:

var name			string
var age 			int
var location  string

```



**Variable Initialization**

```go

var (
	name     string = "Prince Oberyn"
	age      int    =  32
	location string = "Dorne"
)

// or inferred typing

var (
	name     = "Prince Oberyn"
	age      =  32
	location = "Dorne"
)

var (
	name, location, age = "Prince Oberyn", "Dorne", 32
)

```



 Inside a function the `:=` short assignment statement can be used in place of `var` declaration with implicit type.



```go

package main
import "fmt"

func main() {
	name, location := "Prince Oberyn", "Dorne"
	age := 32
	fmt.Printf("%s age %d from %s ", name, age, location)
}

```



A variable can contain any type, including functions:

```go

func main() {
	action := func() { //action is a variable that contains a function
		//doing something
	}
	action()
}

```



https://go.dev/blog/declaration-syntax



### Constant

Constants are declared like variables, but with the `const` keyword.

Constants can only be a **character**, **string**, **boolean**, or **numeric values** and cannot be declared using the `:=` syntax. An *untyped* constant takes the type needed by its context.



```go

const Pi = 3.14
const (
        StatusOK                   = 200
        StatusCreated              = 201
        StatusAccepted             = 202
        StatusNonAuthoritativeInfo = 203
        StatusNoContent            = 204
        StatusResetContent         = 205
        StatusPartialContent       = 206
)

```



```go

package main
import "fmt"

const (
	Pi    = 3.14
	Truth = false
	Big   = 1 << 62
	Small = Big >> 61
)

func main() {
	const Greeting = "ハローワールド" //declaring a constant
	fmt.Println(Greeting)
	fmt.Println(Pi)
	fmt.Println(Truth)
	fmt.Println(Big)
}


Output

ハローワールド
3.14
false
4611686018427387904


```



### Printing

**Printing Constants and Variables**

While you can print the value of a variable or constant using the built-in `print` and `println` functions, the more idiomatic and flexible way is to use the [`fmt` package](http://golang.org/pkg/fmt/)



```go

package main
import "fmt"

func main() {
	cylonModel := 6
  // prints the passed in variables’ values and appends a newline.
	fmt.Println(cylonModel)
}

```



```go

package main
import "fmt"

func main() {
	name := "Caprica-Six"
	aka := fmt.Sprintf("Number %d", 6)
  //print one or multiple values using a defined format specifier (%d, %s).
	fmt.Printf("%s is also known as %s",
		name, aka) //printing variables within the string
}

// Output:
// Caprica-Six is also known as Number 6

```



https://pkg.go.dev/fmt#hdr-Printing



### Packages and Imports

**Packages**

Every Go program is made up of packages. Programs start running in package `main`.

```go

package main
import "fmt"

func main() {
	fmt.Printf("Hello, World!\n")
}

```



If you are writing an executable code (versus a library), then you need to define a `main` package and a `main()` function which will be the entry point to your software.

By convention, the package name is the same as the last element of the import path. For instance, the “math/rand” package comprises files that begin with the statement package rand.



**Import Statement**

```go

import "fmt"
import "math/rand"

// or

import (
    "fmt"
    "math/rand"
)

```



```go

package main

import (
	"fmt"
	"math/rand"
)

func main() {
	fmt.Println("My favorite number is", rand.Intn(10))
}

// Output:
My favorite number is 1


```



```go


package main

import (
	"fmt"
	"math"
)

func main() {
	fmt.Printf("Now you have %g problems.", math.Sqrt(7))
}

// Output:
Now you have 2.6457513110645907 problems.

```



Usually, nonstandard lib packages are namespaced using a *web url*. For instance, there is some Rails logic ported to GO, including the cryptography code used in *Rails 4*. The source code is hosted on **GitHub** containing a few packages, in the following repository: http://github.com/mattetti/goRailsYourself

To import the crypto package, you would need to use the following import statement:

```go
import "github.com/mattetti/goRailsYourself/crypto"
```











