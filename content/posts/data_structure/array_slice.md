---
title: "[DataStructure] Array & Slice"  
date: 2022-04-04
categories: ["Go", "DataStructure", "EN"]  
tags: ["Go", "DataStructure", "EN"]

summary: What are array and slice

weight: 1
math: true
draft: false
---

## What is Array?

- Array is a collection of data stored in physically contiguous memory.
- Array is fixed size. Once the size is given to it, it cannot be changed.
- Elements must be same type.

### Initialize Array
```go
package main

import "fmt"

func main() {
	var s [4]int    // initialize int type array with length 4.

	fmt.Println(s)  // [0 0 0 0]
}
```
You can define length of array like [4]T.<br>
Length of array is defined at compile time. So it cannot be changed.


### Memory Alignment

```go
package main

import (
	"fmt"
	"unsafe"
)

func main() {
	s := [4]int{10, 20, 30, 40}
	fmt.Printf("%p, %p, %p, %p\n", &s[0], &s[1], &s[2], &s[3])  // 0xc000102000, 0xc000102008, 0xc000102010, 0xc000102018
	fmt.Println(unsafe.Sizeof(s))   // total size is 8 x 4 = 32 bytes
}
```
Array is contiguous memory.<br>
Each element in array s is int. Int is 8 bytes in 64 bits architecture.<br>
Calculate the position of each element by simply adding an offset to a base value.<br>
You can see address of elements are increasing by 8.

### Access Array by Index
```go
package main

import (
	"fmt"
)

func main() {
	s := [4]int{10, 20, 30, 40}
	fmt.Println(s[0]) // 10
}
```
Array has advantage in random access. It uses index to access the specific element.<br>
No matter how long the array is, if you use index, it always costs O(1).<br>

Array has good cache locality too.<br>
Since it is stored in contiguous memory, there is less chance of cache miss.<br>

But since it has fixed size, it is a bit inflexible, so array is not often seen in Go code.<br>
However, we see slices everywhere.<br>


## What is Slice?

- Slice is a dynamic array.
- Unlike array, slice's size can be changed during runtime.

### Initialize Slice
```go
package main

import (
	"fmt"
)

func main() {
	s := []int{10, 20, 30, 40}  // method one
	t := make([]int, 5) // method two
	fmt.Println(s)  // [10 20 30 40]
	fmt.Println(t)  // [0 0 0 0 0]
}
```
There are two ways to initialize slice.<br>
One is same as initializing array, but without any length. Like []T.<br>
Second is by using built-in function called "make".<br>
<br>
func make([]T, len, cap) []T<br>
make function takes a type, a length, and an optional capacity.

### Access Slice by Index
```go
package main

import (
	"fmt"
)

func main() {
	s := []int{10, 20, 30, 40}
	fmt.Println(s[0]) // 10
}
```
Slice just works like array.

Because...

### Slice Internal

![DataStructure](/../images/slice-struct.png)

Slice is just data structure that points to its underlying array.<br>
It consists of a pointer to the array, the length of the slice, and capacity.<br>

![DataStructure](/../images/slice-1.png)
```go
package main

import "fmt"

func main() {
	s := make([]byte, 5)
	fmt.Println(s) // [0 0 0 0 0]
}
```
This is how slice of bytes looks like under layer.<br>
Slice pointer is pointing to its underlying array.<br>
Slice has length of 5 and capacity of 5.

### Slice Append
```go
package main

import "fmt"

func main() {
	s := make([]int, 5)
	t := make([]int, 5, 6)

	fmt.Println(len(s), cap(s)) // 5 5
	fmt.Println(len(t), cap(t)) // 5 6

	fmt.Printf("%p\n", s)   // 0xc00018a000
	fmt.Printf("%p\n", t)   // 0xc00018a030

	// append data

	s = append(s, 1) 
	t = append(t, 1) 

	fmt.Println(len(s), cap(s)) // 6 10
	fmt.Println(len(t), cap(t)) // 6 6

	fmt.Printf("%p\n", s)   // 0xc000194000
	fmt.Printf("%p\n", t)   // 0xc00018a030
}
```
When adding element to the back of slice, if slice has free capacity, then it costs O(1).<br>
You just have to push into the free space.<br>
However if capacity does not have any free space, then the magic happens.<br>
Since array is fixed size and cannot be changed, slice copies its underlying array then make another new array with bigger size. (usually doubles)<br>
Then paste old data into new array and points to it.<br>
This costs O(N) because you have to copy N times.<br><br>

```go
package main

import "fmt"

func main() {
	s := make([]int, 5)
	t := make([]int, 5, 10)

	fmt.Println(len(s), cap(s)) // 5 5
	fmt.Println(len(t), cap(t)) // 5 10

	fmt.Printf("%p\n", s)   // 0xc00018a000
	fmt.Printf("%p\n", t)   // 0xc00018a030

	// append data

	s = append(s[:4], s[3:]...) // append to make replace spot
	t = append(t[:4], t[3:]...) // append to make replace spot
	s[3] = 100  // change
	t[3] = 100  // change

	fmt.Println(len(s), cap(s)) // 6 10
	fmt.Println(len(t), cap(t)) // 6 10

	fmt.Printf("%p\n", s)   // 0xc000194000
	fmt.Printf("%p\n", t)   // 0xc00018a030

	fmt.Println(s)  // [0 0 0 100 0 0]
	fmt.Println(t)  // [0 0 0 100 0 0]
}
```
Now let's try to add to the middle.<br>
It looks same as appending to last. But it's not.<br>
Even though you have enough capacity and did not make new underlying array, you appended to make replace spot.<br>
So it is O(N).

### Slice Remove
```go
package main

import "fmt"

func main() {
	s := []int{1,2,3,4,5}

	s = append(s[:2], s[3:]...) // append to delete index 2

	fmt.Println(s)  // [1 2 4 5]
}
```
When removing, element from first and last index is O(1).<br>
Because you just move around length pointer.<br>
But removing element in the middle needs append and costs O(N).<br>

### Nil Slice
```go
package main

import "fmt"

func main() {
	var a []int
	var b []int = make([]int, 0)
	var c []int = []int{}

	fmt.Println(a, b, c) // [] [] []
	fmt.Println(a == nil) // true
	fmt.Println(b == nil) // false
	fmt.Println(c == nil) // false
}
```
Nil slice means, slice does not have memory address.<br>
B and C slices are initialized with 0 length underlying array, and have memory address.<br>
However, A slice is just declared and is not initialized. (A does not have underlying array)<br>
All 3 of them have 0 length, but they are not same.<br>
You have to be careful dealing with nil slice and empty slice.

## References

- https://go.dev/blog/slices-intro
- https://en.wikipedia.org/wiki/Locality_of_reference
- https://www.geeksforgeeks.org/introduction-to-arrays