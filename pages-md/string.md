# Strings library

The C++ strings library includes support for three general types of strings:

- `std::basic_string` - a templated class designed to manipulate strings of any
  character type.
- `std::basic_string_view` (since C++17) - a lightweight non-owning read-only
  view into a subsequence of a string.
- Null-terminated strings - arrays of characters terminated by a special *null*
  character.

### `std::basic_string`

The templated class `std::basic_string` generalizes how sequences of characters
are manipulated and stored. String creation, manipulation, and destruction are
all handled by a convenient set of class methods and related functions.

Several specializations of `std::basic_string` are provided for commonly-used
types:

- **`std::string`** — std::basic_string<char>
- **`std::wstring`** — std::basic_string<wchar_t>
- **`std::u8string` (since C++20)** — std::basic_string<char8_t>
- **`std::u16string` (since C++11)** — std::basic_string<char16_t>
- **`std::u32string` (since C++11)** — std::basic_string<char32_t>

### `std::basic_string_view`
The templated class `std::basic_string_view` provides a lightweight object that
offers read-only access to a string or a part of a string using an interface
similar to the interface of `std::basic_string`.
Several specializations of `std::basic_string_view` are provided for
commonly-used types:
- **`std::string_view`** — std::basic_string_view<char>
- **`std::wstring_view`** — std::basic_string_view<wchar_t>
- **`std::u8string_view` (since C++20)** — std::basic_string_view<char8_t>
- **`std::u16string_view`** — std::basic_string_view<char16_t>
- **`std::u32string_view`** — std::basic_string_view<char32_t>
*(since C++17)*

### Null-terminated strings

Null-terminated strings are arrays of characters that are terminated by a
special *null* character. C++ provides functions to create, inspect, and modify
null-terminated strings.

There are three types of null-terminated strings:

- null-terminated byte strings
- null-terminated multibyte strings
- null-terminated wide strings

### Additional support

#### `std::char_traits`

The string library also provides class template `std::char_traits` that defines
types and functions for `std::basic_string` and `std::basic_string_view`(since
C++17).

The following specializations are defined:

```cpp
template<> class char_traits<char>;
template<> class char_traits<wchar_t>;
template<> class char_traits<char8_t>;  // (since C++20)
template<> class char_traits<char16_t>;  // (since C++11)
template<> class char_traits<char32_t>;  // (since C++11)
```

#### Conversions and classification

The localizations library provides support for string conversions (e.g.
`std::wstring_convert` or `std::toupper`) as well as functions that classify
characters (e.g. `std::isspace` or `std::isdigit`).

### See also

**C documentation for Strings library**

---
*Source: https://en.cppreference.com/w/cpp/string*
