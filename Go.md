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





In Go, `:=` is for declaration + assignment, whereas `=` is for assignment only.

For example, `var foo int = 10` is the same as `foo := 10`.





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



### Type Assersion





### Structs



A **struct** is a collection of fields/properties. You can define new types as structs or [interfaces](https://www.educative.io/collection/page/10370001/6199152924950528/5454560214646784/).



struct declaration:



```go

package main

import (
	"fmt"
	"time"
)

type Bootcamp struct {
	// Latitude of the event
	Lat float64
	// Longitude of the event
	Lon float64
	// Date of the event
	Date time.Time
}

func main() {
	fmt.Println(Bootcamp{
		Lat:  34.012836,
		Lon:  -118.495338,
		Date: time.Now(),
	})
}


Output:

{34.012836 -118.495338 2022-05-15 03:05:03.477758773 +0000 UTC}

```



Declaration of struct literals:



```go

package main

import "fmt"

type Point struct {
	X, Y int
}

var (
	p = Point{1, 2}  // has type Point
	q = &Point{1, 2} // has type *Point
	r = Point{X: 1}  // Y:0 is implicit
	s = Point{}      // X:0 and Y:0
)

func main() {
	fmt.Println(p, q, r, s)
}


Output:
{1 2} &{1 2} {1 0} {0 0}


```



Accessing fields using the dot notation:



```go

package main

import (
	"fmt"
	"time"
)

type Bootcamp struct {
	Lat, Lon float64
	Date     time.Time
}

func main() {
	event := Bootcamp{
		Lat: 34.012836,
		Lon: -118.495338,
	}
	event.Date = time.Now()
	fmt.Printf("Event on %s, location (%f, %f)",
		event.Date, event.Lat, event.Lon)

}


Output:

Event on 2022-05-15 03:07:47.185464322 +0000 UTC, location (34.012836, -118.495338)

```





### Initialising

Using the `new` key expression



```go
x := new(int)
```



Note that following expressions using `new` and an empty struct literal are equivalent and result in the same kind of allocation/initialization:



```go

package main

import (
	"fmt"
)

type Bootcamp struct {
	Lat float64
	Lon float64
}

func main() {
	x := new(Bootcamp)
	// To get the pointer of a value, use the `&` symbol in front of the value; 
	y := &Bootcamp{}
	// To dereference a pointer, use the `*` symbol.
	fmt.Println(*x == *y)
}

Output
true

```



### Composition vs Inheritance


Coming from an [OOP](http://en.wikipedia.org/wiki/Object-oriented_programming) background a lot of us are used to inheritance, something that isn’t supported by Go. Instead you have to think in terms of *composition* and *interfaces*.



```go

package main

import "fmt"

type User struct {
	Id       int
	Name     string
	Location string
}

//type Player with one additional attribute

type Player struct {
	Id       int  
	Name     string
	Location string
	GameId	 int
}

func main() {
	p := Player{}
	p.Id = 42
	p.Name = "Matt"
	p.Location = "LA"
	p.GameId = 90404
	fmt.Printf("%+v", p) // the value in a default format when printing structs,
                        // the plus flag (%+v) adds field names
}

Output:

{Id:42 Name:Matt Location:LA GameId:90404}

```



```go

type User struct {
	Id             int
	Name, Location string
}

type Player struct {
	User //user will contain all the required attributes
	GameId int
}
```



We can initialize a new variable of type `Player` two different ways.



1. Using the dot notation to set the fields:

```go

package main

import "fmt"

type User struct {
	Id             int
	Name, Location string
}

type Player struct {
	User
	GameId int
}

func main() {
	p := Player{} //initializing
	p.Id = 42
	p.Name = "Matt"
	p.Location = "LA"
	p.GameId = 90404
	fmt.Printf("%+v", p)
}

Output:
{User:{Id:42 Name:Matt Location:LA} GameId:90404}

```



2. The other option is to use a struct literal:

```go

package main

import "fmt"

type User struct {
	Id             int
	Name, Location string
}

type Player struct {
	User
	GameId int
}

func main() {
  
  //we can’t just pass the composed fields. We instead need to pass the types composing the struct.
	p := Player{
		User{Id: 42, Name: "Matt", Location: "LA"},
		90404,
	}
	fmt.Printf(
		"Id: %d, Name: %s, Location: %s, Game id: %d\n",
		p.Id, p.Name, p.Location, p.GameId)
	// Directly set a field defined on the Player struct
	p.Id = 11
	fmt.Printf("%+v", p)
}


Output:
Id: 42, Name: Matt, Location: LA, Game id: 90404
{User:{Id:11 Name:Matt Location:LA} GameId:90404}


```



```go

package main

import "fmt"

type User struct {
	Id             int
	Name, Location string
}

???
func (u *User) Greetings() string {
	return fmt.Sprintf("Hi %s from %s",
		u.Name, u.Location)
}

type Player struct {
	User
	GameId int
}

func main() {
	p := Player{}
	p.Id = 42
	p.Name = "Matt"
	p.Location = "LA"
	fmt.Println(p.Greetings())
}

```



```go

package main

import (
	"log"
	"os"
)

type Job struct {
	Command string
	Logger  *log.Logger
  // Or We can skip defining the field for our logger and now all the methods available on a pointer to log.Logger are available from our struct:
 	*log.Logger
}

func main() {
	job := &Job{"demo", log.New(os.Stdout, "Job: ", log.Ldate)}
	// same as
	// job := &Job{Command: "demo",
	//            Logger: log.New(os.Stderr, "Job: ", log.Ldate)}
	job.Logger.Print("test")
}


Output
Job: 2022/05/15 test

```



### Exercise on Composition



```go

package main
import "fmt"
import "encoding/json"


type User struct {
	Id             int
	Name, Location string
}

func (u User) Greetings() string {
	return fmt.Sprintf("Hi %s from %s",
                     u.Name, u.Location)
}

type Player struct {
	u User
	GameId int
}

func GreetingsForPlayer(p Player) string{
	return p.u.Greetings();
}

```



## Collection types



### Arrays

declares a variable `a` as an array of ten integers.

```go
var a [10]int
```



```go

package main

import "fmt"

func main() {
	var a [2]string
	a[0] = "Hello"
	a[1] = "World"
	fmt.Println(a[0], a[1])
	fmt.Println(a)

	primes := [6]int{2, 3, 5, 7, 11, 13}  // array of prime numbers of size 6 
	fmt.Println(primes)
}

Output:
Hello World
[Hello World]
[2 3 5 7 11 13]

```





Finally, you can use an **ellipsis** to use an implicit length when you pass the values:



```go

package main

import "fmt"

func main() {
	a := [...]string{"hello", "world!"}
	fmt.Printf("%q", a)
}

Output:
["hello" "world!"]

```



**Printing arrays**

Note how we used the [`fmt` package](http://golang.org/pkg/fmt/) using `Printf` and used the `%q` “verb” to print each element quoted.

If we had used `Println` or the `%s` verb, we would have had a different result:



```go

package main

import "fmt"

func main() {
	a := [2]string{"hello", "world!"}
	fmt.Println(a)
	// [hello world!]
	fmt.Printf("%s\n", a)
	// [hello world!]
	fmt.Printf("%q\n", a)
	// ["hello" "world!"]
}

Output:
[hello world!]
[hello world!]
["hello" "world!"]

```



**Multi-dimensional arrays**



```go

package main

import "fmt"

func main() {
	var a [2][3]string
	for i := 0; i < 2; i++ {
		for j := 0; j < 3; j++ {
			a[i][j] = fmt.Sprintf("row %d - column %d", i+1, j+1)
		}
	}
	fmt.Printf("%q", a)
	// [["row 1 - column 1" "row 1 - column 2" "row 1 - column 3"]
	//  ["row 2 - column 1" "row 2 - column 2" "row 2 - column 3"]]
}

```



```go

package main

func main() {
  
  	// Trying to access or set a value at an index that doesn’t exist will prevent your program from compiling
    var a [2]string
    a[3] = "Hello"
}

Output
# command-line-arguments
./main.go:5: invalid array index 3 (out of bounds for 2-element array)

```



### Slices

Slices wrap arrays to give a more general, powerful, and convenient interface to sequences of data. 



```go

package main

import "fmt"

func main() {
    p := []int{2, 3, 5, 7, 11, 13}
    fmt.Println(p)
    // [2 3 5 7 11 13]
}

```



Slicing a slice

```go

package main

import "fmt"

func main() {
	mySlice := []int{2, 3, 5, 7, 11, 13}
	fmt.Println(mySlice)
	// [2 3 5 7 11 13]

	fmt.Println(mySlice[1:4])
	// [3 5 7]

	// missing low index implies 0
	fmt.Println(mySlice[:3])
	// [2 3 5]

	// missing high index implies len(s)
	fmt.Println(mySlice[4:])
	// [11 13]
}

```



Slices hold references to an underlying array, and if you assign one slice to another, both refer to the same array. If a function takes a slice argument, changes it makes to the elements of the slice will be visible to the caller, analogous to passing a pointer to the underlying array.



```go

package main
import "fmt"

func main() {
	names := [4]string{
		"John",
		"Paul",
		"George",
		"Ringo",
	}
	fmt.Println(names) // [John Paul George Ringo]

	a := names[0:2]    //slice   [John Paul]
	b := names[1:3]      //slice b [Paul George]
	fmt.Println(a, b) // 

	b[0] = "XXX"      // value at zeroth index of slice b changed
	fmt.Println(a, b) //  [John XXX] [XXX George]
	fmt.Println(names) // [John XXX George Ringo]
}

```



**Making slices**

```go

package main

import "fmt"

func main() {
	cities := make([]string, 3)
	cities[0] = "Santa Monica"
	cities[1] = "Venice"
	cities[2] = "Los Angeles"

	fmt.Printf("%q", cities)
	// ["Santa Monica" "Venice" "Los Angeles"]
}

```



Appending to a slice

```go

package main

import "fmt"

func main() {
	cities := []string{}
	cities = append(cities, "San Diego")
	fmt.Println(cities)
	// [San Diego]
}


package main

import "fmt"

func main() {
	cities := []string{}
	cities = append(cities, "San Diego", "Mountain View")
	fmt.Printf("%q", cities)
	// ["San Diego" "Mountain View"]
}


// And you can also append a slice to another using an ellipsis:

package main

import "fmt"

func main() {
	cities := []string{"San Diego", "Mountain View"}
	otherCities := []string{"Santa Monica", "Venice"}
	cities = append(cities, otherCities...)
	fmt.Printf("%q", cities)
	// ["San Diego" "Mountain View" "Santa Monica" "Venice"]
}

```



**Length**

```go

package main

import "fmt"

func main() {
	cities := []string{
		"Santa Monica",
		"San Diego",
		"San Francisco",
	}
	fmt.Println(len(cities))
	// 3
	countries := make([]string, 42)
	fmt.Println(len(countries))
	// 42
}

```



**Nil slices**

The zero value of a slice is nil. A nil slice has a *length* and *capacity* of **0**.

```go

package main

import "fmt"

func main() {
    var z []int
    fmt.Println(z, len(z), cap(z))
    // [] 0 0
    if z == nil {
        fmt.Println("nil!")
    }
    // nil!
}

```













































































































