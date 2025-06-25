+++
title = "Learning Go Concepts"
date = "2025-06-25"
description = "Trying to understand the fundamentals of Go"
tags = [
    "golang",
    "programming",
    "tutorial",
    "basics"
]
+++

## 1. Basic Syntax and Structure

### Hello World

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, World!")
}
```

Every Go program starts with a package declaration. The `main` package is special - it's the entry point of executable programs.

## 2. Variables and Data Types

### Variable Declarations

```go
// Explicit type declaration
var name string = "John"
var age int = 25

// Type inference
var city = "New York"

// Short variable declaration (only inside functions)
country := "USA"
```

### Basic Data Types

- **Integers**: `int`, `int8`, `int16`, `int32`, `int64`
- **Unsigned integers**: `uint`, `uint8`, `uint16`, `uint32`, `uint64`
- **Floating point**: `float32`, `float64`
- **Boolean**: `bool`
- **String**: `string`
- **Byte**: `byte` (alias for uint8)
- **Rune**: `rune` (alias for int32, represents Unicode code points)

## 3. Control Structures

### If Statements

```go
if age >= 18 {
    fmt.Println("Adult")
} else if age >= 13 {
    fmt.Println("Teenager")
} else {
    fmt.Println("Child")
}

// If with initialization
if x := 10; x > 5 {
    fmt.Println("x is greater than 5")
}
```

### Loops

Go has only one loop construct: `for`

```go
// Traditional for loop
for i := 0; i < 10; i++ {
    fmt.Println(i)
}

// While-like loop
count := 0
for count < 5 {
    fmt.Println(count)
    count++
}

// Infinite loop
for {
    // Loop forever (use break to exit)
}

// Range loop
numbers := []int{1, 2, 3, 4, 5}
for index, value := range numbers {
    fmt.Printf("Index: %d, Value: %d\n", index, value)
}
```

### Switch Statements

```go
switch day := "Monday"; day {
case "Monday":
    fmt.Println("Start of work week")
case "Friday":
    fmt.Println("TGIF!")
case "Saturday", "Sunday":
    fmt.Println("Weekend!")
default:
    fmt.Println("Regular day")
}
```

## 4. Functions

### Basic Function Syntax

```go
func add(a int, b int) int {
    return a + b
}

// Multiple parameters of same type
func multiply(a, b int) int {
    return a * b
}

// Multiple return values
func divide(a, b int) (int, error) {
    if b == 0 {
        return 0, fmt.Errorf("division by zero")
    }
    return a / b, nil
}
```

### Named Returns

```go
func rectangle(length, width int) (area, perimeter int) {
    area = length * width
    perimeter = 2 * (length + width)
    return // naked return
}
```

### Variadic Functions

```go
func sum(numbers ...int) int {
    total := 0
    for _, num := range numbers {
        total += num
    }
    return total
}

// Usage: sum(1, 2, 3, 4, 5)
```

## 5. Data Structures

### Arrays

```go
// Fixed size
var arr [5]int
arr[0] = 10

// Initialize with values
numbers := [5]int{1, 2, 3, 4, 5}

// Let compiler count
auto := [...]int{1, 2, 3}
```

### Slices

```go
// Dynamic arrays
var slice []int
slice = append(slice, 1, 2, 3)

// Create with make
slice2 := make([]int, 5)    // length 5, capacity 5
slice3 := make([]int, 5, 10) // length 5, capacity 10

// Slice operations
numbers := []int{1, 2, 3, 4, 5}
subset := numbers[1:4] // [2, 3, 4]
```

### Maps

```go
// Create map
ages := make(map[string]int)
ages["Alice"] = 25
ages["Bob"] = 30

// Map literal
scores := map[string]int{
    "Alice": 95,
    "Bob":   87,
    "Carol": 92,
}

// Check if key exists
if age, exists := ages["Alice"]; exists {
    fmt.Printf("Alice is %d years old\n", age)
}

// Delete key
delete(ages, "Bob")
```

### Structs

```go
type Person struct {
    Name    string
    Age     int
    Email   string
}

// Create struct
p1 := Person{
    Name:  "John",
    Age:   30,
    Email: "john@example.com",
}

// Anonymous struct
point := struct {
    X, Y int
}{10, 20}
```

## 6. Pointers

```go
func main() {
    x := 42
    p := &x        // p is a pointer to x
    fmt.Println(*p) // dereference pointer, prints 42

    *p = 21        // change value through pointer
    fmt.Println(x) // prints 21
}

// Pointers with structs
func (p *Person) UpdateAge(newAge int) {
    p.Age = newAge
}
```

## 7. Methods and Interfaces

### Methods

```go
type Rectangle struct {
    Width, Height float64
}

// Method with value receiver
func (r Rectangle) Area() float64 {
    return r.Width * r.Height
}

// Method with pointer receiver
func (r *Rectangle) Scale(factor float64) {
    r.Width *= factor
    r.Height *= factor
}
```

### Interfaces

```go
type Shape interface {
    Area() float64
    Perimeter() float64
}

type Circle struct {
    Radius float64
}

func (c Circle) Area() float64 {
    return 3.14159 * c.Radius * c.Radius
}

func (c Circle) Perimeter() float64 {
    return 2 * 3.14159 * c.Radius
}

// Circle automatically implements Shape interface
```

## 8. Error Handling

```go
import (
    "errors"
    "fmt"
)

func divide(a, b float64) (float64, error) {
    if b == 0 {
        return 0, errors.New("division by zero")
    }
    return a / b, nil
}

func main() {
    result, err := divide(10, 0)
    if err != nil {
        fmt.Printf("Error: %v\n", err)
        return
    }
    fmt.Printf("Result: %f\n", result)
}
```

## 9. Goroutines and Channels

### Goroutines (Concurrent execution)

```go
import (
    "fmt"
    "time"
)

func sayHello(name string) {
    for i := 0; i < 3; i++ {
        fmt.Printf("Hello, %s!\n", name)
        time.Sleep(100 * time.Millisecond)
    }
}

func main() {
    go sayHello("Alice") // runs concurrently
    go sayHello("Bob")   // runs concurrently

    time.Sleep(1 * time.Second) // wait for goroutines
}
```

### Channels

```go
func main() {
    ch := make(chan string)

    go func() {
        ch <- "Hello from goroutine!"
    }()

    message := <-ch // receive from channel
    fmt.Println(message)
}

// Buffered channel
ch := make(chan int, 3)
```

## 10. Packages and Imports

### Creating a Package

```go
// In file math/calculator.go
package math

func Add(a, b int) int {
    return a + b
}

func subtract(a, b int) int { // lowercase = private
    return a - b
}
```

### Using the Package

```go
package main

import (
    "fmt"
    "yourproject/math"
)

func main() {
    result := math.Add(5, 3)
    fmt.Println(result)
}
```

## Key Go Principles

1. **Simplicity**: Go favors simple, readable code over clever solutions
2. **Composition over Inheritance**: Use interfaces and embedding instead of traditional inheritance
3. **Explicit Error Handling**: Errors are values, handle them explicitly
4. **Concurrency**: Goroutines and channels make concurrent programming easier
5. **Fast Compilation**: Go compiles quickly to native machine code
