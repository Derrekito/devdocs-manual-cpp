# C++ named requirements: LiteralType (since C++11)

Specifies that a type is a *literal type*. Literal types are the types of
`constexpr` variables and they can be constructed, manipulated, and returned
from `constexpr` functions.

Note: the standard doesn't define a named requirement with this name. This is a
type category defined by the core language. It is included here as a named
requirement only for consistency.

### Requirements

A literal type is any of the following:

- possibly cv-qualified `void` (so that `constexpr` functions can return void);
*(since C++14)*

- scalar type;
- reference type;
- an array of literal type;
- possibly cv-qualified class type that has all of the following properties:

- a lambda type,
*(since C++17)*

- an aggregate union type that
- a non-union aggregate type, and each of its anonymous union members
- a type with at least one `constexpr` (possibly template) constructor that is
  not a copy or move constructor,

### Notes

A type can be literal even if all of its constexpr constructors are deleted,
inaccessible, or cannot participate in overload resolution.

```cpp
struct A { constexpr A(int) = delete; char c; }; // A is a literal type
constexpr A v = std::bit_cast<A>('0'); // OK in C++20
                                       // v has literal type and thus can be constexpr
```

### Example

Literal type that extends string literals:

```cpp
#include <cstddef>
#include <iostream>
#include <stdexcept>

class conststr // conststr is a literal type
{
    const char* p;
    std::size_t sz;
public:
    template<std::size_t N>
    constexpr conststr(const char(&a)[N]) : p(a), sz(N - 1) {}

    constexpr char operator[](std::size_t n) const
    {
        return n < sz ? p[n] : throw std::out_of_range("");
    }

    constexpr std::size_t size() const { return sz; }
};

constexpr std::size_t count_lower(conststr s)
{
    std::size_t c{};
    for (std::size_t n{}; n != s.size(); ++n)
        if ('a' <= s[n] && s[n] <= 'z')
            ++c;
    return c;
}

// An output function that requires a compile-time constant N, for testing
template<int N>
struct constN
{
    constN() { std::cout << N << '\n'; }
};

int main()
{
    std::cout << "the number of lowercase letters in \"Hello, world!\" is ";
    constN<count_lower("Hello, world!")>(); // the string literal is implicitly
                                            // converted to conststr
}
```

Output:

```text
the number of lowercase letters in "Hello, world!" is 9
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  CWG 1453 | C++11 | a literal class could have volatile data members | not
      allowed
  CWG 1951 | C++11 C++14 | it was unclear whether cv-qualified `void` (C++14)
      and class types (C++11) are literal types | they are
  CWG 2096 | C++11 | for a union type to be literal, all its non- static data
      members must be literal | only one non-static data member needs to be
  CWG 2598 | C++11 | for a union type to be literal, it must have at least one
      non-static data member | it can have no non- static data member

### See also

- **is_literal_type (C++11)(deprecated in C++17)(removed in C++20)** — checks if
  a type is a literal type (class template)

---
*Source: https://en.cppreference.com/w/cpp/named_req/LiteralType*
