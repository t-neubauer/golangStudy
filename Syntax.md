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

// make is a builtin function to create dynamically sized arrays

b := make([]int, 0, 5) // len(b)=0, cap(b)=5
b = b[:cap(b)] // len(b)=5, cap(b)=5
b = b[1:]      // len(b)=4, cap(b)=4

// slice in slice

// Create a tic-tac-toe board.
	board := [][]string{
		[]string{"_", "_", "_"},
		[]string{"_", "_", "_"},
		[]string{"_", "_", "_"},
	}

// append to a slice (builtin function)

func append ( s []T, vs ...T) []T // if backing array of s is too small, bigger array will be allocated.
var s []int
s = append (s, 2, 3, 4)

// range
// iterates over slice or map. Two elements returned. 1st index. 2nd copy of the element at that index
var pow = []int{1, 2, 3, 4, 8, 16, 32}
for i,v := range pow{
}

//skip index or value by assigning `_`. omit value if you only want the index.
for i, _ := range pow
for _, v := range pow
for i := range pow



```

### Maps

maps keys to values *C# Dictionary*

zero value is `nil`. 
make function returns map of given type, initialized and ready to use

Example:
```golang
type Vertex struct {
  Lat, Long float64
}

var m map[string]Vertex

func main() {
  m = make(map[string]Vertex)
  m["Bell Labs"] = Vertex {
    40.68322, -74.39967,
  }
  fmt.Println(m["Bell Labs"])
}
```


Map Literals, like struct literals, but Key is required.

```golang 
var m = map[string]Vertex{
  "Bell Labs": Vertex {
    40.68433, -74.39976,
  },
  "Google": Vertex{
    37.42202, -122.08408,
  },
}

// if top level type is just a type name, you can omit it from the elements of the literal
var m = map[string]Vertex{
  "Bell Labs":  { 40.68433, -74.39976 } , 
  "Google": { 37.42202, -122.08408 } ,
}
```

#### Mutating Maps
```golang
// insert or update element in map m
m[key] = elem
// retrieve an element
elem = m[key]
//delete an element
delete(m, key)
// test that a key is present with a two-value assignment
elem, ok = m[key] // if key is in m, ok is true, else it is false. If key is not in the map, elem is the zero value for the maps element type.
// IF elem or ok have not yet been declared, you can use the short declaration form
elem, ok := m[key]
// Example:
m := make(m[string]int)
m["Answer"] = 42
delete(m, "Answer")
value, ok := m["Answer"] //value = 0; ok = false

```

### Functions as values

function values may be used as function arguments and return values.

```golang
func compute (fn func(float64, float64) float64) float64
{
  return fn(3,4)
}
func main(){
  hypot := func(x,y float64) float64{
    return math.Sqrt(x*x + y*y)
  }
  fmt.Println(compute(hypot))
}
```

### Function Closures

A closure is a function value that references variables from outside its body. 

```golang 
func adder () func(int) int {
  sum:= 0
  return func(x int) int {
    sum += x
    return sum
  }
}

func main() {
  pos, neg := adder(), adder()
  for i:=0; i<10; i++{
    fmt.Println(
      pos(i),
      neg (-2*i),
    )
  }>
}
```

### Methods

Methods can be defined on types.
Methods have a special receiver argument.
```golang 
v:=Vertex{1,2}
func (v Vertex) Abs() floa64 { // Abs method has a receiver of type Vertex

}
v.Abs()

// normal function
func Abs(v Vertex) float64 {...}
v:= Vertex {3,4}
Abs(v)

// Methods can be declared on non-struct types too.
// Only declare a method with a reveicer whose type is defined in the same package as the method.
type MyFloat float64

func (f MyFloat) Abs() float64
  if f < 0{
    return float64(-f)
  }
  return float64(f)

// Pointer Receivers
// receiver type then has the literal syntax *T for a type T. T cannot itself be a pointer such as *int
// Methods with pointer receivers can modify the value to which the receiver points.
// pointer receivers are more common than value receivers.

func (v *Vertex) Scale(f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}
v := Vertex {3,4}
v.Scale(10)


// Pointer Indirection
// functions with pointer argument must take a pointer
func ScaleFunc(v *Vertex, f float){...}
ScaleFunc(v,5) // doesnt compile
ScaleFunc(&v,5) //OK
// methods with pointer receiver take either a value or a pointer.
// in case of a value, the method with the pointer receiver is indirectly called automatically. 
var v Vertex
v.Scale(5) // OK, because its interpreted as (&v).Scale(5) because Scale has a pointer receiver.
p:= &v
p.Scale(10) //OK
// Same for value arguments. Functions that take value arguments must take a value of that type while methods with value receivers take either a value or a pointer as the receiver when they are called.
var v Vertex 
fmt.Println(AbsFunc(v)) // OK
fmt.Println(AbsFunc(&v)) // compile error
var v Vertex 
fmt.Println(v.Abs()) // OK
p:= &v
fmt.Println(p.Abs()) // OK

// Two Reasons to use a pointer receiver
// 1) so the method can modify the value that its receiver points to
// 2) to avoid copying the value on each method call. More efficient for large structs for example. 


// all methods on a given type should have either value or pointer receivers but not a mixture of both. 

```
### Interfaces

```golang
// set of method signatures
// value of interface type can hold any value that implements those methods

type Abser interface {
  Abs() float64
}

func main() {
 var a Abser
 f := MyFloat(-math.Sqrt2)
 v := Vertex{3, 4}
 a = f // a MyFloat implements Abser
 a = &v // a *Vertex implements Abser
 a = v // v is a Vertex, not *Vertex and does NOT implement abser
}

type MyFloat float64
func (f MyFloat) Abs() float64 {...}
type Vertex struct { X,Y float64}
func (v *Vertex) Abs() float64 {...}

// interfaces are implemented implicitly. There is no explicit `implements` declaration
// decouples definition of an interface from its implementation which can then appear in any package without prearrangement.

// Interface value
// interfaces values can be thought of as a tuple of a value and a concrete type
(value, type)
// interface holds a value of a specific underlying concrete type.
// Calling a method on an interface value executes the method of the same name on its underlying type.

// Interface values with nil underlying values
// if value within interface itself is nil, method will be called with a nil receiver.
// In other languages: NullPointerException
// Go: Common to write methods that gracefully handle being called with a nil receiver.
// Note: interface value that hold a nil concrete value is itself not nil

type I interface {
  M()
}

type T struct { S string }
func (t *T) M() {
  // handle nil gracefully
  if t == nil {
    fmt.Println("<nil>")
    return
  }
  fmt.Println(t.S)
}

// Nil interface values
// holds neither value nor concrete type.
// Calling method on a nil interface creates a runtime error (because there is no type inside the interface tuple to indicate which concrete method to call)

// Empty interface
// interface that specifies zero methods
// may hold values of any type. (Cause any type implements at least zero methods)
// "any" is an equivalent to interface{}
// used by code that handles values of unknown type.
var i interface{}
var i any

// Type assertions
// provide access to an interface value's underlying concrete value
t:= i.(T) // asserts that interface value i holds concrete type T and assigns the underlying T value to variable t
// if i does not hold T the statement will trigger a panic. see Concepts.md
// testing wether an interface value holds a specific type. The type assertion can return two values. The underlying value and a boolean.
t, ok := i.(T) //note similarity of reading from a map.

// Type switches
// construct that permits serveral type assertions in series
switch v:= i.(type) {
  case T: // here v has type T
  case S: // here v has type S
  default: // no match. type is the same as i
}

// Stringers
// type that can describe itself as a string. 
type Stringer interface {
  String() string
}
```

### Errors

error type is a built-in interfaces.
Functions often return an `error` value and calling code should handle errors by testing wether the error equals `nil`.
`nil` means success, non-nil means error.
```golang
type error interface {
  Error() string
}

i, err := strconv.Atoi("42")
if err != nil {
  fmt.Printf("couldnt convert number: %v\n", err)
  return
}
fmt.Println("Converted integrer:", i)

```
