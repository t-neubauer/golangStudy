# Syntax

### Functions
- Parameter types are declared after the variable name, e.g. `func add (x int, y int) int { return x + y }`  
reads like "Function add takes two ints and returns an int". Background: makes complex functions and pointers easier to read. 

```golang
anInteger int = 4
x,y int = 10, 2 //declare multiple variables at once

// {} are mandatory for functions
func returnFive() int{
  return 5
}

// functions can return any number of results
func swap (x,y string) (string, string){
  return y,x
}

// "naked" return returns the value of the named return values. Not suggested for longer functions due to harm of readability.
func nakedReturn (input int ) (output1, output2 string){
  output1 = "this is"
  output2 = "your output"
  return
}
```

### Variables

Can be at package or at function level.
```golang
package main
import "fmt"

var c, python, java bool
var i, j int = 1,2 // Variables with initializers
var (              // Variable blocks. Like import blocks. 
  ToBe bool = false
  MaxInt uint64 = 1<<64 -1 
  z complex128 = cmplx.Sqrt(-5 + 12i)
)

//zero values
numeric int = 0
boolean bool = false
empty string = ""

// Type conversions need to be explicitly stated in Go. 
var i int = 42
var f float64 = float64(i)
var u uint = uint(f)
// Type Inference when declaring a variable. Inferred from value on right side
var i int 
j := i // j is an int

//Constants. Can be chars, strings, booleans, numerics. Cannot be declared using ":="
const World = "Earth"

func main() {
	var i int
	fmt.Println(i, c, python, java)

  k := 5 // Instead of variable declaration. Typed implicitly. Only within functions!
}
```

### For

For is the only looping construct in go. 
does not have (). Requires {}.
- init statement (optional)
- condition expression
- post statement (optional)
  
``` golang
// Basic loop
for i := 0; i < 10; i++ {

}

// while loop
for i < 10{

}

// Infinite loop
for {

}

// same for if. () are optional. {} required.
if x < 10 {

}
// can have init statement too. Declared variable only available for if and else statement.
if v:= 10; v < 11{
  return "v is small"
}

//switch. has no fallthrough. Switch cases dont need to be constants or integers.
switch os := runtime.GOOS; os {
  case "darwin":
  fmt.Println("maxOS")
  case "linux":
  fmt.Println("Linux")
  default:
  fmt.Printf("%s.\n", os)
}
// switch true
switch {
  ...
}

//Defer. Defers execution of a function until the surrounding function returns. Deferred calls are pushed onto a stack. Last-in-First-out
defer fmt.Println("world")
fmt.Println("Hello")

```

### Pointers
Pointer holds the memory address of a value. zero value is `nil`
```golang
// *T is a pointer to a T value. 
var p *int 

// & operator generates a pointer to its operand
i :=42
p = &i

// the * operator denotes the pointers underlying value
// Known as "Dereferencing" or "Indirecting"
fmt.Println(*p)
*p = 21

// pointer example:
i, j := 42, 2701

p := &i         // point to i
fmt.Println(*p) // read i through the pointer
*p = 21         // set i through the pointer
fmt.Println(i)  // see the new value of i

p = &j         // point to j
*p = *p / 37   // divide j through the pointer
fmt.Println(j) // see the new value of j

//Structs (collection of fields). Fields are accessed using a dot(.)
type Vertex struct {
  X int
  Y int
}
v := Vertex{1,2}
v.X = 4
//Pointers to structs. 
p := &v
p.X = 9 // go lets us permit the explicit dereference. you could write (*p).X too...
v2 = Vertex{X: 1}  // Y:0 is implicit

// Arrays
var a[10]int //cant be resized

// Slices
// dynamically sized, flexible view into elements of an array. formed by specifying low and high bound as integers. 
// Includces low, excludes high. Slices are like references to arrays.
a[low : high]

primes := [6]int{2, 3, 5, 7, 11, 13}
var s []int = primes[1:4] // container 3,5,7

// array literal vs slice literal
[3]bool{true, true, false} //array literal
[]bool{true, true, false} //slice literal

// high or low bounds can be omitted to use their defaults instead.
// low default is 0; high default is length of the slice/array

var a [10]int //array 
a[:] // equal to a[0:10] or a[:10] or a[0:]

len(a)  // length of slice a. Number of elements it contains.
cap(a) // capacity of slice a. Number of elements in underlying array counting from first element in the slice.

// A slices length can be extended by re-slicing it, provided it has sufficient capacity.
s := []int{2, 3, 5, 7, 11, 13}
s = s[:4]
s = s[2:]
```