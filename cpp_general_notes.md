# C++ General Notes <!-- omit in toc -->

## Table of Contents <!-- omit in toc -->

- [1) Standard Data Types and Literal Assignment](#1-standard-data-types-and-literal-assignment)
  - [Fundamental Integer Types](#fundamental-integer-types)
  - [Character Types](#character-types)
  - [Floating-Point Types](#floating-point-types)
  - [Boolean and Void](#boolean-and-void)
  - [Common Literal Forms](#common-literal-forms)
  - [Type Aliases from `<cstdint>` (Fixed Width)](#type-aliases-from-cstdint-fixed-width)
- [2) Keywords Added by C++ Standard (C++11 to Latest)](#2-keywords-added-by-c-standard-c11-to-latest)
  - [Common Keywords Before C++11 (C++98/C++03 era)](#common-keywords-before-c11-c98c03-era)
  - [C++11 (new keywords)](#c11-new-keywords)
  - [C++14](#c14)
  - [C++17](#c17)
  - [C++20 (new keywords)](#c20-new-keywords)
  - [C++23](#c23)
  - [C++26 (latest draft/ongoing standardization)](#c26-latest-draftongoing-standardization)
  - [Quick Context: Reserved vs Contextual](#quick-context-reserved-vs-contextual)
- [3) Features Added by New Standards (Not in Older Standards)](#3-features-added-by-new-standards-not-in-older-standards)
  - [C++11 (major jump from C++03)](#c11-major-jump-from-c03)
  - [C++14](#c14-1)
  - [C++17](#c17-1)
  - [C++20](#c20)
  - [C++23](#c23-1)
  - [C++26 (in progress)](#c26-in-progress)
  - [Practical Tip](#practical-tip)
- [4) Compiler Compatibility Comparison: GNU GCC, LLVM Clang, and Microsoft MSVC](#4-compiler-compatibility-comparison-gnu-gcc-llvm-clang-and-microsoft-msvc)
  - [Main Compilers](#main-compilers)
  - [Latest Current Versions (as of May 2026)](#latest-current-versions-as-of-may-2026)
  - [Standards Support](#standards-support)
  - [What Is Not Fully Supported Yet](#what-is-not-fully-supported-yet)
  - [C++ Standard vs Standard Library](#c-standard-vs-standard-library)
  - [Standard Library Names in the Three Main Implementations](#standard-library-names-in-the-three-main-implementations)
  - [Why This Matters](#why-this-matters)
  - [Main Standard Library Areas](#main-standard-library-areas)
  - [Are Keywords Part of the Standard Library?](#are-keywords-part-of-the-standard-library)
  - [Quick Comparison](#quick-comparison)
  - [Important C++20 Feature Support](#important-c20-feature-support)
  - [Important C++23 Feature Support](#important-c23-feature-support)
  - [Reading These Tables Correctly](#reading-these-tables-correctly)
  - [Language Mode Flags](#language-mode-flags)
  - [Important Practical Differences](#important-practical-differences)
    - [GCC](#gcc)
    - [Clang/LLVM](#clangllvm)
    - [MSVC](#msvc)
  - [Compatibility Notes](#compatibility-notes)
  - [Common Sources of Portability Problems](#common-sources-of-portability-problems)
  - [Good Practice for Cross-Compiler Code](#good-practice-for-cross-compiler-code)



## 1) Standard Data Types and Literal Assignment

### Fundamental Integer Types

| Type                 | Typical Size* | Signed/Unsigned | Example Literal Assignment                 |
| -------------------- | ------------: | --------------- | ------------------------------------------ |
| `short`              |       2 bytes | signed          | `short s = -120;`                          |
| `unsigned short`     |       2 bytes | unsigned        | `unsigned short us = 65000;`               |
| `int`                |       4 bytes | signed          | `int n = 42;`                              |
| `unsigned int`       |       4 bytes | unsigned        | `unsigned int un = 42u;`                   |
| `long`               |  4 or 8 bytes | signed          | `long l = 100000L;`                        |
| `unsigned long`      |  4 or 8 bytes | unsigned        | `unsigned long ul = 100000UL;`             |
| `long long`          |       8 bytes | signed          | `long long big = 9000000000LL;`            |
| `unsigned long long` |       8 bytes | unsigned        | `unsigned long long ubig = 9000000000ULL;` |

\* Sizes are implementation-dependent, but ordering/ranges are guaranteed by the standard.

### Character Types

| Type               | Purpose                                | Example                          |
| ------------------ | -------------------------------------- | -------------------------------- |
| `char`             | basic narrow character                 | `char c = 'A';`                  |
| `signed char`      | explicitly signed byte-sized integer   | `signed char sc = -10;`          |
| `unsigned char`    | explicitly unsigned byte-sized integer | `unsigned char uc = 250;`        |
| `wchar_t`          | wide character                         | `wchar_t wc = L'Z';`             |
| `char8_t` (C++20)  | UTF-8 code unit                        | `char8_t u8c = u8'A';`           |
| `char16_t` (C++11) | UTF-16 code unit                       | `char16_t u16c = u'\u03A9';`     |
| `char32_t` (C++11) | UTF-32 code unit                       | `char32_t u32c = U'\U0001F600';` |

### Floating-Point Types

| Type          | Typical Size* | Example Literal Assignment |
| ------------- | ------------: | -------------------------- |
| `float`       |       4 bytes | `float f = 3.14f;`         |
| `double`      |       8 bytes | `double d = 3.14;`         |
| `long double` | 8/12/16 bytes | `long double ld = 3.14L;`  |

\* Implementation-dependent.

### Boolean and Void

| Type   | Meaning                   | Example           |
| ------ | ------------------------- | ----------------- |
| `bool` | logical true/false        | `bool ok = true;` |
| `void` | no value / no type object | `void print();`   |

### Common Literal Forms

```cpp
// Integer literals
int dec = 42;          // decimal
int hex = 0x2A;        // hexadecimal
int oct = 052;         // octal
int bin = 0b101010;    // binary (C++14+)

// Integer suffixes
unsigned int u = 42u;
long l = 42L;
long long ll = 42LL;

// Floating literals
float f = 1.5f;
double d = 1.5;
long double ld = 1.5L;

// Character and string literals
char c = 'x';
wchar_t wc = L'x';
char16_t c16 = u'x';
char32_t c32 = U'x';
const char* s = "hello";
const char* raw = R"(line1\nline2)"; // raw string literal

// Boolean and null pointer literal
bool ready = false;
int* p = nullptr;      // C++11+
```

### Type Aliases from `<cstdint>` (Fixed Width)

```cpp
#include <cstdint>

std::int8_t a = -5;
std::uint32_t b = 100u;
std::int64_t c = 9000000000LL;
```

Use these when exact bit width matters (file formats, networking, embedded systems).

---

## 2) Keywords Added by C++ Standard (C++11 to Latest)

Note: This list focuses on **new reserved keywords** introduced in each standard revision.

### Common Keywords Before C++11 (C++98/C++03 era)

These are core keywords available in older C++ versions and still used today:

- `asm`, `auto`, `bool`, `break`, `case`, `catch`, `char`, `class`, `const`, `const_cast`, `continue`
- `default`, `delete`, `do`, `double`, `dynamic_cast`
- `else`, `enum`, `explicit`, `export`, `extern`
- `false`, `float`, `for`, `friend`
- `goto`
- `if`, `inline`, `int`
- `long`
- `mutable`
- `namespace`, `new`
- `operator`
- `private`, `protected`, `public`
- `register`, `reinterpret_cast`, `return`
- `short`, `signed`, `sizeof`, `static`, `static_cast`, `struct`, `switch`
- `template`, `this`, `throw`, `true`, `try`, `typedef`, `typeid`, `typename`
- `union`, `unsigned`, `using`
- `virtual`, `void`, `volatile`
- `wchar_t`, `while`

### C++11 (new keywords)

- `alignas`
- `alignof`
- `char16_t`
- `char32_t`
- `constexpr`
- `decltype`
- `noexcept`
- `nullptr`
- `static_assert`
- `thread_local`

### C++14

- No new reserved keywords.

### C++17

- No new reserved keywords.

### C++20 (new keywords)

- `char8_t`
- `concept`
- `consteval`
- `constinit`
- `co_await`
- `co_return`
- `co_yield`
- `requires`

### C++23

- No new reserved keywords.

### C++26 (latest draft/ongoing standardization)

- As of now, no finalized additional reserved keywords beyond C++23.

### Quick Context: Reserved vs Contextual

- Reserved keywords cannot be used as identifiers (variable/function/class names).
- Some tokens in C++ are contextual (special meaning only in certain grammar positions), but not always reserved globally.

---

## 3) Features Added by New Standards (Not in Older Standards)

### C++11 (major jump from C++03)

- `auto` type deduction for variables
- Range-based `for` loop
- Lambda expressions
- Alias declarations (`using Name = Type;` C++11 style, better for templates)
- Move semantics and rvalue references (`T&&`)
- Smart pointers: `std::unique_ptr`, `std::shared_ptr`, `std::weak_ptr`
- `nullptr` and strongly typed enums (`enum class`)
- `constexpr` (basic form), `decltype`, and trailing return types
- Variadic templates
- Concurrency library: `std::thread`, `std::mutex`, `std::future`, `std::async`
- Uniform initialization (`{}`), initializer lists

In this context, an lvalue is an expression that identifies a specific object in memory, usually one you can name, reference, or reach through a pointer. An rvalue is a temporary value expression used during a computation or statement, and it usually disappears at the end of the full expression unless lifetime extension applies when const apply to consumming object.

### C++14

- Generic lambdas (`auto` parameters)
- Return type deduction for normal functions (`auto f() {}`)
- Relaxed `constexpr` rules
- Variable templates
- Binary literals and digit separators (`0b1010`, `1'000'000`)
- `std::make_unique`

### C++17

- Structured bindings (`auto [a, b] = ...`)
- `if constexpr`
- Fold expressions for variadic templates
- Inline variables
- `std::optional`, `std::variant`, `std::any`
- `std::string_view`
- Filesystem library (`std::filesystem`)
- Parallel algorithms (execution policies)

### C++20

- Concepts and `requires`
- Coroutines (`co_await`, `co_yield`, `co_return`)
- Ranges library (`std::ranges`)
- Modules
- Three-way comparison operator (`<=>`)
- `consteval`, `constinit`, and expanded `constexpr`
- Calendar/time-zone updates in chrono
- `std::span`, `std::format`, `std::jthread`, `std::stop_token`

### C++23

- `std::expected`
- `if consteval`
- Explicit object parameter syntax (`this` as parameter)
- Multidimensional subscript operator support
- `std::print` / `std::println`
- More constexpr support across standard library containers/utilities
- Ranges additions (`views::zip` and related utilities)

### C++26 (in progress)

- Standardization work is ongoing.
- Some features are still draft-level and compiler support may vary.

### Practical Tip

- In real projects, available features depend on both compiler version and selected language mode (for example, `-std=c++17`, `-std=c++20`, `-std=c++23`).

---

## 4) Compiler Compatibility Comparison: GNU GCC, LLVM Clang, and Microsoft MSVC

### Main Compilers

| Compiler | Full Name                    | Common Command | Typical Platforms             |
| -------- | ---------------------------- | -------------- | ----------------------------- |
| GCC      | GNU Compiler Collection      | `g++`          | Linux, Unix, MinGW on Windows |
| Clang    | LLVM Clang compiler frontend | `clang++`      | Linux, macOS, Windows         |
| MSVC     | Microsoft Visual C++         | `cl`           | Windows                       |

### Latest Current Versions (as of May 2026)

| Compiler   | Latest Version | Notes                                        |
| ---------- | -------------- | -------------------------------------------- |
| GCC        | `16.1`         | Released April 30, 2026                      |
| Clang/LLVM | `22.1.0`       | Released February 24, 2026                   |
| MSVC       | `14.50`        | Shipped with Visual Studio 2026 version 18.0 |

### Standards Support

The table below is a practical summary anchored to recent compiler generations around GCC 16.1, Clang 22.1.0, and MSVC 14.50.

| Standard | GCC                                                                                            | Clang/LLVM                                                                         | MSVC                                                                                                                                  |
| -------- | ---------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| C++11    | Fully supported                                                                                | Fully supported                                                                    | Fully supported                                                                                                                       |
| C++14    | Fully supported                                                                                | Fully supported                                                                    | Fully supported                                                                                                                       |
| C++17    | Fully supported                                                                                | Fully supported                                                                    | Fully supported                                                                                                                       |
| C++20    | Mostly supported; some newer library parts may still depend on `libstdc++` version             | Mostly supported; some support depends on `libc++` or platform toolchain packaging | Mostly supported; some library features historically required `/std:c++latest` before full rollout                                    |
| C++23    | Partial support; many features available, but not all language and library papers are complete | Partial support; many features available, but full C++23 coverage is not complete  | Partial support; Microsoft marks C++23 as supported overall, but some features are still `no`, `partial`, or only in `/std:c++latest` |
| C++26    | Not fully supported; draft and experimental only                                               | Not fully supported; draft and experimental only                                   | Not fully supported; draft and experimental only                                                                                      |

### What Is Not Fully Supported Yet

| Compiler     | Main Gaps to Remember                                                                                                                                              |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| GCC 16.1     | C++26 is not finalized; some C++23 and newer standard library features still depend on the shipped `libstdc++` version                                             |
| Clang 22.1.0 | Frontend support may exist before matching `libc++` library support is complete; C++26 remains experimental                                                        |
| MSVC 14.50   | Some C++23 items are still marked `no` or `partial` in Microsoft's conformance tables; several newest features require `/std:c++latest`; C++26 is still draft-only |

### C++ Standard vs Standard Library

The C++ language standard and the C++ standard library are related, but they are different layers of the language ecosystem.

The **C++ language standard** defines how C++ itself works. It covers the grammar and rules for code such as keywords, expressions, classes, templates, overload resolution, modules, coroutines, and other core language behavior.

The **C++ standard library** defines the reusable facilities that come with C++. It provides containers, algorithms, strings, streams, ranges, formatting, filesystem support, concurrency utilities, and helper types.

This separation matters because a feature can have two sides:

- a **language side**, which depends on the compiler frontend
- a **library side**, which depends on the standard library implementation shipped with the toolchain

For example, `if consteval`, `using enum`, and multidimensional subscript are language features. By contrast, `std::span`, `std::format`, `std::expected`, `std::print`, `std::mdspan`, and ranges are library features. A compiler may implement the language rule first while the matching library support is still incomplete.

This is why compiler version alone is not enough when checking whether a feature is usable in practice. The compiler may be new enough, but the bundled standard library may still lag behind.

### Standard Library Names in the Three Main Implementations

| Compiler family | Standard library implementation | Common name |
| --------------- | ------------------------------- | ----------- |
| GCC             | GNU C++ standard library        | `libstdc++` |
| Clang/LLVM      | LLVM C++ standard library       | `libc++`    |
| MSVC            | Microsoft C++ Standard Library  | MSVC STL    |

### Why This Matters

- GCC usually pairs with `libstdc++`, so GCC language support and `libstdc++` support are often discussed together, but they are still separate versioned components.
- Clang is only the compiler frontend; on many systems it can use either `libc++` or `libstdc++` depending on how the toolchain is configured.
- MSVC ships its own standard library implementation, so its language support and library support evolve together inside Visual Studio releases.
- A feature may look supported in a compiler table but still fail in practice if the linked standard library is too old or the platform package is incomplete.

### Main Standard Library Areas

- Containers: `std::vector`, `std::map`, `std::array`, `std::span`
- Algorithms: `std::sort`, `std::find`, ranges algorithms
- Strings and views: `std::string`, `std::string_view`, formatting facilities
- Input/output: streams, buffers, printing, formatting
- Utilities: `std::optional`, `std::variant`, `std::expected`, type traits, iterators, memory helpers
- Concurrency: `std::thread`, `std::mutex`, `std::jthread`, synchronization primitives
- Filesystem and time: `std::filesystem`, chrono calendars, time zones

This distinction is important because some of the newest C++ additions are primarily library features, not language keywords. For example, `std::expected` and `std::print` are library additions, while `if consteval` and multidimensional subscript are language additions.

### Are Keywords Part of the Standard Library?

No. Keywords belong to the **C++ language standard**, not the standard library.

- The standard library does not add new keywords.
- New keywords such as `constexpr`, `nullptr`, `concept`, `requires`, `char8_t`, and `consteval` come from the language standard.
- Library facilities use normal identifiers inside headers, such as `std::vector`, `std::string`, `std::span`, and `std::expected`.
- Some library headers define macros or types that look special, but they are still library names, not keywords.

In short: keywords are controlled by the language standard, while names in the `std::` namespace come from the standard library.

### Quick Comparison

| Topic           | C++ Language Standard                            | C++ Standard Library                                           |
| --------------- | ------------------------------------------------ | -------------------------------------------------------------- |
| What it defines | Syntax and rules of the language                 | Reusable utilities and components                              |
| Examples        | `if`, `class`, `template`, `concept`, `requires` | `std::vector`, `std::string`, `std::span`, `std::format`       |
| Added by        | The language standard itself                     | The library implementation shipped with the compiler toolchain |
| Depends on      | Compiler frontend                                | Compiler plus standard library version                         |
| Keywords?       | Yes                                              | No                                                             |

> Callout: when you check whether a feature is supported, always ask two questions: is it a language feature, and is the matching standard library feature available in the toolchain?

### Important C++20 Feature Support

Status meaning:

- `Yes` = broadly available in the current compiler + standard library combination
- `Partial` = supported, but with limitations, toolchain caveats, or library gaps
- `Note` = supported but commonly needs extra care in real projects

| C++20 Feature                      | GCC 16.1 | Clang 22.1.0 | MSVC 14.50 | Notes                                                                                                     |
| ---------------------------------- | -------- | ------------ | ---------- | --------------------------------------------------------------------------------------------------------- |
| Concepts                           | Yes      | Yes          | Yes        | Core language feature                                                                                     |
| Coroutines                         | Yes      | Partial      | Yes        | Clang support is usable, but ecosystem/tooling and library integration can vary                           |
| Modules                            | Partial  | Partial      | Partial    | All three have support, but build-system integration and portability are still harder than normal headers |
| Three-way comparison (`<=>`)       | Yes      | Yes          | Yes        | Core language feature                                                                                     |
| `consteval` / `constinit`          | Yes      | Yes          | Yes        | Widely available in current releases                                                                      |
| Ranges (`std::ranges`)             | Yes      | Yes          | Yes        | Library feature; depends on `libstdc++`, `libc++`, or MSVC STL                                            |
| `std::span`                        | Yes      | Yes          | Yes        | Stable C++20 library feature                                                                              |
| `std::format`                      | Yes      | Yes          | Yes        | Library support matters; older toolchains were uneven, current ones are much better                       |
| `std::jthread` / `std::stop_token` | Yes      | Yes          | Yes        | C++20 concurrency library feature                                                                         |
| `std::source_location`             | Yes      | Yes          | Yes        | Library feature with broad modern support                                                                 |
| Calendar/time-zone chrono          | Yes      | Partial      | Yes        | Clang frontend may be fine, but `libc++` packaging/support can lag by platform                            |

### Important C++23 Feature Support

| C++23 Feature                                  | GCC 16.1 | Clang 22.1.0                | MSVC 14.50 | Notes                                                                                    |
| ---------------------------------------------- | -------- | --------------------------- | ---------- | ---------------------------------------------------------------------------------------- |
| `std::expected`                                | Yes      | Yes                         | Yes        | Important C++23 library feature                                                          |
| `std::print` / `std::println`                  | Yes      | Yes                         | Yes        | Implemented in current major toolchains, but older system libraries may lag              |
| `if consteval`                                 | Yes      | Yes                         | Yes        | Core language feature                                                                    |
| Explicit object parameter (`deducing this`)    | Yes      | Yes                         | Yes        | Earlier MSVC support was partial; current versions are much better                       |
| Multidimensional subscript operator            | Yes      | Yes                         | Yes        | Core language feature                                                                    |
| `views::zip` and related range views           | Yes      | Yes                         | Yes        | Historically partial in some libraries; current major versions provide practical support |
| `std::mdspan`                                  | Yes      | Yes                         | Yes        | Important library feature; older libc++ support was partial                              |
| Standard Library Modules (`std`, `std.compat`) | Partial  | Partial                     | Partial    | Available in some toolchains, but still one of the least portable areas                  |
| `#warning`                                     | Yes      | Yes                         | Yes        | Preprocessor convenience feature                                                         |
| `std::generator`                               | Yes      | No clear mainstream support | Yes        | Still less uniform across toolchains than simpler C++23 features                         |

### Reading These Tables Correctly

- Language features depend mostly on the compiler frontend.
- Library features depend on both the compiler and the standard library implementation.
- Clang can support a language feature while the shipped `libc++` on a platform still lacks the matching library feature.
- GCC usually ships with `libstdc++`, while MSVC uses Microsoft's STL, so practical support can differ even when the language mode is the same.

### Language Mode Flags

```bash
# GCC
g++ -std=c++17 main.cpp -o main

# Clang
clang++ -std=c++20 main.cpp -o main

# MSVC
cl /std:c++20 main.cpp
```

### Important Practical Differences

#### GCC

- Very common on Linux and in competitive programming
- Strong optimization support
- Usually paired with `libstdc++`
- Good support for GNU-specific extensions

#### Clang/LLVM

- Known for clear and helpful error messages
- Default compiler on many macOS setups
- Usually paired with `libc++` on some platforms, but may also use `libstdc++`
- Popular in tooling and static analysis ecosystems

#### MSVC

- Main compiler for native Windows development
- Integrated closely with Visual Studio
- Uses Microsoft's standard library implementation
- Some compiler flags and platform behavior differ from GCC/Clang

### Compatibility Notes

- Code that follows the ISO C++ standard usually works on all three compilers.
- Non-standard extensions may compile on one compiler but fail on another.
- Standard library implementation details can differ even when the language feature is supported.
- Warning messages and error formatting differ across compilers.
- Build commands and compiler flags are not identical across GCC, Clang, and MSVC.

### Common Sources of Portability Problems

- Compiler-specific extensions such as GNU-only attributes or pragmas
- Windows-specific APIs in MSVC-based projects
- Different behavior for warnings, default signedness, and header availability
- Differences in experimental or newly added standard library features
- ABI and binary compatibility differences between standard library implementations

### Good Practice for Cross-Compiler Code

- Prefer standard C++ features over compiler extensions
- Test code with at least two compilers when possible
- Enable warnings and treat important warnings seriously
- Choose the correct language mode explicitly (`-std=c++20`, `/std:c++20`)
- Check compiler support tables when using very new C++ features

