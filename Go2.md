# Go



An Introduction to Programming in Go (educative)



## Collection types



### Range in for loops

```go

package main

import "fmt"

var pow = []int{1, 2, 4, 8, 16, 32, 64, 128}

func main() {
    for i, v := range pow { //loops over the length of pow
        fmt.Printf("2**%d = %d\n", i, v)
    }
}

```



You can skip the index or value by assigning to `_`. If you only want the index, drop the “, value” entirely.



```go

package main

import "fmt"

func main() {
    pow := make([]int, 10)
    for i := range pow {
        pow[i] = 1 << uint(i)
    }
    for _, value := range pow {
        fmt.Printf("%d\n", value)
    }
}

Output:

1
2
4
8
16
32
64
128
256
512

```



**break and continue**

```go

package main

import "fmt"

func main() {
	pow := make([]int, 10)
	for i := range pow {
		pow[i] = 1 << uint(i)
		if pow[i] >= 16 {
			break //stop iterating over pow when it reaches 16
		}
	}
	fmt.Println(pow)
	// [1 2 4 8 16 0 0 0 0 0]
}


package main

import "fmt"

func main() {
	pow := make([]int, 10)
	for i := range pow {
		if i%2 == 0 {
			continue //skip each even index of pow
		}
		pow[i] = 1 << uint(i)
	}
	fmt.Println(pow)
	// [0 2 0 8 0 32 0 128 0 512]
}

```



**Range and Maps**

```go

package main

import "fmt"

func main() {
	cities := map[string]int{
		"New York":    8336697,
		"Los Angeles": 3857799,
		"Chicago":     2714856,
	}
	for key, value := range cities { //for each key-value pair in cities
		fmt.Printf("%s has %d inhabitants\n", key, value)
	}
}

Output:

New York has 8336697 inhabitants
Los Angeles has 3857799 inhabitants
Chicago has 2714856 inhabitants


```



### Maps in Go



```go

package main

import "fmt"

func main() {
	celebs := map[string]int{ //mapping strings to integers
		"Nicolas Cage":       50,
		"Selena Gomez":       21,
		"Jude Law":           41,
		"Scarlett Johansson": 29,
	}

	fmt.Printf("%#v", celebs)
}

Output:

map[string]int{"Nicolas Cage":50, "Selena Gomez":21, "Jude Law":41, "Scarlett Johansson":29}

```



```go

package main

import "fmt"

type Vertex struct {
	Lat, Long float64
}

var m map[string]Vertex

func main() {
	m = make(map[string]Vertex)
	m["Bell Labs"] = Vertex{40.68433, -74.39967} //assignment
	fmt.Println(m["Bell Labs"])
}

Output:
{40.68433 -74.39967}




package main

import "fmt"

type Vertex struct {
	Lat, Long float64
}

var m = map[string]Vertex{
	"Bell Labs": {40.68433, -74.39967},
	// same as "Bell Labs": Vertex{40.68433, -74.39967}
	"Google": {37.42202, -122.08408},
}

func main() {
	fmt.Println(m)
}

Output
map[Bell Labs:{40.68433 -74.39967} Google:{37.42202 -122.08408}]

```



```go

package main

import "fmt"

type Vertex struct {
	Lat, Long float64
}

var m = map[string]Vertex{
	"Bell Labs": {40.68433, -74.39967},
	// same as "Bell Labs": Vertex{40.68433, -74.39967}
	"Google": {37.42202, -122.08408},
}

func main() {
  m["Splice"] = Vertex{34.05641, -118.48175} //inserting a new (key,value) here
	fmt.Println(m["Splice"]) // {34.05641 -118.48175}
	delete(m, "Splice")    //deleting the element 
	fmt.Printf("%v\n", m) // map[Bell Labs:{40.68433 -74.39967} Google:{37.42202 -122.08408}]

	name, ok := m["Splice"]      //checks to see if element is present

	// key 'Splice' is present?: false - value: {0 0}
	fmt.Printf("key 'Splice' is present?: %t - value: %v\n", ok, name) 
	name, ok = m["Google"]
	// key 'Google' is present?: true - value: {37.42202 -122.08408}
	fmt.Printf("key 'Google' is present?: %t - value: %v\n", ok, name)
}

```



The code below should return a map of the counts of each “word” in the string `s`.



```go

package main

import (
   "strings"
   "fmt"
   "encoding/json"
   "strconv"
)

func WordCount(s string) map[string]int {
   words := strings.Fields(s)
   count := map[string]int{}
   for _, word := range words{
      count[word]++
   }
  return count  //returning an empty map, modify the statement to return the correct answer
}

```



## Control flow



### If Statement

Example of an `if` short statement with a variable declared within the statement:



```go

package main

import (
	"fmt"
	"math"
)

func pow(x, n, lim float64) float64 {
	if v := math.Pow(x, n); v < lim {
		return v
	}
	return lim
}

func main() {
	fmt.Println(
		pow(3, 2, 10),
		pow(3, 3, 20),
	)
}

Output
9 20

```



### For Loop

`for` loop syntax

```go

package main

import "fmt"

func main() {
	sum := 0
	for i := 0; i < 10; i++ {
		sum += i
	}
	fmt.Println(sum)
}

Output
45

```



For loop without statements

```go

package main

import "fmt"

func main() {
	sum := 1
	for ; sum < 1000; { //iterate as long as sum<1000
		sum += sum
	}
	fmt.Println(sum)
}

Output
1024

```



For loop as an alternative to While

```go

package main

import "fmt"

func main() {
	sum := 1
	for sum < 1000 {
		sum += sum
	}
	fmt.Println(sum)
}

Output
1024

```



Inifinite loop

```go

package main

func main() {
	for {
  // do something in a loop forever
  }
}

```



### Switch Case Statement

```go

package main

import "fmt"

func main() {
	score := 7
	switch score {
	case 0, 1, 3:
    // no need break like other languages
		fmt.Println("Terrible")
	case 4, 5:
		fmt.Println("Mediocre")
	case 6, 7:
		fmt.Println("Not bad")
	case 8, 9:
		fmt.Println("Almost perfect")
	case 10:
		fmt.Println("hmm did you cheat?")
	default: //to be executed if no other case matches
		fmt.Println(score, " off the chart")
	}
}

Output
Not bad

```



You can execute all the following statements after a match using the `fallthrough` statement:

```go

package main

import "fmt"

func main() {
	n := 4
	switch n {
	case 0:
		fmt.Println("is zero")
		fallthrough //if case matches, all following conditions will be executed as well
	case 1:
		fmt.Println("is <= 1")
		fallthrough
	case 2:
		fmt.Println("is <= 2")
		fallthrough
	case 3:
		fmt.Println("is <= 3")
		fallthrough
	case 4:
		fmt.Println("is <= 4")
		fallthrough
	case 5:
		fmt.Println("is <= 5")
		fallthrough
	case 6:
		fmt.Println("is <= 6")
		fallthrough
	case 7:
		fmt.Println("is <= 7")
		fallthrough
	case 8:
		fmt.Println("is <= 8")
		fallthrough
	default:
		fmt.Println("Try again!")
	}
}

Output
is <= 4
is <= 5
is <= 6
is <= 7
is <= 8
Try again!

```



You can use a `break` statement inside your matched statement to exit the switch processing:



```go

package main

import (
	"fmt"
	"time"
)

func main() {
	n := 1
	switch n {
	case 0:
		fmt.Println("is zero")
		fallthrough
	case 1:
		fmt.Println("<= 1")
		fallthrough
	case 2:
		fmt.Println("<= 2")
		fallthrough
	case 3:
		fmt.Println("<= 3")
		if time.Now().Unix()%2 == 0 {
			fmt.Println("un pasito pa lante maria")
			break //execution stops here if this case matches i.e. no other case will be checked
		}
		fallthrough
	case 4:
		fmt.Println("<= 4")
		fallthrough
	case 5:
		fmt.Println("<= 5")
	}
}


```



## Methods

A frequently asked question is “what is the difference between a function and a method”. A method is a function that has a defined receiver, in OOP terms, a method is a function on an instance of an object.



Go does **not** have classes. However, you can define methods on struct types.



```go

package main

import (
  "fmt"
)

type User struct {
  FirstName, LastName string
}

func (u User) Greeting() string {
  return fmt.Sprintf("Dear %s %s", u.FirstName, u.LastName)
}


func main(){
  u := User{"Matt", "Aimonetti"}
	fmt.Println(u.Greeting())
}

Output
Dear Matt Aimonetti


```



Methods can be defined on any file in the package, but my recommendation is to organize the code as shown below:

```go

package main

// list of packages to import
import (
	"fmt"
)

// list of constants
const (
	ConstExample = "const before vars"
)

// list of variables
var (
	ExportedVar    = 42
	nonExportedVar = "so say we all"
)

// Main type(s) for the file,
// try to keep the lowest amount of structs per file when possible.
type User struct {
	FirstName, LastName string
	Location            *UserLocation
}

type UserLocation struct {
	City    string
	Country string
}

// List of functions
func NewUser(firstName, lastName string) *User {
	return &User{FirstName: firstName,
		LastName: lastName,
		Location: &UserLocation{
			City:    "Santa Monica",
			Country: "USA",
		},
	}
}

// List of methods
func (u *User) Greeting() string {
	return fmt.Sprintf("Dear %s %s", u.FirstName, u.LastName)
}

func main() {
  us:=User {FirstName: "Matt",
		LastName: "Damon",
		Location: &UserLocation{
			City:    "Santa Monica",
			Country: "USA",}}
  fmt.Println(us.Greeting())
}


Output
Dear Matt Damon


```



***what is \* in a Golang type declaration?\***

> “ * ” says, “you are declaring that this variable holds a memory address to a string, or int or whatever type follows “ * ”. For example, “var a *int” declares that the variable “a” holds a memory address(pointer) to an int datatype.



***what is \* everywhere else in Golang?\***

> ”*” says “give me whatever the variable that follows is pointing to”. So your variable better be an actual memory address, otherwise you’ll get an error. It tells the system to use the value as a pointer and return whatever is at that address.



### Type Aliasing

To define methods on a type you don’t “own”, you need to define an *alias* for the type you want to extend.



```go

package main

import (
	"fmt"
	"strings"
)

type MyStr string //using MyStr as an alias for type string

func (s MyStr) Uppercase() string {
	return strings.ToUpper(string(s))
}

func main() {
	fmt.Println(MyStr("test").Uppercase())
}

Output
TEST


```



```go

package main

import (
    "fmt"
    "math"
)

type MyFloat float64   //using MyFloat as an alias for type float64

func (f MyFloat) Abs() float64 {
    if f < 0 {
        return float64(-f)
    }
    return float64(f)
}

func main() {
    f := MyFloat(-math.Sqrt2)
    fmt.Println(f.Abs())
}


Output
1.4142135623730951


```



### Method Receivers

Methods can be associated with a *named* type (`User` for instance) or a *pointer* to a named type (`*User`).



There are two reasons to use a *pointer receiver*. 

1. To avoid copying the value on each method call (more efficient if the value type is a large struct). 

2. The method can modify the value that its receiver points to.

   

 The previous `User` example would have been better written as follows:

```go

package main

import (
	"fmt"
)

type User struct {
	FirstName, LastName string
}

func (u *User) Greeting() string { //pointers
	return fmt.Sprintf("Dear %s %s", u.FirstName, u.LastName)
}

func main() {
	u := &User{"Matt", "Aimonetti"}
	fmt.Println(u.Greeting())
}

```



Remember that Go passes everything by value, meaning that when `Greeting()` is defined on the value type, every time you call `Greeting()`, you are copying the `User` struct. Instead when using a pointer, only the pointer is copied (cheap).



```go

package main

import (
	"fmt"
	"math"
)

type Vertex struct {
	X, Y float64
}

func (v *Vertex) Scale(f float64) {
	v.X = v.X * f //v will be modified directly here
	v.Y = v.Y * f
}

func (v *Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func main() {
	v := &Vertex{3, 4}
	v.Scale(5)
	fmt.Println(v, v.Abs())
}

Output
&{15 20} 25

```



In the example above, `Abs()` could be defined on the value type or the pointer since the method doesn’t modify the receiver value (the vertex). However `Scale()` has to be defined on a pointer since it does modify the receiver. `Scale()` resets the values of the `X` and `Y` fields.



## Interface



An **interface** type is defined by a set of methods. A value of interface type can hold any value that implements those methods. Interfaces increase the flexibility as well as the scalability of the code. Hence, it can be used to achieve polymorphism in Golang. Interface does not require a particular type, specifying that only some behavior is needed, which is defined by a set of methods.



```go

package main

import (
	"fmt"
)

type User struct {
	FirstName, LastName string
}

func (u *User) Name() string {
	return fmt.Sprintf("%s %s", u.FirstName, u.LastName)
}

type Namer interface {
  Name() string //The Namer interface is defined by the Name() method
}

func Greet(n Namer) string {
	return fmt.Sprintf("Dear %s", n.Name())
}

func main() {
	u := &User{"Matt", "Aimonetti"}
	fmt.Println(Greet(u))
}


```



```go

package main

import (
	"fmt"
)

type User struct {
	FirstName, LastName string
}

func (u *User) Name() string { //Name method used for type User
	return fmt.Sprintf("%s %s", u.FirstName, u.LastName)
}

type Customer struct {
	Id       int
	FullName string
}

func (c *Customer) Name() string { //Name method used for type Customer
	return c.FullName
}

type Namer interface {
  Name() string //Both Name() methods can be called using the Namer interface
}

func Greet(n Namer) string {
	return fmt.Sprintf("Dear %s", n.Name())
}

func main() {
	u := &User{"Matt", "Aimonetti"}
	fmt.Println(Greet(u))
	c := &Customer{42, "Francesc"}
	fmt.Println(Greet(c))
}


Output
Dear Matt Aimonetti
Dear Francesc

```



### Satisfying Interfaces



A type implements an interface by implementing the methods that it contains.

*There is no explicit declaration of intent.* The interfaces are satisfied implicitly through its methods.

Implicit interfaces decouple implementation packages from the packages that define the interfaces: neither depends on the other.

It also encourages the definition of **precise interfaces**, because you don’t have to find every implementation and tag it with the new interface name.



```go

package main

import (
	"fmt"
	"os"
)

type Reader interface {
	Read(b []byte) (n int, err error)
}

type Writer interface {
	Write(b []byte) (n int, err error)
}

type ReadWriter interface {
	Reader
	Writer
}

func main() {
	var w Writer

	// os.Stdout implements Writer
	w = os.Stdout

	fmt.Fprintf(w, "hello, writer\n")
}


```



### Returning Errors

An **error** is anything that can describe itself as an error string. The idea is captured by the predefined, built-in interface type, error, with its single method, `Error`, returning a string:

```go
type error interface {
    Error() string
}
```



**The `fmt` Package**



The `fmt` package’s various print routines automatically know to call the method when asked to print an error.



```go

package main

import (
    "fmt"
    "time"
)

type MyError struct {
    When time.Time
    What string
}

func (e *MyError) Error() string {
    return fmt.Sprintf("at %v, %s",
        e.When, e.What)
}

func run() error {
    return &MyError{
        time.Now(),
        "it didn't work",
    }
}

func main() {
    if err := run(); err != nil {
        fmt.Println(err)
    }
}


Output:
at 2022-05-17 11:36:09.048999615 +0000 UTC, it didn't work


```















































