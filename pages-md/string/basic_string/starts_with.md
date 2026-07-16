# std::basic_string<CharT,Traits,Allocator>::starts_with

```cpp
constexpr bool
    starts_with( std::basic_string_view<CharT,Traits> sv ) const noexcept;  // (1) (since C++20)
constexpr bool
    starts_with( CharT ch ) const noexcept;  // (2) (since C++20)
constexpr bool
    starts_with( const CharT* s ) const;  // (3) (since C++20)
```

Checks if the string begins with the given prefix. The prefix may be one of the
following:

1) A string view `sv` (which may be a result of implicit conversion from another
   `std::basic_string`).

2) A single character `ch`.

3) A null-terminated character string `s`.

All three overloads effectively return `std::basic_string_view<CharT,
Traits>(data(), size()).starts_with(x)`, where `x` is the parameter.

### Parameters

- **sv** — a string view which may be a result of implicit conversion from
  another `std::basic_string`
- **ch** — a single character
- **s** — a null-terminated character string

### Return value

`true` if the string begins with the provided prefix, `false` otherwise.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_starts_ends_with` | 201711L | (C++20) | String prefix and suffix
      checking: `starts_with()` and `ends_with()`

### Example

```cpp
#include <cassert>
#include <string>
#include <string_view>

int main()
{
    using namespace std::literals;

    const auto str = "Hello, C++20!"s;

    assert
    (""
        && str.starts_with("He"sv)  // (1)
        && !str.starts_with("he"sv) // (1)
        && str.starts_with("He"s)   // (1) implicit conversion string to string_view
        && !str.starts_with("he"s)  // (1) implicit conversion string to string_view
        && str.starts_with('H')     // (2)
        && !str.starts_with('h')    // (2)
        && str.starts_with("He")    // (3)
        && !str.starts_with("he")   // (3)
    );
}
```

### See also

- **ends_with (C++20)** — checks if the string ends with the given suffix
  (public member function)
- **starts_with (C++20)** — checks if the string view starts with the given
  prefix (public member function of `std::basic_string_view<CharT,Traits>`)
- **ends_with (C++20)** — checks if the string view ends with the given suffix
  (public member function of `std::basic_string_view<CharT,Traits>`)
- **contains (C++23)** — checks if the string contains the given substring or
  character (public member function)
- **contains (C++23)** — checks if the string view contains the given substring
  or character (public member function of
  `std::basic_string_view<CharT,Traits>`)
- **compare** — compares two strings (public member function)
- **substr** — returns a substring (public member function)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/starts_with*
