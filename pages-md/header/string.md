# Standard library header <string>

`<string>` is the strings library: it defines `std::basic_string`, the
owning, dynamically-sized character sequence, along with the traits
type that parameterizes character comparison/copying and the
free functions that convert numbers to and from text. `std::string` is
just `basic_string<char>`; the other aliases below cover wide and
Unicode-ish character types. Include it whenever you're building or
mutating text rather than just scanning it — for a non-owning view over
existing character data, see `<string_view>` instead.

### Includes

- **`<compare>`** (C++20) — three-way comparison operator support
- **`<initializer_list>`** (C++11) — `std::initializer_list` class
  template

### Classes

- **char_traits** — properties of a character type (comparison, copying,
  EOF handling)
- **char_traits<char>** / **char_traits<wchar_t>** — specializations
- **char_traits<char8_t>** (C++20) / **char_traits<char16_t>** /
  **char_traits<char32_t>** (C++11) — specializations
- **basic_string** — stores and manipulates a sequence of characters
- **`std::string`** — `basic_string<char>`
- **`std::wstring`** — `basic_string<wchar_t>`
- **`std::u8string`** (C++20) — `basic_string<char8_t>`
- **`std::u16string`** / **`std::u32string`** (C++11) —
  `basic_string<char16_t>` / `basic_string<char32_t>`
- **`std::pmr::basic_string`** / **`std::pmr::string`** (C++17) —
  polymorphic-allocator versions
- **`std::pmr::u16string`** / **`std::pmr::u32string`** /
  **`std::pmr::wstring`** (C++17) — polymorphic-allocator versions
- **`std::pmr::u8string`** (C++20) — polymorphic-allocator version
- **std::hash<basic_string>** (C++11) — hash support for strings

### Functions

- **operator+** — concatenates two strings, or a string and a char
- **operator==** and the relational operators — lexicographically
  compares two strings (`operator<=>` added in C++20; the other
  relational overloads were removed then, synthesized from it)
- **std::swap(basic_string)** — specializes `std::swap`
- **erase(basic_string)** / **erase_if(basic_string)** (C++20) — erases
  every element matching a value or predicate

### Input/output

- **operator<<** / **operator>>** — stream output and input for strings
- **getline** — reads a line from an input stream into a string

### Numeric conversions

- **stoi** / **stol** / **stoll** (C++11) — string to signed integer
- **stoul** / **stoull** (C++11) — string to unsigned integer
- **stof** / **stod** / **stold** (C++11) — string to floating-point
- **to_string** (C++11) — integral or floating-point value to `string`
- **to_wstring** (C++11) — integral or floating-point value to `wstring`

### Range access

- **begin** / **cbegin** (C++11 / C++14) — iterator to the start of a
  container or array
- **end** / **cend** (C++11 / C++14) — iterator to the end
- **rbegin** / **crbegin** (C++14) — reverse iterator to the start
- **rend** / **crend** (C++14) — reverse iterator to the end
- **size** / **ssize** (C++17 / C++20) — element count (`ssize` returns
  a signed type)
- **empty** (C++17) — is the container empty?
- **data** (C++17) — pointer to the underlying array

### Literals

- **operator""s** (C++14) — converts a character array literal to
  `basic_string`

---
*Source: https://en.cppreference.com/w/cpp/header/string*
