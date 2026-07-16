# std::basic_string_view<CharT,Traits>::contains

```cpp
constexpr bool contains( basic_string_view sv ) const noexcept;  // (1) (since C++23)
constexpr bool contains( CharT c ) const noexcept;  // (2) (since C++23)
constexpr bool contains( const CharT* s ) const;  // (3) (since C++23)
```

Checks if the string view contains the given substring, where

1) the substring is a string view.

2) the substring is a single character.

3) the substring is a null-terminated character string.

All three overloads are equivalent to `return find(x) != npos;`, where `x` is
the parameter.

### Parameters

- **sv** — a string view
- **c** — a single character
- **s** — a null-terminated character string

### Return value

`true` if the string view contains the provided substring, `false` otherwise.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_string_contains` | 202011L | (C++23) | `contains` functions

### Example

```cpp
#include <string_view>
using namespace std::literals;

static_assert
(
    // bool contains(basic_string_view x) const noexcept;
    "https://cppreference.com"sv.contains("cpp"sv) == true and
    "https://cppreference.com"sv.contains("php"sv) == false and

    // bool contains(CharT x) const noexcept;
    "C++23"sv.contains('+') == true and
    "C++23"sv.contains('-') == false and

    // bool contains(const CharT* x) const;
    std::string_view("basic_string_view").contains("string") == true and
    std::string_view("basic_string_view").contains("String") == false
);

int main() {}
```

### See also

- **starts_with (C++20)** — checks if the string view starts with the given
  prefix (public member function)
- **ends_with (C++20)** — checks if the string view ends with the given suffix
  (public member function)
- **find** — find characters in the view (public member function)
- **substr** — returns a substring (public member function)
- **contains (C++23)** — checks if the string contains the given substring or
  character (public member function of
  `std::basic_string<CharT,Traits,Allocator>`)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string_view/contains*
