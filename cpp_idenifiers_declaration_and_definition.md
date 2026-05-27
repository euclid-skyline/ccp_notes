# C++ Identifier Declaration and Definition Notes 

## Table of Contents

- [C++ Identifier Declaration and Definition Notes](#c-identifier-declaration-and-definition-notes)
  - [Table of Contents](#table-of-contents)
  - [Purpose](#purpose)
  - [Identifier Declaration vs Definition: (The Core Idea)](#identifier-declaration-vs-definition-the-core-idea)
  - [Forward Declarations and Why They Matter](#forward-declarations-and-why-they-matter)
    - [Core Rule at the Point of Use](#core-rule-at-the-point-of-use)
    - [Forward Declaration in the Same File](#forward-declaration-in-the-same-file)
    - [Forward Declaration Through a Header](#forward-declaration-through-a-header)
    - [Common Forward Declaration Forms](#common-forward-declaration-forms)
  - [Variable Declaration and Definition](#variable-declaration-and-definition)
    - [Simple Examples](#simple-examples)
    - [When a Variable Declaration Is Not a Definition](#when-a-variable-declaration-is-not-a-definition)
  - [Function Declaration and Definition](#function-declaration-and-definition)
    - [Simple Examples](#simple-examples-1)
    - [Function Prototype Meaning](#function-prototype-meaning)
  - [How to Read a C++ Declaration](#how-to-read-a-c-declaration)
  - [Variable Type Modifiers Before the Name](#variable-type-modifiers-before-the-name)
    - [`const`](#const)
    - [`volatile`](#volatile)
    - [`constexpr`](#constexpr)
    - [`constinit`](#constinit)
    - [`mutable`](#mutable)
    - [`static`](#static)
    - [`extern`](#extern)
    - [`thread_local`](#thread_local)
    - [`signed`, `unsigned`, `short`, `long`](#signed-unsigned-short-long)
  - [Variable Type Modifiers After the Name](#variable-type-modifiers-after-the-name)
    - [Pointers](#pointers)
    - [References](#references)
    - [Arrays](#arrays)
    - [Pointer Constness Combinations](#pointer-constness-combinations)
  - [Function Type Modifiers Before the Name](#function-type-modifiers-before-the-name)
    - [Return Type](#return-type)
    - [`inline`](#inline)
    - [`static` on Functions](#static-on-functions)
    - [`constexpr` and `consteval`](#constexpr-and-consteval)
    - [`virtual`](#virtual)
    - [`explicit`](#explicit)
  - [Function Type Modifiers After the Name](#function-type-modifiers-after-the-name)
    - [Parameter List](#parameter-list)
    - [`const` Member Functions](#const-member-functions)
    - [Reference Qualifiers `&` and `&&`](#reference-qualifiers--and-)
    - [`noexcept`](#noexcept)
    - [`override` and `final`](#override-and-final)
    - [Trailing Return Types](#trailing-return-types)
  - [Declarations That Often Confuse Returning Developers](#declarations-that-often-confuse-returning-developers)
  - [Examples: Reading Declarations Left to Right and Right to Left](#examples-reading-declarations-left-to-right-and-right-to-left)
  - [Common Mistakes](#common-mistakes)
    - [1. Thinking `int* a, b;` makes both pointers](#1-thinking-int-a-b-makes-both-pointers)
    - [2. Confusing Declaration with Definition](#2-confusing-declaration-with-definition)
    - [3. Misreading `const`](#3-misreading-const)
    - [4. Forgetting That Member Function `const` Applies to `this`](#4-forgetting-that-member-function-const-applies-to-this)
    - [5. Assuming `static` Always Means the Same Thing](#5-assuming-static-always-means-the-same-thing)
  - [Quick Cheat Sheet](#quick-cheat-sheet)
  - [Practical Rules of Thumb](#practical-rules-of-thumb)
  - [Summary](#summary)

## Purpose

This document is a practical guide to C++ identifiers, declarations, definitions, and forward declarations. It explains what an identifier is, where and why it must be declared, and how declaration and definition rules apply to variables, arrays, user-defined types, and functions.

The goal is to help you read and write declarations confidently. By the end, you should be able to interpret modifiers before and after names, distinguish declaration from definition, and avoid common mistakes that cause compile or linkage errors.

## Identifier Declaration vs Definition: (The Core Idea)

A C++ identifier is a name given to a program entity.

In everyday code, identifiers name:

- scalar/simple variable objects: `int`, `float`, `bool`, `char`
- user-defined types and their objects: `struct`, `class`
- array objects
- function names

Examples of identifiers:

```cpp
int age = 30;               // identifier: age
float price = 9.5f;         // identifier: price
bool ready = true;          // identifier: ready
char grade = 'A';           // identifier: grade

struct Point { int x; int y; }; // identifier: Point
class Engine {};                 // identifier: Engine

int scores[5] = {1, 2, 3, 4, 5}; // identifier: scores

void print_report();             // identifier: print_report
```

Once an identifier is chosen, C++ needs declaration and definition rules for it.

A declaration tells the compiler that an identifier exists and what its type or signature is.

A definition provides the actual entity: storage for an object, or the body of a function, or the full body of a struct or class.

Short rule:

- declaration introduces an identifier and its type or signature
- definition creates the thing itself

Examples:

```cpp
extern int global_count;   // declaration only
int global_count = 0;      // definition

void print_value(int x);   // declaration only
void print_value(int x)    // definition
{
	std::cout << x << '\n';
}
```

Why this matters:

- headers usually contain declarations
- source files often contain definitions
- separating them reduces compile dependencies

## Forward Declarations and Why They Matter

C++ is a strong, statically typed language. The compiler must know what an identifier means before that identifier is used in a context that requires its type or signature.

Practical effect:

- you cannot call a function unless it has been declared before the call point
- you cannot use a class type by name unless the compiler has seen a declaration of that class
- you cannot refer to an external variable unless it has been declared

### Core Rule at the Point of Use

The important rule is not "declare it somewhere". The rule is "declare it before the use point in that translation unit".

Example that fails:

```cpp
int main()
{
  return add(2, 3); // error: add not declared yet
}

int add(int a, int b)
{
  return a + b;
}
```

Fixed with forward declaration:

```cpp
int add(int a, int b); // forward declaration

int main()
{
  return add(2, 3); // OK
}

int add(int a, int b)
{
  return a + b;
}
```

### Forward Declaration in the Same File

You can place declarations near the top of a source file, then define later.

```cpp
class Engine;                 // forward declaration of class
void initialize(Engine& e);   // function declaration

class Engine
{
public:
  void start();
};

void initialize(Engine& e)
{
  e.start();
}
```

This approach is useful for short files or local helper functions.

### Forward Declaration Through a Header

A more common style is to put declarations in a header and include that header where needed.

`math_utils.h`:

```cpp
#pragma once

int add(int a, int b);
```

`math_utils.cpp`:

```cpp
#include "math_utils.h"

int add(int a, int b)
{
  return a + b;
}
```

`main.cpp`:

```cpp
#include "math_utils.h"

int main()
{
  return add(10, 20);
}
```

This is the standard pattern in larger projects.

### Common Forward Declaration Forms

Function:

```cpp
double compute_area(double radius);
```

Class:

```cpp
struct Config;
```

Struct:

```cpp
struct Node;
```

External variable:

```cpp
extern int global_status;
```

Type alias declaration (not forward declaration in the same sense, but still a prior type introduction):

```cpp
using Index = std::size_t;
```

Notes:

- a forward-declared class is an incomplete type until its full definition is seen
- you can use incomplete types for pointers and references
- you cannot access members of an incomplete type until full definition is available

> [!NOTE]
> Forward declarations are also a key API design tool for hiding implementation details from clients.
> Example: a public header can expose only `struct Config;` and pointers/references to it, while the full struct definition lives in a private source/header file.
> This hiding gives practical benefits:
> - smaller public headers and fewer transitive includes
> - reduced client rebuilds when internals change
> - lower coupling between client code and implementation choices
> - better encapsulation and clearer API boundaries
> - easier long-term maintenance and refactoring

## Variable Declaration and Definition

When you write a normal variable with storage, that line is usually both a declaration and a definition.

### Simple Examples

```cpp
int x;          // declaration and definition
double price;   // declaration and definition
const int max_count = 100; // declaration and definition
```

These lines tell the compiler the name and type, and they also create storage.

### When a Variable Declaration Is Not a Definition

The main common case is `extern`.

```cpp
extern int global_value;
```

This says:

- there is an `int` named `global_value`
- its storage is defined somewhere else

Then later in one source file:

```cpp
int global_value = 42;
```

That second line is the definition.

## Function Declaration and Definition

Function declarations and definitions are even more important to separate.

### Simple Examples

```cpp
int add(int a, int b); // declaration

int add(int a, int b)  // definition
{
	return a + b;
}
```

The declaration gives the function name, return type, and parameter types. The definition adds the body.

### Function Prototype Meaning

This declaration:

```cpp
int add(int a, int b);
```

means:

- function name is `add`
- it returns `int`
- it takes two `int` parameters

Parameter names in a declaration are optional:

```cpp
int add(int, int);
```

This still declares the same function.

## How to Read a C++ Declaration

The safest reading method is:

1. find the name
2. read right if possible
3. then read left
4. apply surrounding syntax like `*`, `&`, `[]`, `()`, and qualifiers

Example:

```cpp
const int* ptr;
```

Start at `ptr`:

- `ptr` is a pointer
- pointer to `const int`

So `ptr` means: pointer to const int.

Another example:

```cpp
int& ref = value;
```

Start at `ref`:

- `ref` is a reference
- reference to `int`

## Variable Type Modifiers Before the Name

These are modifiers that appear before the variable name and usually affect storage duration, linkage, mutability, compile-time behavior, or the base type itself.

### `const`

`const` means the object may not be modified through that name.

```cpp
const int limit = 10;
```

Meaning:

- `limit` is an `int`
- its value may not be changed after initialization

Usage:

- constants
- read-only parameters
- documenting immutability intent

### `volatile`

`volatile` tells the compiler that the value may change for reasons outside normal program flow.

```cpp
volatile int hardware_register;
```

Typical usage:

- memory-mapped I/O
- signal handlers in limited cases
- hardware access

It does not mean thread-safe. It does not replace synchronization.

### `constexpr`

`constexpr` means the object can be a constant expression if initialized with a compile-time constant.

```cpp
constexpr int buffer_size = 256;
```

Usage:

- array bounds
- compile-time configuration
- template parameters

### `constinit`

`constinit` means the variable must be initialized at compile time, but is not necessarily immutable.

```cpp
constinit int startup_count = 5;
```

This is mostly useful for static or thread storage duration objects where you want to prevent dynamic initialization surprises.

### `mutable`

`mutable` applies to class data members. It allows modification even inside a `const` member function.

```cpp
class Cache
{
public:
	int value() const
	{
		++hits_;
		return data_;
	}

private:
	int data_ = 10;
	mutable int hits_ = 0;
};
```

Usage:

- caching
- debug counters
- lazily computed internal state

### `static`

For variables, `static` has different meanings depending on location.

File scope:

```cpp
static int file_local_counter = 0;
```

Meaning:

- static storage duration
- internal linkage in that translation unit

Inside a function:

```cpp
void log_once()
{
	static bool first = true;
}
```

Meaning:

- object lives for entire program
- scope is only that function

Inside a class:

```cpp
class Widget
{
public:
	static int count;
};
```

Meaning:

- member belongs to the class, not each object

### `extern`

`extern` usually means declaration without definition for an object with external linkage.

```cpp
extern int global_status;
```

Usage:

- declare globals defined in another source file

### `thread_local`

`thread_local` means each thread gets its own instance.

```cpp
thread_local int request_id = 0;
```

Usage:

- per-thread caches
- thread-specific counters

### `signed`, `unsigned`, `short`, `long`

These modify integer types.

```cpp
unsigned int count = 0;
long total = 1000L;
short code = 7;
long long big = 9000000000LL;
```

Meaning:

- `signed` or `unsigned` changes signedness
- `short` and `long` affect rank and usually size range
- `long long` gives at least 64-bit integer range

## Variable Type Modifiers After the Name

These appear attached to the declarator and often cause most of the confusion.

### Pointers

```cpp
int* ptr;
```

Meaning:

- `ptr` is a pointer to `int`

The `*` belongs to the declarator, not the base type alone.

Important example:

```cpp
int* a, b;
```

This means:

- `a` is pointer to int
- `b` is plain int

This is why many developers prefer one declaration per line.

### References

```cpp
int& ref = value;
const std::string& name = text;
```

Meaning:

- `ref` is reference to int
- `name` is reference to const string

References must be initialized and cannot be reseated.

### Arrays

```cpp
int values[10];
```

Meaning:

- `values` is array of 10 ints

Another example:

```cpp
const char name[6] = "Alice";
```

Meaning:

- `name` is array of 6 const chars

### Pointer Constness Combinations

These forms are very important.

```cpp
const int* p1;
int* const p2 = &value;
const int* const p3 = &value;
```

Interpretation:

- `const int* p1`: pointer to const int
- `int* const p2`: const pointer to int
- `const int* const p3`: const pointer to const int

Rule:

- `const` on the left of `*` usually applies to the pointed object
- `const` on the right of `*` applies to the pointer itself

## Function Type Modifiers Before the Name

These appear before the function name and mostly affect return type, linkage, compile-time properties, or dispatch.

### Return Type

```cpp
int add(int a, int b);
double average(int a, int b);
void print();
```

The type before the name is usually the return type.

Meaning:

- `int add(...)` returns int
- `void print()` returns no value

### `inline`

`inline` on a function means multiple identical definitions across translation units are permitted, and it suggests inlining as an optimization hint.

```cpp
inline int square(int x)
{
	return x * x;
}
```

Modern meaning is more about ODR rules for definitions in headers than about forced machine-code inlining.

### `static` on Functions

At namespace or file scope:

```cpp
static void helper();
```

Meaning:

- internal linkage
- visible only in that translation unit

As a class member:

```cpp
class Math
{
public:
	static int abs(int x);
};
```

Meaning:

- no object instance required
- no `this` pointer

### `constexpr` and `consteval`

```cpp
constexpr int cube(int x)
{
	return x * x * x;
}
```

Meaning:

- function may be evaluated at compile time when arguments are constant expressions

`consteval` is stronger:

```cpp
consteval int always_compile_time(int x)
{
	return x + 1;
}
```

Meaning:

- function must be evaluated at compile time

### `virtual`

```cpp
class Base
{
public:
	virtual void draw();
};
```

Meaning:

- enables dynamic dispatch through base pointers or references

Usage:

- runtime polymorphism

### `explicit`

`explicit` does not apply to ordinary functions. It applies to constructors and conversion operators.

```cpp
class Widget
{
public:
	explicit Widget(int size);
	explicit operator bool() const;
};
```

Meaning:

- prevents unwanted implicit conversions

## Function Type Modifiers After the Name

These appear after the function name and often after the parameter list. They are critical for understanding member function behavior.

### Parameter List

```cpp
int add(int a, int b);
void print(std::string_view text);
```

The parameter list is part of the function type.

Meaning:

- what argument types are accepted
- how overload resolution distinguishes functions

### `const` Member Functions

```cpp
class Report
{
public:
	std::string_view name() const;
};
```

Meaning of `const` after the parameter list:

- applies to the implicit `this` object
- this member function promises not to modify the logical state of the object, except `mutable` members

You can think of it as:

- `name() const` means this function can be called on const objects

### Reference Qualifiers `&` and `&&`

Member functions can be qualified based on whether the object is an lvalue or rvalue.

```cpp
class Text
{
public:
	std::string& value() & { return data_; }
	std::string&& value() && { return std::move(data_); }

private:
	std::string data_;
};
```

Meaning:

- `value() &` can be called on lvalue objects
- `value() &&` can be called on rvalue objects

This is advanced but useful in move-aware APIs.

### `noexcept`

```cpp
void cleanup() noexcept;
```

Meaning:

- function promises not to throw exceptions

This affects optimization and some standard library behavior.

Conditional form:

```cpp
template <typename T>
void swap_value(T& a, T& b) noexcept(noexcept(T(std::move(a))))
{
	T temp = std::move(a);
	a = std::move(b);
	b = std::move(temp);
}
```

### `override` and `final`

```cpp
class Base
{
public:
	virtual void draw();
};

class Derived : public Base
{
public:
	void draw() override;
};
```

Meaning of `override`:

- this function must override a virtual base function
- compiler checks correctness

`final` means no further overriding.

```cpp
class Derived : public Base
{
public:
	void draw() final;
};
```

### Trailing Return Types

Sometimes the return type appears after the parameter list.

```cpp
auto multiply(int a, int b) -> int;
```

Meaning:

- function name is `multiply`
- parameters are `(int, int)`
- return type is `int`

This style is useful when the return type depends on parameter names or complex template expressions.

## Declarations That Often Confuse Returning Developers

```cpp
int* p;
const int* p;
int* const p = &x;
const int& r = x;
int values[5];
int (*fp)(int, int);
int (&arr_ref)[5] = values;
```

Interpretation:

- `int* p`: pointer to int
- `const int* p`: pointer to const int
- `int* const p = &x`: const pointer to int
- `const int& r = x`: reference to const int
- `int values[5]`: array of 5 ints
- `int (*fp)(int, int)`: pointer to function taking two ints and returning int
- `int (&arr_ref)[5] = values`: reference to array of 5 ints

## Examples: Reading Declarations Left to Right and Right to Left

Example 1:

```cpp
const std::string* ptr;
```

Read from name:

- `ptr` is a pointer
- to `const std::string`

Example 2:

```cpp
int (*operation)(int, int);
```

Read from name:

- `operation` is a pointer
- to a function
- taking `(int, int)`
- returning `int`

Example 3:

```cpp
auto get_name() const -> std::string;
```

Read from name:

- `get_name` is a member function
- taking no explicit parameters
- callable on const object
- returning `std::string`

## Common Mistakes

### 1. Thinking `int* a, b;` makes both pointers

It does not.

- `a` is pointer to int
- `b` is int

### 2. Confusing Declaration with Definition

```cpp
extern int total;
```

This does not create storage.

### 3. Misreading `const`

`const` may apply to the object, the pointed-to object, the pointer itself, or a member function.

### 4. Forgetting That Member Function `const` Applies to `this`

```cpp
int size() const;
```

This does not mean the return value is const. It means the function will not modify the object state through `this`.

### 5. Assuming `static` Always Means the Same Thing

Its meaning depends on whether it is used for:

- local variable
- namespace-scope variable
- class member variable
- namespace-scope function
- class member function

## Quick Cheat Sheet

| Pattern                     | Meaning                                                                                                |
| --------------------------- | ------------------------------------------------------------------------------------------------------ |
| `int x;`                    | `x` is an `int`                                                                                        |
| `extern int x;`             | `x` is declared here, defined elsewhere                                                                |
| `const int x = 1;`          | `x` is a read-only `int`                                                                               |
| `int* p;`                   | `p` is a pointer to `int`                                                                              |
| `const int* p;`             | `p` is a pointer to const `int`                                                                        |
| `int* const p = &x;`        | `p` is a const pointer to `int`                                                                        |
| `const int* const p = &x;`  | `p` is a const pointer to const `int`                                                                  |
| `int& r = x;`               | `r` is a reference to `int`                                                                            |
| `const int& r = x;`         | `r` is a reference to const `int`                                                                      |
| `int a[5];`                 | `a` is an array of 5 `int` values                                                                      |
| `int* a, b;`                | `a` is pointer to `int`, `b` is plain `int`                                                            |
| `int add(int, int);`        | function taking two `int`, returning `int`                                                             |
| `void print() noexcept;`    | function returning nothing and promising not to throw                                                  |
| `std::string name() const;` | const member function returning `std::string`                                                          |
| `auto f() -> int;`          | function returning `int` using trailing return type                                                    |
| `int (*fp)(int, int);`      | `fp` is a pointer to function taking two `int` and returning `int`                                     |
| `int (&arr)[5] = values;`   | `arr` is a reference to an array of 5 `int`                                                            |
| `static void helper();`     | function with internal linkage in this translation unit                                                |
| `static int count;`         | class/static storage meaning depends on context; for a class member, one shared variable for the class |

When in doubt, find the name first, then read right, then left.

## Practical Rules of Thumb

- Separate declaration and definition clearly in your mind.
- In headers, prefer declarations; in source files, put definitions unless the function must be inline or templated.
- Read complex declarations starting from the name.
- Use one declaration per line when pointers or references are involved.
- Prefer `const` aggressively for clarity.
- Use `explicit` on constructors and conversion operators unless implicit conversion is intended.
- Use `override` on overriding virtual functions.
- Use `noexcept` when the no-throw guarantee is real and important.
- Prefer trailing return types when the return type depends on template expressions or improves readability.

## Summary

In C++, a declaration introduces a name and type, while a definition creates the actual object or function body. Variables and functions can also carry modifiers before and after the name, and those modifiers change meaning depending on context.

The key skill is to stop reading declarations as flat text. Instead, find the name first, then read outward. Once you do that consistently, even complex C++ declarations become manageable.
