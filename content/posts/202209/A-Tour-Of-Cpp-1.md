---
title: "A Tour of C++ Notes (Part 1)"
date: 2022-09-12T23:44:21-04:00
draft: false
tags: ["C++", "A Tour of C++"]
author: "Kin" 
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
hidemeta: false
comments: false
---

[[C++]] 
[[A Tour of C++]]

# A Tour of C++  (2nd edition) Notes Part 1

## Preface

+ To really know a city, you have to live in it, often for years.
+ Read C++ Core Guidelines later

## The Basics

+ Many obj files can be combined by a linker 
+ When we talk about portability of C++ programs, we usually mean portability of source code
+ C++ STL can be implemented by C++ itself
+ C++ STL has minor uses of machine code (except thread context switching
+ C++ is a satically typed language (every entity must be known to the compiler before it's used)
+ A single quote (') can be used as a digit separator for better read.
+ Narrowing conversions are allowed and implicitly applied when using `=` (but not `{}`), this is for C compatibility
```cpp
int i1 = 7.8; // i1 becomes 7 (surprised for me)
int i2 {7.8}; // error : floating-point to integer conversion
```
+ C++ supports 2 notions of immutability
	+ `const`: "I promise not to change the value". Value can be calculated at run time.
	+ `constexpr`: "To be evaluated at compile time". Value is calculated by the compiler. `constexpr` can also be used for functions. A `constexpr` function can be used for non-constant arguments. 
+ pointer with `const`:
	+ `char*` is a **mutable** pointer to a **mutable** character/string.
	+ `const char*` is a **mutable** pointer to an **immutable** character/string. You cannot change the contents of the location(s) this pointer points to. Also, compilers are required to give error messages when you try to do so. For the same reason, conversion from `const char *` to `char*` is deprecated.
	+ `char* const` is an **immutable** pointer (it cannot point to any other location) **but** the contents of location at which it points are **mutable**.
	+ `const char* const` is an **immutable** pointer to an **immutable** character/string.
+ `constexpr` example:
```cpp
constexpr double nth(double x, int n)
{
	double res = 1;
	int i = 0;
	while (i < n)
	{
		res *= x;
		i++;
	}
	return res;
}
```

+ reference & avoids copy of the variable, and it can be used with const together.
+ C++ direct mapping to hardware is crucial for the performance.
+ An uninitialized reference is invalid (int & r2).

## User-Defined Types
+ The `new` operator allocates memory from ==free store== (dynamic memory, heap)
+ Constructor: `Vector(int s): elem{new double[s]}, sz{s} {}`, interesting to see new in a constructor
+ using `variant` is safer and easier than `union` . E.g.: `variant<Node*, int>`  pp26
+ `enum`： there are 2 kinds of enumerations: `enum class` and plain `enum`
```cpp
enum class Color {red, blue green};
Color x = red;         // error: which red?
Color z = Color::red;  // OK
int i = Color::red;    // error
Color x = Color{5};    // OK
Color y {6};           // OK
```

By default, `enum class` has only assignment, initialization and comparisons (== and <), but we can define operators to it:
```cpp
Color& operator++(Color& t)
{
	switch (t)
	{
		case Color::green:  return t=Color::yellow;
		case Color::yellow: return t=Color::red;
		case Color::red:    return t=Color::green;
	}
}
Color light = Color::red;
Color next = ++light;  // next becomes green
```

