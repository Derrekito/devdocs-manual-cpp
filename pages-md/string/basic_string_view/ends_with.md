# std::basic_string_view<CharT,Traits>::ends_with

```cpp
constexpr bool ends_with( basic_string_view sv ) const noexcept;  // (1) (since C++20)
constexpr bool ends_with( CharT ch ) const noexcept;  // (2) (since C++20)
constexpr bool ends_with( const CharT* s ) const;  // (3) (since C++20)
```

Checks if the string view ends with the given suffix, where

1) the suffix is a string view. Effectively returns `size() >= sv.size() &&
   compare(size() - sv.size(), npos, sv) == 0`.

2) the suffix is a single character. Effectively returns `!empty() &&
   Traits::eq(back(), ch)`.

3) the suffix is a null-terminated character string. Effectively returns
   `ends_with(basic_string_view(s))`.

### Parameters

- **sv** — a string view which may be a result of implicit conversion from
  `std::basic_string`
- **ch** — a single character
- **s** — a null-terminated character string

### Return value

`true` if the string view ends with the provided suffix, `false` otherwise.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_starts_ends_with` | 201711L | (C++20) | String prefix and suffix
      checking: `starts_with()` and `ends_with()`

### Example

```cpp
#include <cassert>
#include <string_view>

int main()
{
    using namespace std::literals;

    assert
    (""
        // (1) ends_with( basic_string_view sv )
        && std::string_view("https://cppreference.com").ends_with(".com"sv) == true
        && std::string_view("https://cppreference.com").ends_with(".org"sv) == false

        // (2) ends_with( CharT c )
        && std::string_view("C++20").ends_with('0') == true
        && std::string_view("C++20").ends_with('3') == false

        // (3) ends_with( const CharT* s )
        && std::string_view("string_view").ends_with("view") == true
        && std::string_view("string_view").ends_with("View") == false
    );
}
```

### See also

- **starts_with (C++20)** — checks if the string view starts with the given
  prefix (public member function)
- **starts_with (C++20)** — checks if the string starts with the given prefix
  (public member function of `std::basic_string<CharT,Traits,Allocator>`)
- **ends_with (C++20)** — checks if the string ends with the given suffix
  (public member function of `std::basic_string<CharT,Traits,Allocator>`)
- **contains (C++23)** — checks if the string contains the given substring or
  character (public member function of
  `std::basic_string<CharT,Traits,Allocator>`)
- **contains (C++23)** — checks if the string view contains the given substring
  or character (public member function)
- **compare** — compares two views (public member function)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string_view/ends_with*
