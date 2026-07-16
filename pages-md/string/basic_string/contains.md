# std::basic_string<CharT,Traits,Allocator>::contains

```cpp
constexpr bool
    contains( std::basic_string_view<CharT,Traits> sv ) const noexcept;  // (1) (since C++23)
constexpr bool
    contains( CharT ch ) const noexcept;  // (2) (since C++23)
constexpr bool
    contains( const CharT* s ) const;  // (3) (since C++23)
```

Checks if the string contains the given substring. The substring may be one of
the following:

1) A string view `sv` (which may be a result of implicit conversion from another
   `std::basic_string`).

2) A single character `ch`.

3) A null-terminated character string `s`.

All three overloads are equivalent to `return find(x) != npos;`, where `x` is
the parameter.

### Parameters

- **sv** — a string view which may be a result of implicit conversion from
  another `std::basic_string`
- **ch** — a single character
- **s** — a null-terminated character string

### Return value

`true` if the string contains the provided substring, `false` otherwise.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_string_contains` | 202011L | (C++23) | `contains` functions

### Example

```cpp
#include <iomanip>
#include <iostream>
#include <string>
#include <string_view>
#include <type_traits>

template<typename SubstrType>
void test_substring(const std::string& str, SubstrType subs)
{
    constexpr char delim = std::is_scalar_v<SubstrType> ? '\'' : '\"';
    std::cout << std::quoted(str)
              << (str.contains(subs) ? " contains "
                                     : " does not contain ")
              << std::quoted(std::string{subs}, delim) << '\n';
}

int main()
{
    using namespace std::literals;

    auto helloWorld = "hello world"s;

    test_substring(helloWorld, "hello"sv);
    test_substring(helloWorld, "goodbye"sv);
    test_substring(helloWorld, 'w');
    test_substring(helloWorld, 'x');
}
```

Output:

```text
"hello world" contains "hello"
"hello world" does not contain "goodbye"
"hello world" contains 'w'
"hello world" does not contain 'x'
```

### See also

- **starts_with (C++20)** — checks if the string starts with the given prefix
  (public member function)
- **ends_with (C++20)** — checks if the string ends with the given suffix
  (public member function)
- **find** — finds the first occurrence of the given substring (public member
  function)
- **substr** — returns a substring (public member function)
- **contains (C++23)** — checks if the string view contains the given substring
  or character (public member function of
  `std::basic_string_view<CharT,Traits>`)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/contains*
