## Slices

```golang
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