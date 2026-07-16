# std::as_const

```cpp
template< class T >
constexpr std::add_const_t<T>& as_const( T& t ) noexcept;  // (1) (since C++17)
template< class T >
void as_const( const T&& ) = delete;  // (2) (since C++17)
```

1) Forms lvalue reference to const type of `t`.

2) const rvalue reference overload is deleted to disallow rvalue arguments.

### Possible implementation

```cpp
template<class T>
constexpr std::add_const_t<T>& as_const(T& t) noexcept
{
    return t;
}
```

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_as_const` | 201510L | (C++17) | `std::as_const`

### Example

```cpp
#include <cassert>
#include <string>
#include <type_traits>
#include <utility>

int main()
{
    std::string mutableString = "Hello World!";
    auto&& constRef = std::as_const(mutableString);

    mutableString.clear(); // OK
//  constRef.clear(); // Error: 'constRef' is 'const' qualified,
                      //        but 'clear' is not marked const

    assert(&constRef == &mutableString);
    assert(&std::as_const(mutableString) == &mutableString);

    using ExprType = std::remove_reference_t<decltype(std::as_const(mutableString))>;

    static_assert(std::is_same_v<std::remove_const_t<ExprType>, std::string>,
                  "ExprType should be some kind of string.");
    static_assert(!std::is_same_v<ExprType, std::string>,
                  "ExprType shouldn't be a mutable string.");
}
```

### See also

- **is_const (C++11)** — checks if a type is const-qualified (class template)
- **add_cvadd_constadd_volatile (C++11)(C++11)(C++11)** — adds const and/or
  volatile specifiers to the given type (class template)
- **remove_cvremove_constremove_volatile (C++11)(C++11)(C++11)** — removes const
  and/or volatile specifiers from the given type (class template)
- **ranges::as_const_viewviews::as_const (C++23)** — converts a `view` into a
  `constant_range` (class template) (range adaptor object)

---
*Source: https://en.cppreference.com/w/cpp/utility/as_const*
