# std::basic_string<CharT,Traits,Allocator>::ends_with

```cpp
constexpr bool
    ends_with( std::basic_string_view<CharT, Traits> sv ) const noexcept;  // (1) (since C++20)
constexpr bool
    ends_with( CharT ch ) const noexcept;  // (2) (since C++20)
constexpr bool
    ends_with( const CharT* s ) const;  // (3) (since C++20)
```

Checks if the string ends with the given suffix. The suffix may be one of the
following:

1) A string view `sv` (which may be a result of implicit conversion from another
   `std::basic_string`).

2) A single character `ch`.

3) A null-terminated character string `s`.

All three overloads effectively return `std::basic_string_view<CharT,
Traits>(data(), size()).ends_with(x)`, where `x` is the parameter.

### Parameters

- **sv** — a string view which may be a result of implicit conversion from
  another `std::basic_string`
- **ch** — a single character
- **s** — a null-terminated character string

### Return value

`true` if the string ends with the provided suffix, `false` otherwise.

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
        && str.ends_with("C++20!"sv)  // (1)
        && !str.ends_with("c++20!"sv) // (1)
        && str.ends_with("C++20!"s)   // (1) implicit conversion string to string_view
        && !str.ends_with("c++20!"s)  // (1) implicit conversion string to string_view
        && str.ends_with('!')         // (2)
        && !str.ends_with('?')        // (2)
        && str.ends_with("C++20!")    // (3)
        && !str.ends_with("c++20!")   // (3)
    );
}
```

### See also

- **starts_with (C++20)** — checks if the string starts with the given prefix
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
*Source: https://en.cppreference.com/w/cpp/string/basic_string/ends_with*
