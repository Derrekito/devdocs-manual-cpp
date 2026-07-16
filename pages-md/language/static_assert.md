# `static_assert` declaration (since C++11)

Performs compile-time assertion checking.

### Syntax

```text
static_assert( bool-constexpr , message )    (since C++11)
static_assert( bool-constexpr )    (since C++17)
```

### Explanation

- **bool-constexpr** — a contextually converted constant expression of type
  bool. Built-in conversions are not allowed, except for non-narrowing integral
  conversions to bool. (until C++23) an expression contextually converted to
  bool where the conversion is a constant expression (since C++23)
- **a contextually converted constant expression of type bool. Built-in
  conversions are not allowed, except for non-narrowing integral conversions to
  bool.** — (until C++23)
- **an expression contextually converted to bool where the conversion is a
  constant expression** — (since C++23)
- **message** — an unevaluated string literal that will appear as compiler error
  if bool-constexpr is false or an expression `msg` such that `msg.size()` is
  implicitly convertible to `std::size_t` `msg.data()` is implicitly convertible
  to `const char*` (since C++26)
- **or an expression `msg` such that `msg.size()` is implicitly convertible to
  `std::size_t` `msg.data()` is implicitly convertible to `const char*`** —
  (since C++26)

A `static_assert` declaration may appear at namespace and block scope (as a
block declaration) and inside a class body (as a member declaration).

If bool-constexpr is well-formed and evaluates to `true`, or is evaluated in the
context of a template definition and the template is uninstantiated, this
declaration has no effect. Otherwise a compile-time error is issued, and the
text of message, if any, is included in the diagnostic message.

If message is not a string literal, it is evaluated only if bool-constexpr
evaluates to `false`. The text of message is formed by the character sequence
`[``msg.data()``,``msg.data() + msg.size()``)` which must denote a valid range,
and each element in the sequence must be an integral constant expression.
*(since C++26)*

### Notes

The standard does not require a compiler to print the verbatim text of message,
though compilers generally do so as much as possible.

Since message has to be a string literal, it cannot contain dynamic information
or even a constant expression that is not a string literal itself. In
particular, it cannot contain the name of the template type argument.
*(until C++26)*

  Feature-test macro | Value | Std | Feature
  `__cpp_static_assert` | 200410L | (C++11) | `static_assert(bool-constexpr ﻿,
      "comment")`
  201411L | (C++17) | Single-argument `static_assert(bool-constexpr ﻿)`
  202306L | (C++26) | user-generated `static_assert` messages

### Example

```cpp
#include <format>
#include <type_traits>

static_assert(03301 == 1729); // since C++17 the message string is optional

template<class T>
void swap(T& a, T& b) noexcept
{
    static_assert(std::is_copy_constructible_v<T>,
                  "Swap requires copying");
    static_assert(std::is_nothrow_copy_constructible_v<T> &&
                  std::is_nothrow_copy_assignable_v<T>,
                  "Swap requires nothrow copy/assign");
    auto c = b;
    b = a;
    a = c;
}

template<class T>
struct data_structure
{
    static_assert(std::is_default_constructible_v<T>,
                  "Data structure requires default-constructible elements");
};

template<class>
constexpr bool dependent_false = false; // workaround before CWG2518/P2593R1

template<class T>
struct bad_type
{
    static_assert(dependent_false<T>, "error on instantiation, workaround");
    static_assert(false, "error on instantiation"); // OK because of CWG2518/P2593R1
};

struct no_copy
{
    no_copy(const no_copy&) = delete;
    no_copy() = default;
};

struct no_default
{
    no_default() = delete;
};

#if __cpp_static_assert >= 202306L
// Not real C++ yet (std::format should be constexpr to work):
static_assert(sizeof(int) == 4, std::format("Expected 4, got {}", sizeof(int)));
#endif

int main()
{
    int a, b;
    swap(a, b);

    no_copy nc_a, nc_b;
    swap(nc_a, nc_b); // 1

    [[maybe_unused]] data_structure<int> ds_ok;
    [[maybe_unused]] data_structure<no_default> ds_error; // 2
}
```

Possible output:

```text
1: error: static assertion failed: Swap requires copying
2: error: static assertion failed: Data structure requires default-constructible elements
3: error: static assertion failed: Expected 4, got 2
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  CWG 2039 | C++11 | only the expression before conversion is required to be
      constant | the conversion must also be valid in a constant expression
  CWG 2518 | C++11 | uninstantiated `static_assert(false, "");` was error | made
      well-formed

### See also

- **#error** — shows the given error message and renders the program ill-formed
  (preprocessing directive)
- **assert** — aborts the program if the user-specified condition is not `true`.
  May be disabled for release builds (function macro)
- **enable_if (C++11)** — conditionally removes a function overload or template
  specialization from overload resolution (class template)
- ****Type traits** (C++11)** — defines a compile-time template-based interface
  to query or modify the properties of types

**C documentation for Static assertion**

---
*Source: https://en.cppreference.com/w/cpp/language/static_assert*
