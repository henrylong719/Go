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



The path `github.com/mattetti/goRailsYourself/crypto` basically tells the compiler to import the *crypto package* available. It doesn’t mean that the compiler will automatically pull down the repository, so where does it find the code?

```bash
$ go get github.com/mattetti/goRailsYourself/crypto
```



### Functions and Return values

When declaring functions, the type comes after the variable *name* in the inputs.

The return type(s) are then specified after the function name and inputs, before writing the definition. Functions can be defined to return any number of values and each of them are put here.



```go

package main
import "fmt"

func add(x int, y int) int {
    return x + y
}

func main() {
    fmt.Println(add(42, 13))
}

// we only declare one type that applies to both.

package main
import "fmt"

func add(x, y int) int {
    return x + y
}

func main() {
    fmt.Println(add(42, 13))
}

```



**Return values**

```go

package main

import "fmt"

func location(city string) (string, string) {
	var region string
	var continent string

	switch city {
	case "Los Angeles", "LA", "Santa Monica":
		region, continent = "California", "North America"
	case "New York", "NYC":
		region, continent = "New York", "North America"
	default:
		region, continent = "Unknown", "Unknown"
	}
	return region, continent
}

func main() {
	region, continent := location("Santa Monica")
	fmt.Printf("Matt lives in %s, %s", region, continent)
}

```



**Named Results**

Functions take parameters. In Go, functions can return multiple “result parameters”, not just a single value. They can be named and act just like variables.

If the result parameters are named, a return statement without arguments returns the current values of the results.



```go

package main
import "fmt"

func location(city string) (region, continent string) {
	switch city {
	case "Los Angeles", "LA", "Santa Monica":
		region, continent = "California", "North America"
	case "New York", "NYC":
		region, continent = "New York", "North America"
	default:
		region, continent = "Unknown", "Unknown"
	}
	return //returning region and continent
}

func main() {
	region, continent := location("Santa Monica")
	fmt.Printf("Matt lives in %s, %s", region, continent)
}

```



### Go and Pointers

Go has *pointers*, but no pointer arithmetic. *Struct* fields can be accessed through a struct pointer. The indirection through the pointer is **transparent** (you can directly call fields and methods on a pointer).

Note that by *default* Go passes arguments by value (copying the arguments), if you want to pass the arguments by reference, you need to pass pointers (or use a structure using reference values like [slices](https://www.educative.io/collection/page/10370001/6199152924950528/6247318835691520/) and [maps](https://www.educative.io/collection/page/10370001/6199152924950528/5113733084872704/).

To get the pointer of a value, use the `&` symbol in front of the value; to dereference a pointer, use the `*` symbol.

Methods are often defined on pointers and not values (although they can be defined on both), so you will often store a pointer in a variable as in the example below:



```go
client := &http.Client{}
resp, err := client.Get("http://gobootcamp.com")
```



### Mutability



In Go, only *constants* are **immutable**. However, because arguments are passed by value, a function receiving a value argument and mutating it, won’t mutate the original value.



```go

package main
import "fmt"

type Artist struct {
	Name, Genre string
	Songs       int
}

func newRelease(a Artist) int { //passing an Artist by value
	a.Songs++
	return a.Songs
}

func main() {
	me := Artist{Name: "Matt", Genre: "Electro", Songs: 42}
	fmt.Printf("%s released their %dth song\n", me.Name, newRelease(me))
	fmt.Printf("%s has a total of %d songs", me.Name, me.Songs)
}


Output:
Matt released their 43th song
Matt has a total of 42 songs

```



As you can see the total amount of songs on the `me` variable’s value wasn’t changed. To mutate the passed value, we need to pass it by reference, using a pointer.



```go

package main
import "fmt"

type Artist struct {
	Name, Genre string
	Songs       int
}

func newRelease(a *Artist) int { //passing an Artist by reference
	a.Songs++
	return a.Songs
}

func main() {
	me := &Artist{Name: "Matt", Genre: "Electro", Songs: 42}
	fmt.Printf("%s released their %dth song\n", me.Name, newRelease(me))
	fmt.Printf("%s has a total of %d songs", me.Name, me.Songs)
}

```



The only change between the two versions is that `newRelease` takes a pointer to an `Artist` value and when I initialize our `me` variable, I used the `&` symbol to get a pointer to the value.





## Types



### Basic Types

```go

package main

import (
	"fmt"
	"math/cmplx"
)

var (
	goIsFun bool       = true //declaring a variable of type bool
	maxInt  uint64     = 1<<64 - 1 //declaring a variable of type uint64
	complex complex128 = cmplx.Sqrt(-5 + 12i) //declaring a variable of type complex128
)

func main() {
	const f = "%T(%v)\n"
	fmt.Printf(f, goIsFun, goIsFun)
	fmt.Printf(f, maxInt, maxInt)
	fmt.Printf(f, complex, complex)
}

```





### Type Conversion

```go
var i int = 42
var f float64 = float64(i)
var u uint = uint(f)

```



These can be put more simply:



```go
i := 42
f := float64(i)
u := uint(f)

```

















































































