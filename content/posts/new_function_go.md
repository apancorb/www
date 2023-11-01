---
title: "The new Function in Go"
author: "Antonio Pancorbo"
summary: "Brief overview of the new built-in function in Go"
date: 2023-04-25
cover:
    image: /posts/go_new_function.jpeg
    caption: "Image from [Guy J Grigsby](https://medium.com/star-gazers/go-options-pattern-da49185a2526)"
tags: ["go", "programming"]
draft: false
---

A variable is a piece of storage containing a value. In Go there are multiple
ways of creating variables. Most often, these variables are created by
declarations and are identified by name, such as the famous `i` index variable.
Another way to create a variable in to use the built-in function `new`.

```go
// The new built-in function allocates memory. The first argument is a type,
// not a value, and the value returned is a pointer to a newly
// allocated zero value of that type.
func new(Type) *Type
```
When you use the `new(T)` expression in Go, it creates an unnamed variable
of type `T`, initializes it to the zero value of `T`, and returns its address
as a value of type `*T`. This allows you to allocate memory for a new value
of a specific type and get a pointer to that memory, without having to give
the variable a name or explicitly initialize it.

In Go, a variable that is created using the `new` function is functionally
equivalent to an ordinary local variable whose address has been taken,
with the main difference being that a new variable doesn't need to be
given a random name. As a result, new can be thought of as a syntactic
convenience rather than a fundamental feature of the language.
For instance the following two functions have identical behaviors.

```go
// using new built-in function
func newInt() *int {
  return new(int)
}

// declaring dummy variable
func newInt() *int {
  var dummy int
  return &dummy
}
```

The actual implementation of the `new` function in Go is part of the Go
runtime and is written in Go itself. The implementation is quite complex
and involves a number of different components, including the garbage collector,
memory allocator, and type system. Here is a simplified version of what the
`new` function could sort look like from a high level:

```go
// new allocates memory for a single value of the specified type and returns a pointer to it.
func new(Type) *Type {
    // Get the size of the type.
    size := unsafe.Sizeof(Type)

    // Allocate a new block of memory using the built-in make function.
    ptr := make([]byte, size)

    // Convert the byte slice to a pointer of the specified type.
    return (*Type)(unsafe.Pointer(&ptr[0]))
}
```

In this implementation, the new function takes a single parameter of type `Type`,
which is the type of the value that will be allocated. The unsafe package is 
used to work with low-level memory operations that are not guaranteed to be safe 
or portable. The `unsafe.Sizeof` function is used to get the size of the type in 
bytes, which is needed to allocate the correct amount of memory. The make function 
is used to allocate a new block of memory as a byte slice of the appropriate size.
Finally, the `unsafe.Pointer` function is used to convert the byte slice to a pointer 
of the specified type. This pointer is returned to the caller as a `*Type`.

You would typically use the new function when you need to create a new value of a
specific type, and you want to initialize it to its zero value. This is useful when 
you need to allocate memory for a new value on the heap, rather than on the stack.
This can be useful in situations where you need to create a temporary value or 
pass a pointer to a function without needing to access the value directly.

Note that the new function is rarely used in idiomatic Go code, because most values 
are allocated on the stack rather than the heap. Furthermore, Go programmers often 
prefer to use type literals or type declarations to create and initialize variables,
as this can be more explicit and easier to read. In some cases, using a type literal 
or type declaration can also be more efficient than using new, as it allows the Go
compiler to optimize the code more effectively.

---

### References
1. Donovan, A.A.A. and Kernighan, B.W. (2020) The go programming language.
New York, N.Y: Addison-Wesley. 
