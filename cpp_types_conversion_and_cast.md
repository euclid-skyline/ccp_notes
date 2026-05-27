# C++ Types Conversion and Cast Notes

## Table of Contents

- [C++ Types Conversion and Cast Notes](#c-types-conversion-and-cast-notes)
  - [Table of Contents](#table-of-contents)
  - [Purpose](#purpose)
  - [What Type Conversion Means](#what-type-conversion-means)
  - [Default Conversion Behavior in C++](#default-conversion-behavior-in-c)
  - [Integer Conversions and Promotions](#integer-conversions-and-promotions)
    - [Usual Integer Promotions](#usual-integer-promotions)
    - [Signed and Unsigned Mixing](#signed-and-unsigned-mixing)
    - [Integer Truncation](#integer-truncation)
  - [Floating-Point Conversions](#floating-point-conversions)
    - [Integer to Floating-Point](#integer-to-floating-point)
    - [Floating-Point to Integer](#floating-point-to-integer)
    - [Floating-Point Narrowing](#floating-point-narrowing)
    - [Infinity and NaN](#infinity-and-nan)
  - [Boolean, Character, and Enum Conversions](#boolean-character-and-enum-conversions)
    - [Boolean Conversions](#boolean-conversions)
    - [Character Conversions](#character-conversions)
    - [Enumeration Conversions](#enumeration-conversions)
  - [User-Defined Conversions](#user-defined-conversions)
    - [Converting Constructor](#converting-constructor)
    - [Conversion Operator](#conversion-operator)
  - [Narrowing Conversions](#narrowing-conversions)
  - [Type Casting in C++](#type-casting-in-c)
  - [The Four C++ Casts](#the-four-c-casts)
    - [`static_cast`](#static_cast)
    - [`dynamic_cast`](#dynamic_cast)
    - [`const_cast`](#const_cast)
    - [`reinterpret_cast`](#reinterpret_cast)
  - [C-Style Casts and Why to Avoid Them](#c-style-casts-and-why-to-avoid-them)
  - [Common Pitfalls](#common-pitfalls)
    - [1. Silent Loss of Data](#1-silent-loss-of-data)
    - [2. Signed and Unsigned Comparisons](#2-signed-and-unsigned-comparisons)
    - [3. Assuming `static_cast` Makes Unsafe Conversions Safe](#3-assuming-static_cast-makes-unsafe-conversions-safe)
    - [4. Removing `const` to Work Around API Design](#4-removing-const-to-work-around-api-design)
    - [5. Using `reinterpret_cast` for Normal Conversions](#5-using-reinterpret_cast-for-normal-conversions)
  - [Practical Rules of Thumb](#practical-rules-of-thumb)
  - [Summary](#summary)

## Purpose

This document explains how C++ converts values from one type to another, what the default rules are, and how explicit casting works. It is intended as a practical reference, not just a list of definitions.

The most important idea is this: C++ will often convert types automatically, but automatic conversion is not always safe. Good C++ code makes conversions intentional and visible.

## What Type Conversion Means

Type conversion is the process of turning a value of one type into another type. In C++, this can happen in two ways:

- implicitly, when the compiler does it for you
- explicitly, when you write a cast

Example:

```cpp
int a = 10;
double b = a;      // implicit conversion from int to double
double c = double(a); // explicit conversion
```

The value `10` is the same, but the type changes. That type change matters because the new type may have different range, precision, or meaning.

## Default Conversion Behavior in C++

By default, C++ tries to convert values automatically when it believes the conversion is reasonable. This is called implicit conversion.

Common places where implicit conversion happens:

- assignment to a variable of a different type
- passing an argument to a function
- returning a value from a function
- arithmetic expressions with mixed types
- comparisons between different types

Example:

```cpp
double price = 19;
int count = 3.7;
```

Here C++ applies default rules:

- `19` becomes `19.0` when stored in `double`
- `3.7` becomes `3` when stored in `int`

That second conversion loses the fractional part. This is not an error, but it is a behavior you must understand.

Default conversions are generally safe only when:

- the destination type can represent the source value without loss
- the intent is obvious
- no hidden truncation or sign change occurs

## Integer Conversions and Promotions

Integer conversions are one of the most common sources of bugs in C++.

### Usual Integer Promotions

Smaller integer types are often promoted to `int` or `unsigned int` before arithmetic.

Example:

```cpp
char a = 10;
char b = 20;
auto sum = a + b;
```

Although `a` and `b` are `char`, the addition is usually performed as `int`. The result type of `a + b` is not `char`.

### Signed and Unsigned Mixing

Mixing signed and unsigned integers is dangerous.

```cpp
int x = -1;
unsigned int y = 1;

if (x < y)
{
	// surprising result
}
```

In this comparison, `x` may be converted to unsigned, which can turn `-1` into a very large positive value. The comparison then behaves differently from what many developers expect.

Recommendation:

- avoid mixing signed and unsigned values unless necessary
- use the same signedness in a given computation when possible
- prefer `std::size_t` only for sizes and indexes tied to container APIs

### Integer Truncation

When a value does not fit in the destination type, the result may be truncated or wrapped depending on the types involved.

```cpp
int big = 300;
unsigned char c = big;
```

If `unsigned char` cannot hold `300`, the value will be reduced in a way that depends on the representation. This is usually a bug unless you intentionally want the low bits.

## Floating-Point Conversions

Floating-point conversions can lose precision.

### Integer to Floating-Point

```cpp
int n = 42;
double d = n;
```

This is usually safe for small integers.

### Floating-Point to Integer

```cpp
double pi = 3.14159;
int n = pi;
```

The fractional part is discarded, so `n` becomes `3`.

### Floating-Point Narrowing

```cpp
double x = 1.23456789012345;
float y = x;
```

`float` has less precision than `double`, so some digits may be lost.

### Infinity and NaN

Special floating-point values may also be converted.

```cpp
double inf = 1.0 / 0.0;
int n = static_cast<int>(inf); // not a normal value
```

Such conversions are usually problematic and may be undefined or implementation-defined depending on the situation.

## Boolean, Character, and Enum Conversions

### Boolean Conversions

Any scalar type can usually convert to `bool`.

```cpp
bool a = 0;      // false
bool b = 42;     // true
bool c = -7;     // true
bool d = 0.0;    // false
```

Rule of thumb:

- zero becomes `false`
- non-zero becomes `true`

### Character Conversions

Characters are small integer types, so they participate in integer promotions.

```cpp
char ch = 'A';
int code = ch;
```

The character value is converted to its numeric code point value in the execution character set.

### Enumeration Conversions

Unscoped enums convert more freely than scoped enums.

```cpp
enum Color { Red, Green, Blue };
Color c = Red;
int n = c; // allowed for unscoped enum
```

Scoped enums are safer.

```cpp
enum class TrafficLight { Red, Yellow, Green };
TrafficLight t = TrafficLight::Red;
// int n = t; // error: no implicit conversion
```

This is one reason `enum class` is preferred in modern C++.

## User-Defined Conversions

Classes can define conversions too.

### Converting Constructor

A constructor with one argument can act as a conversion path.

```cpp
class Meter
{
public:
	Meter(double value) : value_(value) {}

private:
	double value_;
};

Meter m = 2.5; // implicit conversion through constructor
```

If you do not want this behavior, mark the constructor `explicit`.

```cpp
class Meter
{
public:
	explicit Meter(double value) : value_(value) {}
};

// Meter m = 2.5; // error
Meter m(2.5);     // OK
```

### Conversion Operator

Classes can also define conversion functions.

```cpp
class Flag
{
public:
	explicit operator bool() const { return enabled_; }

private:
	bool enabled_ = true;
};
```

This lets the object convert to `bool` in controlled ways.

## Narrowing Conversions

A narrowing conversion is a conversion that can lose information, such as:

- `double` to `int`
- `int` to `char`
- large integer to smaller integer
- `double` to `float`

Example:

```cpp
int a = 9.8; // narrowing, fractional part lost
```

Modern C++ initialization can help catch some narrowing.

```cpp
int a{9.8}; // error in list initialization
```

This is one of the reasons brace initialization is often safer than assignment-style initialization.

## Type Casting in C++

Type casting is the explicit version of type conversion. When you cast, you tell the compiler that you want a value treated as another type.

This can be useful, but it should be used carefully because it can hide bugs if overused.

Casting is most appropriate when:

- the conversion is intentional and visible
- you need to choose between overloaded functions
- you are working with APIs that require a specific type
- you are bridging between related types or low-level representations

## The Four C++ Casts

C++ provides four main named cast forms:

- `static_cast`
- `dynamic_cast`
- `const_cast`
- `reinterpret_cast`

Each one has a different purpose.

### `static_cast`

`static_cast` is the general-purpose cast for well-defined compile-time conversions.

Common uses:

- numeric conversions
- converting `void*` to a typed pointer in limited contexts
- upcasting in class hierarchies
- converting enum values to integers

Example:

```cpp
double x = 3.9;
int n = static_cast<int>(x); // n becomes 3
```

Class hierarchy example:

```cpp
class Base {};
class Derived : public Base {};

Derived d;
Base* b = static_cast<Base*>(&d); // upcast is safe
```

Use `static_cast` when the conversion is valid and you want the compiler to enforce that it is an explicit decision.

### `dynamic_cast`

`dynamic_cast` is used for safe downcasting in polymorphic class hierarchies.

It requires at least one virtual function in the base class.

```cpp
class Base
{
public:
	virtual ~Base() = default;
};

class Derived : public Base
{
public:
	void run() {}
};

Base* b = new Derived();
Derived* d = dynamic_cast<Derived*>(b);
```

If the object is really a `Derived`, the cast succeeds. If not, the result is `nullptr` for pointer casts.

This cast is runtime-checked, so it is slower than `static_cast`, but safer for downcasting.

### `const_cast`

`const_cast` adds or removes `const` or `volatile` qualifiers.

Example:

```cpp
void print_text(const char* text)
{
	char* mutable_text = const_cast<char*>(text);
	// use with care
}
```

Important rule:

- you may remove constness only if the original object was not actually const
- modifying a truly const object through a cast leads to undefined behavior

`const_cast` should be rare. It is usually a sign that the API design needs review.

### `reinterpret_cast`

`reinterpret_cast` is the lowest-level named cast. It performs bit-level or representation-level reinterpretation.

Example:

```cpp
std::uintptr_t p = reinterpret_cast<std::uintptr_t>(some_pointer);
```

This cast is powerful but dangerous because it often bypasses type safety.

Use it only when you truly need to work with raw representations, such as:

- low-level systems programming
- hardware interfaces
- serialization or debugging utilities
- interoperability with APIs that require exact bit patterns

Do not use `reinterpret_cast` as a general conversion mechanism.

## C-Style Casts and Why to Avoid Them

You can still write old-style casts like this:

```cpp
int n = (int)3.14;
```

But C-style casts are discouraged because they are less explicit about intent. A C-style cast can behave like different named casts depending on the situation, which makes it harder to reason about.

Prefer named casts because they show the kind of conversion being requested.

Better:

```cpp
int n = static_cast<int>(3.14);
```

## Common Pitfalls

### 1. Silent Loss of Data

```cpp
double d = 9.99;
int n = d;
```

The decimal part disappears.

### 2. Signed and Unsigned Comparisons

```cpp
int a = -1;
unsigned int b = 1;
if (a < b) { }
```

This can behave unexpectedly.

### 3. Assuming `static_cast` Makes Unsafe Conversions Safe

`static_cast` does not validate semantic correctness. It only says the compiler should perform a specific kind of conversion.

### 4. Removing `const` to Work Around API Design

If you find yourself using `const_cast` often, the design may be wrong.

### 5. Using `reinterpret_cast` for Normal Conversions

That is usually a design smell.

## Practical Rules of Thumb

- Prefer implicit conversion only when it is obvious and lossless.
- Use `explicit` on constructors and conversion operators unless implicit conversion is really desired.
- Avoid signed/unsigned mixing in arithmetic and comparisons.
- Use brace initialization when you want narrowing checks.
- Use `static_cast` for normal explicit conversions.
- Use `dynamic_cast` for polymorphic downcasting.
- Use `const_cast` only when working with APIs that require it and you understand the lifetime and mutability rules.
- Use `reinterpret_cast` only for low-level, representation-oriented tasks.
- Avoid C-style casts in modern code.

## Summary

C++ conversion rules are powerful, but they can also be subtle. The compiler will do many conversions automatically, yet automatic conversion is not always what you want. The safest style is to keep conversions intentional, visible, and minimal.

The most useful habit is simple: when a conversion matters, make it explicit and make sure you understand what information may be lost.
