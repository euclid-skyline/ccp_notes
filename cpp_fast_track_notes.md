# C++ Fast Track Notes <!-- omit in toc -->

## Table of Contents <!-- omit in toc -->

- [Purpose](#purpose)
- [0) C++ Language Main Features](#0-c-language-main-features)
  - [Strongly Typed by Design](#strongly-typed-by-design)
  - [Declaration Before Use: First Reading Rule](#declaration-before-use-first-reading-rule)
  - [Forward Declarations and References Across Files](#forward-declarations-and-references-across-files)
  - [C++ File Anatomy](#c-file-anatomy)
    - [Header File and Implementation File](#header-file-and-implementation-file)
    - [Header-Only Project](#header-only-project)
- [1) Core Reading Model: Lifetime, Ownership, and Type Intent](#1-core-reading-model-lifetime-ownership-and-type-intent)
  - [C++ Built-in Types](#c-built-in-types)
  - [Introducing Types in C++](#introducing-types-in-c)
    - [`struct` and `class`: Same Power, Different Defaults](#struct-and-class-same-power-different-defaults)
    - [`union`: One Storage Region, Multiple Interpretations](#union-one-storage-region-multiple-interpretations)
    - [`enum` and `enum class`: Named Constant Sets](#enum-and-enum-class-named-constant-sets)
    - [Template Types: Type Families](#template-types-type-families)
    - [Type Aliases with `using` and `typedef`](#type-aliases-with-using-and-typedef)
  - [C++ String Handling](#c-string-handling)
  - [Core Reading Deep Dive: The Concepts You Need First](#core-reading-deep-dive-the-concepts-you-need-first)
    - [Class Reading Model: Objects, Constructors, and Destructors](#class-reading-model-objects-constructors-and-destructors)
    - [Object Lifetime and Ownership](#object-lifetime-and-ownership)
      - [Why Raw Pointer Ownership Causes Problems](#why-raw-pointer-ownership-causes-problems)
      - [How Smart Pointers Fix Ownership Issues](#how-smart-pointers-fix-ownership-issues)
    - [Const Correctness](#const-correctness)
    - [RAII (Resource Acquisition Is Initialization)](#raii-resource-acquisition-is-initialization)
    - [Separation of Interface and Implementation](#separation-of-interface-and-implementation)
      - [Example 1: Classic Header/Source Split](#example-1-classic-headersource-split)
      - [Example 2: Intentional Hiding with pImpl](#example-2-intentional-hiding-with-pimpl)
      - [Interface Design Intent](#interface-design-intent)
    - [Template Basics](#template-basics)
    - [STL Containers and Algorithms](#stl-containers-and-algorithms)
    - [Exception Handling for Exceptional Paths](#exception-handling-for-exceptional-paths)
  - [Rule of Three to Rule of Five to Rule of Zero](#rule-of-three-to-rule-of-five-to-rule-of-zero)
    - [Rule of Three](#rule-of-three)
    - [Rule of Five](#rule-of-five)
    - [Rule of Zero](#rule-of-zero)
- [2) Modern C++ Reading Mindset](#2-modern-c-reading-mindset)
- [3) C++11: The New Baseline of Modern C++](#3-c11-the-new-baseline-of-modern-c)
  - [`auto` and Type Deduction](#auto-and-type-deduction)
  - [`nullptr` and Type Safety](#nullptr-and-type-safety)
  - [Range-Based `for`](#range-based-for)
  - [Lambdas](#lambdas)
  - [Move Semantics](#move-semantics)
  - [Smart Pointers and Ownership](#smart-pointers-and-ownership)
- [4) C++14: C++11, But Smoother](#4-c14-c11-but-smoother)
- [5) C++17: Everyday Productivity Standard](#5-c17-everyday-productivity-standard)
  - [Structured Bindings](#structured-bindings)
  - [`if constexpr`](#if-constexpr)
  - [`std::optional`, `std::variant`, `std::any`](#stdoptional-stdvariant-stdany)
  - [`std::string_view`](#stdstringview)
  - [`std::filesystem`](#stdfilesystem)
- [6) C++20: A Second Major Inflection Point](#6-c20-a-second-major-inflection-point)
  - [Concepts and `requires`](#concepts-and-requires)
  - [Ranges](#ranges)
  - [`std::span`](#stdspan)
  - [`std::format`](#stdformat)
  - [Coroutines and Modules](#coroutines-and-modules)
- [7) C++23: Maturity and Gap-Filling](#7-c23-maturity-and-gap-filling)
  - [`std::expected`](#stdexpected)
  - [`std::print` and `std::println`](#stdprint-and-stdprintln)
  - [`std::mdspan`](#stdmdspan)
  - [Language Cleanups](#language-cleanups)
- [8) Language Features vs Library Features: Why Your Toolchain Matters](#8-language-features-vs-library-features-why-your-toolchain-matters)
- [9) How to Read and Modernize Legacy Code](#9-how-to-read-and-modernize-legacy-code)
  - [Boundary Modernization Pattern](#boundary-modernization-pattern)
  - [Replace the Most Risky Patterns First](#replace-the-most-risky-patterns-first)
- [10) Recommended Fast-Track Plan (Practical)](#10-recommended-fast-track-plan-practical)
- [11) Final Takeaway](#11-final-takeaway)
- [12) A Full Old-to-Modern Refactor Example](#12-a-full-old-to-modern-refactor-example)
  - [Older Style (Common in C++03-era Code)](#older-style-common-in-c03-era-code)
  - [Modernized Style](#modernized-style)
- [13) Common Reading Mistakes in C++](#13-common-reading-mistakes-in-c)
  - [Mistake 1: Keeping Ownership Ambiguous](#mistake-1-keeping-ownership-ambiguous)
  - [Mistake 2: Using Sentinel Values for Everything](#mistake-2-using-sentinel-values-for-everything)
  - [Mistake 3: Overusing Manual Loops](#mistake-3-overusing-manual-loops)
  - [Mistake 4: Treating `auto` as Either Always Good or Always Bad](#mistake-4-treating-auto-as-either-always-good-or-always-bad)
  - [Mistake 5: Ignoring Toolchain Layering](#mistake-5-ignoring-toolchain-layering)
- [14) Practical Review Exercises (One Weekend Plan)](#14-practical-review-exercises-one-weekend-plan)
  - [Exercise 1: Ownership Pass](#exercise-1-ownership-pass)
  - [Exercise 2: Interface Pass](#exercise-2-interface-pass)
  - [Exercise 3: Return Type Pass](#exercise-3-return-type-pass)
  - [Exercise 4: Algorithm Pass](#exercise-4-algorithm-pass)
  - [Exercise 5: Template Pass](#exercise-5-template-pass)
- [15) Closing Advice for Long-Term Success](#15-closing-advice-for-long-term-success)
## Purpose

This document is for a developer transitioning from general object-oriented programming to C++ object-oriented design, with optional exposure to functional-programming ideas, who needs to review and understand production C++ source code with confidence.

It is not a full C++ course and not a glossary. It is a fast-track reading guide: enough core concepts and patterns to make real code reviews easier, especially around class design, constructor/destructor behavior, ownership, pointers, references, and modern library usage.

The central idea is simple: treat C++ as familiar OOP with explicit lifetime rules. Once you read ownership and object lifecycle correctly, most source code becomes predictable.

---

## 0) C++ Language Main Features

In large C++ codebases, the most common review-time blocker is the same question: "why does this fail?" The code may look reasonable locally, but failure usually comes from contracts defined outside the current function or file. In practice, this section exists to make that failure pattern predictable.

Two answers resolve most of these failures in source code:

1. C++ is strongly type-centric: each expression and call must satisfy exact type rules.
2. C++ is declaration-driven: names cannot be used until their declarations are visible at the point of use.

### Strongly Typed by Design

C++ is type-centric. Every variable, function, and expression has a type contract checked at compile time.

- values are not implicitly interchangeable in many contexts
- function signatures define exact parameter and return expectations
- overload resolution depends on types, cv-qualifiers, and references

> [!NOTE]
> `cv` stands for `const` and `volatile`.
> `cv-qualifiers` are type qualifiers that add `const` and/or `volatile` constraints, which affect valid operations and overload selection.

Fast reading rule: start by identifying types on function boundaries, then inspect implementation.

```cpp
int add(int a, int b);
double add(double a, double b);

add(1, 2);      // calls int overload
add(1.0, 2.0);  // calls double overload
```

### Declaration Before Use: First Reading Rule

In C++, a variable, type, or function must be declared before any use that requires compiler knowledge of it.

```cpp
int main()
{
    return multiply(2, 3); // error if declaration is not visible yet
}

int multiply(int a, int b)
{
    return a * b;
}
```

Fixed with a forward declaration:

```cpp
int multiply(int a, int b); // declaration first

int main()
{
    return multiply(2, 3);
}

int multiply(int a, int b)
{
    return a * b;
}
```

### Forward Declarations and References Across Files

Forward declarations let you reference a type without including its full definition immediately.

```cpp
class Engine; // forward declaration

void inspect(const Engine& e); // allowed: reference to incomplete type
```

Practical review benefit:

- smaller headers
- fewer rebuild cascades
- clearer dependency boundaries

You usually need the full type definition only when accessing members, allocating by value, or needing size/layout.

### C++ File Anatomy

The two ideas above explain the usual C++ file layout: declarations must be visible before use, and the compiler needs enough type information at the point of use. That is why many projects split code across headers and implementation files, while some small libraries keep everything in headers.

#### Header File and Implementation File

The normal structure is:

- header file (`.h`, `.hpp`): declares the interface
- implementation file (`.cpp`): defines the functions and methods

Example:

`widget.h`

```cpp
#pragma once

#include <string>

class Widget
{
public:
    explicit Widget(std::string name);
    void render() const;

private:
    std::string name_;
};
```

`widget.cpp`

```cpp
#include "widget.h"
#include <iostream>

Widget::Widget(std::string name) : name_(std::move(name)) {}

void Widget::render() const
{
    std::cout << name_ << '\n';
}
```

How to read this structure:

- the header tells you what the type promises
- the implementation file tells you how it fulfills that promise
- callers include the header, not the `.cpp`
- implementation changes are often local to the `.cpp` file

#### Header-Only Project

A header-only project keeps declarations and definitions in header files. This is common for templates and small reusable utilities.

```cpp
#pragma once

template <typename T>
T square(T value)
{
    return value * value;
}
```

Why header-only is used:

- templates usually must be fully visible where they are instantiated
- small utilities are easier to distribute as a single header
- it can simplify build setup for small libraries

Tradeoffs:

- more code is visible to every file that includes the header
- compile times can grow as headers get larger
- implementation details are harder to hide

Reading rule: if the project is header-only, expect more inline function definitions and more template code directly in headers. If it uses headers plus `.cpp` files, expect a clearer interface/implementation split.

---

## 1) Core Reading Model: Lifetime, Ownership, and Type Intent

If you are moving from general OOP into C++, class structure and interfaces will still feel familiar. The key difference is that lifetime, ownership, and type intent are explicit in code and must be read intentionally.

In this guide, these terms mean:

- Lifetime: when an object is created, how long it stays valid, and when it is destroyed.
- Ownership: which object or scope is responsible for releasing a resource.
- Type intent: what the type in a function signature communicates about usage, such as mutability, nullability, and transfer of responsibility.

Modern C++ did not change those fundamentals. It gave clearer language and library tools to express them.

### C++ Built-in Types

Before user-defined types, C++ gives core built-in types that appear everywhere in source code reviews.

```cpp
bool ok = true;
char ch = 'A';
int count = 42;
unsigned int flags = 0u;
float ratio = 0.5f;
double score = 99.8;
void* raw = nullptr;
```

Reading model for built-in types:

- integral types (`short`, `int`, `long`, `long long`, signed/unsigned forms) represent whole numbers
- floating-point types (`float`, `double`, `long double`) represent decimal values with precision tradeoffs
- character types (`char`, `wchar_t`, `char8_t`, `char16_t`, `char32_t`) represent code units
- `bool` represents logical truth values
- `void` means no value (`void` return) or generic address form when used as `void*`

In production code, most interfaces combine built-in and user-defined types. Read the built-in part first to understand numeric range, mutability qualifiers, and pointer/reference behavior before following domain-specific types.

### Introducing Types in C++

The core language options are:

- `struct`
- `class`
- `union`
- `enum` and `enum class`
- type aliases (`using`, `typedef`)

#### `struct` and `class`: Same Power, Different Defaults

`struct` and `class` are almost identical in capabilities. The key default differences are:

- `struct`: members and inheritance are `public` by default
- `class`: members and inheritance are `private` by default

Use guide in real code reviews:

- use `struct` for simple data carriers and value objects with mostly public state
- use `class` for behavior-centric types with encapsulation boundaries

```cpp
struct Point
{
    double x{0.0};
    double y{0.0};
};

class BankAccount
{
public:
    explicit BankAccount(double initial) : balance_(initial) {}
    void deposit(double amount) { balance_ += amount; }
    double balance() const { return balance_; }

private:
    double balance_;
};
```

#### `union`: One Storage Region, Multiple Interpretations

A `union` stores different fields in the same memory location, one active member at a time.

```cpp
union NumberView
{
    int i;
    float f;
};
```

Use it when memory layout control is required. In most application code, prefer safer alternatives such as `std::variant` unless low-level constraints require a union.

#### `enum` and `enum class`: Named Constant Sets

`enum` creates named constants. `enum class` is strongly scoped and avoids accidental implicit conversions.

```cpp
enum OldState { idle, running };
enum class State { Idle, Running };

State s = State::Idle; // scoped and explicit
```

Review preference: prefer `enum class` for modern code readability and type safety.

#### Template Types: Type Families

Templates define type families, not just one concrete type.

```cpp
template <typename T>
struct Box
{
    T value;
};

Box<int> a{42};
Box<std::string> b{"ok"};
```

Use this model when one design should work for many element types. For fast code review, read the instantiated type first (`Box<int>`, `Box<User>`) and then map back to the template definition.

#### Type Aliases with `using` and `typedef`

Aliases give existing types better names. They do not create a brand-new distinct runtime type.

```cpp
using UserId = std::uint64_t;
typedef std::vector<std::string> StringList;
```

`using` is generally preferred in modern C++ because it is clearer and works better with templates.

### C++ String Handling

C++ projects usually mix several string styles. Reading code becomes easier when you identify ownership and mutability first.

```cpp
const char* c_text = "hello";           // C-style string (null-terminated)
std::string text = "hello";             // owning dynamic string
std::string_view view = text;            // non-owning read-only view
```

Common methods of string handling in C++:

- `const char*` / `char*`: C-style strings, common in legacy APIs and interop code.
- `std::string`: default modern choice for owned and mutable text.
- `std::string_view`: lightweight read-only view when you do not need ownership.
- string literals (`"..."`, `R"(...)"`): compile-time text constants.

Practical reading tips:

- if a function takes `std::string_view`, it usually reads text only
- if a function takes `std::string` by value, it may store or move ownership
- if you see `char*`, check whether code expects writable buffers

Example API shapes:

```cpp
void log_line(std::string_view line);        // read-only, non-owning
void set_name(std::string name);             // takes ownership/move-friendly
void fill_buffer(char* out, std::size_t n);  // writable C-style buffer
```

### Core Reading Deep Dive: The Concepts You Need First

#### Class Reading Model: Objects, Constructors, and Destructors

For source-code reading, start from class lifecycle before algorithm details. In most C++ projects, understanding construction and destruction rules explains more bugs than reading the method bodies first.

```cpp
class Session
{
public:
    Session();                                 // default constructor
    explicit Session(std::string user);        // value constructor
    Session(const Session& other);             // copy constructor
    Session(Session&& other) noexcept;         // move constructor

    Session& operator=(const Session& other);  // copy assignment
    Session& operator=(Session&& other) noexcept; // move assignment

    ~Session();                                // destructor

private:
    std::string user_;
    std::vector<int> cache_;
};
```

How to read constructor/destructor types quickly:

- default constructor creates an object with default state
- value constructor initializes from explicit inputs
- copy constructor builds a new object from an existing lvalue object
- move constructor builds a new object by transferring resources from an rvalue
- destructor releases resources at end of lifetime

Call-site mapping:

```cpp
Session a;                        // default constructor
Session b("alice");              // value constructor
Session c = b;                    // copy constructor
Session d = std::move(b);         // move constructor
```

Other constructor forms you will frequently see in production code:

```cpp
class Config
{
public:
    Config() = default;                    // defaulted constructor
    Config(int retries) : Config(retries, true) {} // delegating constructor
    Config(int retries, bool verbose) : retries_(retries), verbose_(verbose) {}

    Config(const Config&) = delete;        // non-copyable type
    Config(Config&&) noexcept = default;   // movable

private:
    int retries_{3};
    bool verbose_{false};
};
```

Reading rule for these forms:

- `= default` means compiler-generated behavior is intended
- `= delete` means that operation is intentionally forbidden
- delegating constructors centralize initialization logic and reduce duplication

Two practical notes for code review:

- a single-parameter constructor should often be `explicit` to avoid accidental implicit conversions
- if a class owns resources directly, copy/move/destructor behavior must form one coherent contract

When class members are RAII types (`std::string`, `std::vector`, `std::unique_ptr`), prefer defaulted special members and simpler class code.

#### Object Lifetime and Ownership

If there is one subject to re-master before anything else, it is object lifetime and ownership.

Object lifetime answers: when is an object created and when is it destroyed?
Ownership answers: who is responsible for destroying the resource owned by that object?

Most hard C++ bugs still come from weak ownership models:

- double delete
- use-after-free
- dangling references
- hidden shared ownership

Modern C++ expects ownership to be explicit in types and APIs.

- value types (`std::string`, `std::vector`) own their data
- `std::unique_ptr<T>` expresses unique ownership
- `std::shared_ptr<T>` expresses shared ownership
- raw pointers are usually non-owning observers in modern interfaces

If you keep ownership clear, the rest of modern C++ becomes much easier.

##### Why Raw Pointer Ownership Causes Problems

Raw pointers are not inherently bad. The problem appears when raw pointers are used to represent ownership, because the type `T*` does not answer key questions:

- Who owns this object?
- Who must delete it?
- Can ownership be transferred?
- Is this pointer sharing ownership with others?

When those rules are undocumented or inconsistent, common bugs appear.

Example 1: memory leak through forgotten delete

```cpp
Widget* create_widget()
{
    Widget* w = new Widget();
    if (!w->init())
    {
        return nullptr; // leak: w was allocated but never deleted
    }
    return w;
}
```

The issue is not syntax. The issue is ownership tracking during control-flow changes.

Resolved with `std::unique_ptr`:

```cpp
std::unique_ptr<Widget> create_widget()
{
    auto w = std::make_unique<Widget>();
    if (!w->init())
    {
        return nullptr; // safe: unique_ptr destroys object automatically
    }
    return w; // ownership transfer is explicit
}
```

Example 2: double delete from ambiguous ownership

```cpp
void set_widget(Widget* w)
{
    widget_ = w;
}

void clear_widget()
{
    delete widget_;
    widget_ = nullptr;
}
```

If caller also deletes `w`, you get double delete. If caller assumes callee deletes and callee does not, you get leak. Raw pointer type does not make the contract explicit.

Resolved with explicit unique ownership:

```cpp
void set_widget(std::unique_ptr<Widget> w)
{
    widget_ = std::move(w);
}

void clear_widget()
{
    widget_.reset();
}
```

Now ownership transfer is part of the function signature.

Example 3: use-after-free in shared access

```cpp
Widget* w = new Widget();
register_callback([w]() { w->tick(); });
delete w; // callback now holds dangling pointer
```

If callbacks or subsystems share lifetime, manual lifetime coordination is fragile.

Resolved with shared ownership and safe observation:

```cpp
auto w = std::make_shared<Widget>();
std::weak_ptr<Widget> weak_w = w;

register_callback([weak_w]()
{
    if (auto locked = weak_w.lock())
    {
        locked->tick();
    }
});
```

With `weak_ptr`, callback code can safely check whether object lifetime is still valid.

##### How Smart Pointers Fix Ownership Issues

`std::unique_ptr<T>`:

- one owner
- no accidental copy of ownership
- automatic destruction at scope end
- explicit ownership transfer via `std::move`

`std::shared_ptr<T>`:

- shared ownership
- automatic destruction when last owner disappears
- useful for graph-like or callback-driven lifetimes

`std::weak_ptr<T>`:

- non-owning reference to a `shared_ptr`-managed object
- avoids cycles and use-after-free in observers
- safe access through `lock()`

Practical modern rule:

- Use raw pointers and references for non-owning access.
- Use `unique_ptr` by default for ownership.
- Use `shared_ptr` only when ownership is truly shared.
- Use `weak_ptr` for observers of shared lifetime.

What non-owning access means in practice:

- the function can use the object
- the function must not delete or free the object
- lifetime remains the caller or owner responsibility

Example A: non-owning reference (cannot be null)

```cpp
void print_title(const Document& doc)
{
    std::cout << doc.title() << '\n';
}
```

`print_title` only observes `doc`; it does not own `doc` and cannot destroy it.

Example B: non-owning raw pointer observer (can be null)

```cpp
void maybe_tick(Widget* w)
{
    if (w)
    {
        w->tick();
    }
}
```

`maybe_tick` can use `w` if present, but it must never call `delete w` because it is not the owner.

Pointer-to-pointer appears in legacy code and C APIs:

```cpp
void replace_widget(Widget** slot, Widget* next)
{
    *slot = next; // updates caller's pointer variable
}
```

When reading this style, `Widget**` usually means "this function can rebind the caller's pointer". In modern C++, this is often replaced with references or smart-pointer parameters for clarity.

Quick mapping for common signatures during code review:

- `T*` or `const T*`: non-owning, nullable access
- `T&` or `const T&`: non-owning, non-null access
- `std::unique_ptr<T>` by value: ownership transfer
- `std::unique_ptr<T>&`: modify current owner without transfer
- `std::shared_ptr<T>` by value: shared ownership participation

References and reference-of-reference note:

- direct reference-to-reference declarations are not allowed in source code (`T& &`)
- in templates, reference collapsing rules apply and are common in forwarding code

```cpp
template <typename T>
void relay(T&& value) // forwarding reference
{
    consume(std::forward<T>(value));
}
```

Practical reading rule:

- if `T` is deduced as `U&`, then `T&&` becomes `U&`
- if `T` is deduced as `U`, then `T&&` stays `U&&`

You do not need to memorize all template details for review work. Just remember that forwarding code preserves whether the original argument was an lvalue or rvalue.

#### Const Correctness

`const` is still central to clean API design. It communicates intent and enables compiler checks.

- `const T&` means read-only reference access
- `T* const` means pointer value cannot change
- `const T*` means pointed-to value cannot change through that pointer
- `const` member functions promise not to mutate logical object state

Good const usage improves readability and maintainability. It is not just a style preference.

```cpp
class Report
{
public:
    std::string_view name() const { return name_; }

private:
    std::string name_;
};
```

#### RAII (Resource Acquisition Is Initialization)

RAII is still one of the most important C++ design principles.

The idea is simple: tie resource lifetime to object lifetime. Acquire in construction, release in destruction.

```cpp
#include <cstdio>
#include <stdexcept>

class FileWriter
{
public:
    explicit FileWriter(const char* path)
        : file_(std::fopen(path, "a"))
    {
        if (!file_)
        {
            throw std::runtime_error("failed to open file");
        }
    }

    ~FileWriter()
    {
        if (file_)
        {
            std::fclose(file_); // resource is released when object dies
        }
    }

    FileWriter(const FileWriter&) = delete;
    FileWriter& operator=(const FileWriter&) = delete;

    void write_line(const char* text)
    {
        std::fprintf(file_, "%s\n", text);
    }

private:
    std::FILE* file_;
};

void save_log(bool dry_run)
{
    FileWriter writer("app.log"); // acquire file resource here

    if (dry_run)
    {
        return; // writer goes out of scope, destructor closes file automatically
    }

    writer.write_line("started");
}
```

This is RAII in practice: constructor acquires, destructor releases. Even with early return or exceptions, cleanup still happens because object lifetime is tied to scope.

#### Separation of Interface and Implementation

Separation of interface and implementation is still one of the most practical C++ design habits. It answers a simple question:

- What does this module promise to users? (interface)
- How does it actually do it? (implementation)

The interface should be stable and minimal. The implementation can change freely as long as the interface contract remains valid.

Why this matters in real projects:

- Faster builds: fewer includes and less recompilation ripple.
- Easier maintenance: internal changes do not force caller changes.
- Better encapsulation: users depend on behavior, not private details.
- Cleaner APIs: public surface is intentional rather than accidental.
- Better binary compatibility options: internal layout and dependencies can evolve.

##### Example 1: Classic Header/Source Split

Interface (`config_parser.h`):

```cpp
#pragma once

#include <string>
#include <string_view>

class ConfigParser
{
public:
    bool load(std::string_view file_path);
    std::string get(std::string_view key) const;
};
```

Implementation (`config_parser.cpp`):

```cpp
#include "config_parser.h"

#include <unordered_map>
#include <string>

namespace
{
    std::unordered_map<std::string, std::string> g_values;

    void parse_line(const std::string& line)
    {
        const auto pos = line.find('=');
        if (pos == std::string::npos)
        {
            return;
        }

        auto key = line.substr(0, pos);
        auto value = line.substr(pos + 1);
        g_values[key] = value;
    }
}

bool ConfigParser::load(std::string_view file_path)
{
    // Example-only: pretend we read file lines here.
    g_values.clear();
    parse_line("mode=release");
    parse_line("name=demo");
    return !file_path.empty();
}

std::string ConfigParser::get(std::string_view key) const
{
    auto it = g_values.find(std::string(key));
    return (it == g_values.end()) ? std::string{} : it->second;
}
```

Even in this simple form, callers only see parser behavior. They do not depend on parsing helpers or storage details.

##### Example 2: Intentional Hiding with pImpl

For stronger implementation hiding, use the pImpl (pointer-to-implementation) pattern.

How hiding happens in this pattern:

1. The header only forward-declares `Impl` and stores `std::unique_ptr<Impl>`.
2. The real `Impl` class definition exists only in the `.cpp` file.
3. Callers can compile and use `Database`, but they cannot see or depend on `Impl` members.
4. You can change `Impl` fields, helper functions, and private includes without changing the public header.

Public interface (`database.h`):

```cpp
#pragma once

#include <memory>
#include <string_view>

class Database
{
public:
    // Public API: construction, destruction, and moves.
    Database();
    ~Database();

    Database(Database&&) noexcept;
    Database& operator=(Database&&) noexcept;

    // Disable copying to keep unique ownership of implementation.
    Database(const Database&) = delete;
    Database& operator=(const Database&) = delete;

    // Public behavior promised to callers.
    void connect(std::string_view uri);
    void execute(std::string_view sql);

private:
    // Forward declaration of hidden implementation type.
    class Impl;
    // Opaque pointer that owns all private state/logic.
    std::unique_ptr<Impl> impl_;
};
```

Implementation (`database.cpp`):

```cpp
#include "database.h"

#include <iostream>
#include <string>

class Database::Impl
{
public:
    // Internal operation: store connection target and initialize backend state.
    void connect(std::string_view uri)
    {
        uri_ = std::string(uri);
        std::cout << "Connected to " << uri_ << '\n';
    }

    // Internal operation: execute SQL using hidden connection details.
    void execute(std::string_view sql)
    {
        std::cout << "SQL on " << uri_ << ": " << sql << '\n';
    }

private:
    // Hidden per-object state; not visible in public header.
    std::string uri_;
};

// Construct the hidden implementation object.
Database::Database() : impl_(std::make_unique<Impl>()) {}
// Destructor is defined in .cpp where Impl is complete.
Database::~Database() = default;
Database::Database(Database&&) noexcept = default;
Database& Database::operator=(Database&&) noexcept = default;

// Public API forwards work to hidden implementation.
void Database::connect(std::string_view uri)
{
    impl_->connect(uri);
}

// Public API forwards work to hidden implementation.
void Database::execute(std::string_view sql)
{
    impl_->execute(sql);
}
```

What does `class Database::Impl { ... };` mean?

- This is the definition of a nested class that was declared inside `Database`.
- `Database::Impl` means "the type named `Impl` that belongs to `Database`".
- It is not inheritance. Inheritance would look like `class Child : public Parent`.
- The `Database::` part is a scope qualifier, not a base-class list.

Why no separate include for `Impl` in the header?

- In the header, `class Impl;` is only a forward declaration (an incomplete type).
- `std::unique_ptr<Impl>` is allowed with an incomplete type in the class definition.
- The full `Impl` definition is needed only in the `.cpp`, where `Database` methods are compiled and where destruction is emitted.
- This is why the header includes only what it needs (`<memory>`, `<string_view>`), while heavier implementation headers stay in the source file.

Compile-time vs link-time view:

- Compile time (user code including `database.h`): compiler knows `Database` API and that `impl_` points to some hidden type.
- Compile time (`database.cpp`): compiler sees the real `Database::Impl` layout and compiles the actual behavior.
- Link time: object files are combined so calls to `Database::connect` and `Database::execute` resolve to definitions from `database.cpp`.
- Result: callers use a stable API while implementation details remain hidden in one translation unit.

What clients see (`database.h`):

- class `Database`
- public functions `connect` and `execute`
- a pointer-sized private field (`impl_`)

What clients do not see:

- `std::string uri_`
- logging or retry helpers
- low-level backend headers (network/database driver headers)

Concrete maintenance benefit:

- Today `Impl` stores `uri_` and prints to stdout.
- Tomorrow `Impl` can switch to a connection pool, add caching, and include heavy backend headers.
- As long as `Database` public functions stay the same, `database.h` does not change, so user code usually does not need edits.

##### Interface Design Intent

When you design an interface, think in terms of promises:

- Accept minimal, ownership-clear input types.
- Return explicit, intention-revealing result types.
- Hide policy and storage choices unless callers must know them.
- Avoid leaking private dependencies in public headers.

A good interface is small and predictable. A good implementation is free to evolve behind it.

#### Template Basics

Template fundamentals are still required knowledge:

- type parameterization
- specialization
- overload resolution interaction
- instantiation behavior

What changed in modern C++ is not that old template knowledge became wrong, but that constraints and helper utilities became better. C++20 concepts are an evolution of this foundation, not a replacement.

```cpp
template <typename T>
T max_of(T a, T b)
{
    return (a < b) ? b : a;
}
```

#### STL Containers and Algorithms

Containers and algorithms remain at the center of idiomatic C++.

The modern style is to prefer standard containers and algorithms first, and only drop to lower-level manual handling when profiling proves it is necessary.

```cpp
std::vector<int> values{4, 1, 9, 2};
std::sort(values.begin(), values.end());
```

This principle is even stronger in modern C++, because newer library features (`string_view`, `optional`, `span`, ranges) integrate naturally with the STL ecosystem.

#### Exception Handling for Exceptional Paths

Exceptions are still best used for exceptional failure, not normal control flow.

Good modern practice keeps error strategy explicit:

- use exceptions for truly exceptional failures
- use `std::optional` or `std::expected` for expected failure paths when appropriate

```cpp
Config load_config_or_throw(std::string_view path)
{
    if (path.empty())
    {
        throw std::runtime_error("empty path");
    }
    return Config{};
}
```

You do not have to pick one mechanism globally for every function. Choose based on semantics, performance requirements, and project conventions.

A pre-modern function like this is still perfectly valid:

```cpp
void print_value(const std::string& text)
{
    std::cout << text << std::endl;
}
```

Modern C++ does not reject this style. The difference is that newer code gives you options that are safer, clearer, and often faster.

### Rule of Three to Rule of Five to Rule of Zero

These rules are about how a class should behave when it owns a resource such as heap memory, a file handle, a socket, or another object that must be released explicitly.

#### Rule of Three

If a class manages a resource manually, and you define one of these three special member functions, you usually need the other two as well:

- destructor
- copy constructor
- copy assignment operator

Why? Because a class that owns something usually needs to define how that resource is destroyed, copied, and reassigned.

In review practice, if a resource-owning class manually defines one lifecycle function, verify that copy/move/destruction semantics are all coherent.

```cpp
class Buffer
{
public:
    Buffer(std::size_t size) : data(new int[size]), size(size) {}
    ~Buffer() { delete[] data; }

    Buffer(const Buffer& other) : data(new int[other.size]), size(other.size)
    {
        std::copy(other.data, other.data + size, data);
    }

    Buffer& operator=(const Buffer& other)
    {
        if (this != &other)
        {
            int* new_data = new int[other.size];
            std::copy(other.data, other.data + other.size, new_data);
            delete[] data;
            data = new_data;
            size = other.size;
        }
        return *this;
    }

private:
    int* data;
    std::size_t size;
};
```

This style was common in older C++ because the class itself had to manage the resource manually.

#### Rule of Five

Once C++11 added move semantics, the old three special members were no longer enough. If your class manages a resource directly, and you define one of the five special member functions, you often need to think about all five:

- destructor
- copy constructor
- copy assignment operator
- move constructor
- move assignment operator

Move operations matter because they allow the resource to be transferred instead of copied.

Using the same `Buffer` example, the two missing parts are the move constructor and move assignment operator:

```cpp
Buffer(Buffer&& other) noexcept : data(other.data), size(other.size)
{
    other.data = nullptr;
    other.size = 0;
}

Buffer& operator=(Buffer&& other) noexcept
{
    if (this != &other)
    {
        delete[] data;
        data = other.data;
        size = other.size;
        other.data = nullptr;
        other.size = 0;
    }
    return *this;
}
```

These two functions let ownership move from one `Buffer` object to another without allocating and copying a new array.

#### Rule of Zero

This is the modern preferred direction. Instead of writing special member functions yourself, structure the class so the members already know how to manage themselves.

That usually means using RAII types such as `std::string`, `std::vector`, `std::unique_ptr`, and similar library types.

```cpp
class Buffer
{
public:
    explicit Buffer(std::size_t size) : data(size) {}

private:
    std::vector<int> data;
};
```

In this version, you do not write destructor, copy, or move operations at all. The standard library does the right thing for you.

The Rule of Zero is often the best modern design target because it reduces bugs and makes classes easier to maintain.

---

## 2) Modern C++ Reading Mindset

The main shift is not syntax. The shift is design philosophy.

Older C++ code often relied on manual conventions. Modern C++ prefers "expressed correctness," where ownership, optionality, and constraints are visible in types and interfaces.

For an experienced OOP engineer, the key mindset differences when reading C++ are:

- C++ frequently uses value objects instead of always heap objects
- copy vs move is explicit in type operations and function signatures
- destructor timing is deterministic (scope-based), not garbage-collector-driven

Compare the intent in these two interfaces:

```cpp
// Older style
int find_user_id(const char* name); // -1 means not found?

// Modern style
std::optional<int> find_user_id(std::string_view name);
```

The second interface says much more about behavior:

- input is read-only text-like data without forcing allocation
- output can be "no value" in a type-safe way

This is the style transition you need to internalize.

---

## 3) C++11: The New Baseline of Modern C++

C++11 is the practical baseline for understanding modern C++ code in active projects.

### `auto` and Type Deduction

Before C++11, verbose iterator and template types made code noisy. `auto` lets the compiler infer obvious types so the code can focus on meaning.

```cpp
auto it = users.begin();
auto count = users.size();
```

Good modern usage is not "use `auto` everywhere." It is "use `auto` where explicit type adds little value."

### `nullptr` and Type Safety

Using `0` or `NULL` for pointers was always a compromise. `nullptr` removes ambiguity and improves overload resolution.

```cpp
int* p = nullptr;
```

### Range-Based `for`

The language now directly supports element-focused loops.

```cpp
for (const auto& user : users)
{
    std::cout << user.name << '\n';
}
```

This is not just shorter. It reduces iterator boilerplate and focuses on intent.

### Lambdas

In older C++, tiny one-use callable logic often required named functors. Lambdas make local behavior easy to write and keep nearby.

```cpp
std::sort(values.begin(), values.end(),
          [](int a, int b) { return a > b; });
```

### Move Semantics

This is one of the most important conceptual changes in all modern C++. With moves, expensive objects can transfer resources instead of deep-copying them.

```cpp
std::vector<int> make_values()
{
    std::vector<int> out{1, 2, 3};
    return out; // move or copy elision
}
```

Understanding move semantics helps with performance, API design, and avoiding accidental copies.

### Smart Pointers and Ownership

Use ownership types to express intent:

- `std::unique_ptr<T>` means unique ownership
- `std::shared_ptr<T>` means shared ownership
- `std::weak_ptr<T>` means non-owning reference to shared state

The key modern practice is to use raw pointers for non-owning observation and smart pointers for ownership.

---

## 4) C++14: C++11, But Smoother

C++14 improved ergonomics more than architecture.

Generic lambdas made it easier to write local generic behavior:

```cpp
auto add = [](auto x, auto y)
{
    return x + y;
};
```

`std::make_unique` removed a practical gap from C++11 and made unique ownership creation simpler and exception-safe.

```cpp
auto session = std::make_unique<Session>();
```

Relaxed `constexpr` rules made compile-time functions more practical. In short, C++14 did not redefine style, but it made C++11 code easier to write cleanly.

---

## 5) C++17: Everyday Productivity Standard

For many teams, C++17 became the first "comfortable modern" target.

### Structured Bindings

Instead of awkward tuple accessors, you can bind names directly:

```cpp
auto [host, port] = read_endpoint();
```

### `if constexpr`

Template code became much easier to read:

```cpp
template <typename T>
void debug_value(const T& v)
{
    if constexpr (std::is_integral_v<T>)
    {
        std::cout << "integral " << v << '\n';
    }
    else
    {
        std::cout << "non-integral\n";
    }
}
```

### `std::optional`, `std::variant`, `std::any`

These types remove many older ad-hoc patterns:

- optional values no longer need sentinel values
- tagged unions become explicit via `std::variant`
- truly dynamic values can use `std::any` when necessary

### `std::string_view`

One of the most practical improvements for API design:

```cpp
void log_line(std::string_view text);
```

You can accept many string-like inputs without forcing allocation or ownership transfer.

### `std::filesystem`

Portable path and file operations moved into the standard library, reducing platform-specific code and dependency overhead.

---

## 6) C++20: A Second Major Inflection Point

C++20 is a large standard. For returning developers, three areas matter the most: constraints, ranges, and stronger library utilities.

### Concepts and `requires`

In classic template code, errors were often long and indirect. Concepts let you state requirements directly in the interface.

```cpp
template <typename T>
requires std::integral<T>
T increment(T value)
{
    return value + 1;
}
```

This improves readability and diagnostics.

### Ranges

Ranges make algorithm pipelines expressive and composable:

```cpp
auto names = users
    | std::views::filter([](const User& u) { return u.active; })
    | std::views::transform([](const User& u) { return u.name; });
```

You can still use iterators and classic algorithms, but ranges often produce clearer data-processing code.

### `std::span`

`std::span` is a non-owning view over contiguous memory. It is ideal for APIs that should not take ownership:

```cpp
void normalize(std::span<float> data);
```

Compared with pointer-plus-length pairs, `span` is safer and clearer.

### `std::format`

Formatting becomes cleaner and type-safe:

```cpp
auto msg = std::format("id={}, score={:.2f}", id, score);
```

### Coroutines and Modules

Both are important but can be learned later unless your project depends on them immediately.

- Coroutines help with asynchronous and generator-style workflows.
- Modules aim to improve build and isolation models compared to textual includes.

They are powerful, but build-system and toolchain maturity can vary.

---

## 7) C++23: Maturity and Gap-Filling

C++23 is less about big paradigm change and more about making modern C++ workflows complete.

### `std::expected`

`std::expected<T, E>` is a strong tool for explicit error-return design when exceptions are not your preferred mechanism:

```cpp
std::expected<Config, ParseError> load_config(std::string_view path);
```

This can make control flow and failure handling explicit and test-friendly.

### `std::print` and `std::println`

Output formatting can be simpler and more direct than stream-heavy code.

```cpp
std::println("Loaded {} records", count);
```

### `std::mdspan`

For matrix-like or scientific data, `mdspan` provides non-owning multidimensional access without forcing a single storage policy.

### Language Cleanups

Features like `if consteval` and explicit object parameters refine modern metaprogramming and method expression patterns.

---

## 8) Language Features vs Library Features: Why Your Toolchain Matters

A frequent source of confusion in real projects is this: "My compiler says C++20, why does feature X still fail?"

The reason is that support has two layers:

- language support from compiler frontend
- standard library support from library implementation

For example, concepts are mainly language/frontend territory. `std::format` is mainly library territory. You can have one without full maturity of the other depending on toolchain version.

For the major families:

- GCC commonly ships with `libstdc++`
- Clang often uses `libc++`, but can also target `libstdc++`
- MSVC uses Microsoft STL

So when diagnosing support, check compiler version and standard library version together.

---

## 9) How to Read and Modernize Legacy Code

Most teams work in mixed codebases that contain old and new patterns. A practical strategy is to modernize at boundaries instead of rewriting everything.

### Boundary Modernization Pattern

1. Keep stable old internals initially.
2. Improve function signatures first.
3. Replace ownership models next.
4. Introduce algorithm/ranges cleanup later.

For example, an old interface:

```cpp
int find_user(const char* name); // returns -1 if missing
```

Can often be upgraded first to:

```cpp
std::optional<int> find_user(std::string_view name);
```

Without rewriting every internal detail on day one.

### Replace the Most Risky Patterns First

High-risk legacy patterns usually include:

- owning raw pointers
- manual `new`/`delete` chains
- sentinel-value protocols
- unbounded C-string usage
- hidden ownership transfer conventions

Modernizing those gives the biggest safety and maintenance win.

---

## 10) Recommended Fast-Track Plan (Practical)

Do not try to absorb all standards at once. A staged approach works better.

Start by refreshing old fundamentals and quickly adding C++11 core features (`auto`, `nullptr`, lambdas, move semantics, smart pointers). Then adopt C++17 practical utilities (`optional`, `string_view`, structured bindings, `if constexpr`). Next, add C++20 concepts, `span`, ranges, and `format`. Finally, pick selected C++23 features based on project needs (`expected`, `print`, `mdspan`).

This path gets you productive quickly while still moving toward modern style.

---

## 11) Final Takeaway

If you are an experienced multi-language OOP engineer, you are not starting from zero. Most class and interface patterns are familiar.

The part to internalize is explicit lifetime and ownership: constructor/destructor behavior, copy vs move semantics, pointer/reference intent, and when smart pointers express ownership contracts.

The winning mindset for code review is: identify ownership first, identify lifecycle second, then read business logic. Once those two layers are clear, most C++ source code becomes straightforward to reason about.

---

## 12) A Full Old-to-Modern Refactor Example

To make the transition concrete, this section shows a realistic style shift. The goal is not to claim the old code is "wrong." The goal is to show how modern C++ communicates intent and ownership more clearly.

### Older Style (Common in C++03-era Code)

```cpp
int parse_scores(const char* line, int* out_values, int max_values)
{
    if (!line || !out_values || max_values <= 0)
    {
        return -1;
    }

    int count = 0;
    const char* p = line;

    while (*p && count < max_values)
    {
        char* end = 0;
        long v = std::strtol(p, &end, 10);
        if (end == p)
        {
            break;
        }

        out_values[count++] = static_cast<int>(v);
        p = (*end == ',') ? end + 1 : end;
    }

    return count;
}
```

This style works, but it mixes many concerns:

- ownership and buffer management are external and implicit
- error signaling uses sentinel integers
- bounds safety depends on caller discipline
- interface does not describe intent clearly

### Modernized Style

```cpp
std::expected<std::vector<int>, std::string>
parse_scores(std::string_view line)
{
    std::vector<int> out;
    std::size_t pos = 0;

    while (pos < line.size())
    {
        std::size_t next = line.find(',', pos);
        std::string_view token = (next == std::string_view::npos)
            ? line.substr(pos)
            : line.substr(pos, next - pos);

        int value = 0;
        auto [ptr, ec] = std::from_chars(token.data(), token.data() + token.size(), value);
        if (ec != std::errc{} || ptr != token.data() + token.size())
        {
            return std::unexpected(std::string("invalid token: ") + std::string(token));
        }

        out.push_back(value);
        if (next == std::string_view::npos)
        {
            break;
        }
        pos = next + 1;
    }

    return out;
}
```

The new version changes meaning, not just syntax:

- input is non-owning text view (`std::string_view`)
- output owns its own data (`std::vector<int>`)
- errors are explicit and typed (`std::expected`)
- no external output buffer contract is required

This is the core modern pattern: make ownership and error behavior visible in types.

---

## 13) Common Reading Mistakes in C++

Even experienced developers can hit repeated style mismatches when reading C++ for the first time in a large codebase. Knowing them early saves time.

### Mistake 1: Keeping Ownership Ambiguous

A classic pattern is passing raw pointers everywhere. In modern code, use raw pointers mostly for non-owning access. Use `std::unique_ptr` or `std::shared_ptr` when ownership is involved.

### Mistake 2: Using Sentinel Values for Everything

If a function can fail, and failure is not exceptional, modern interfaces usually prefer `std::optional` or `std::expected` instead of magic values.

### Mistake 3: Overusing Manual Loops

Manual loops are still valid, but many are clearer as algorithms or ranges. The goal is not to ban loops; it is to express intent more directly.

### Mistake 4: Treating `auto` as Either Always Good or Always Bad

Both extremes hurt readability. Use `auto` where type repetition adds noise, and use explicit types where they improve understanding.

### Mistake 5: Ignoring Toolchain Layering

A feature may compile in one environment and fail in another because frontend support and standard library support can differ. Always verify compiler mode and library version together.

---

## 14) Practical Review Exercises (One Weekend Plan)

A useful way to build confidence is to take one utility module and modernize it in steps.

### Exercise 1: Ownership Pass

Replace owning raw pointers with `std::unique_ptr` where shared ownership is not required. Remove manual `delete` code from normal control paths.

### Exercise 2: Interface Pass

Where applicable, replace `const std::string&` inputs used only for read access with `std::string_view`. Replace pointer-plus-length arguments with `std::span` for contiguous data.

### Exercise 3: Return Type Pass

Replace sentinel-value returns with `std::optional` or `std::expected` based on whether you need error detail.

### Exercise 4: Algorithm Pass

Refactor one or two loop-heavy functions with algorithms or ranges. Keep behavior identical; only improve clarity.

### Exercise 5: Template Pass

If you have template helpers, add concepts to the public surface where constraints are clear and stable.

By the end of this sequence, the codebase usually feels "modern" without a risky full rewrite.

---

## 15) Closing Advice for Long-Term Success

The fastest way to gain confidence is to treat modern C++ as evolutionary, not revolutionary. Keep your strengths in API design, performance awareness, and careful reasoning about state. Then add modern expression tools on top of that base.

When in doubt, ask three design questions:

1. Who owns this resource?
2. Can this value be absent or erroneous, and how is that expressed?
3. Is the interface saying intent clearly through types?

If your code answers those questions well, you are already writing strong modern C++, even before adopting every new feature in the latest standard.


