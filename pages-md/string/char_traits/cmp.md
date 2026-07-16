# std::char_traits<char>::eq/lt, std::char_traits<wchar_t>::eq/lt, std::char_traits<char8_t>::eq/lt, std::char_traits<char16_t>::eq/lt, std::char_traits<char32_t>::eq/lt

```cpp
static bool eq( char_type a, char_type b );  // (until C++11)
static constexpr bool eq( char_type a, char_type b ) noexcept;  // (since C++11)
static bool lt( char_type a, char_type b );  // (until C++11)
static constexpr bool lt( char_type a, char_type b ) noexcept;  // (since C++11)
```

Compares two characters.

1) Compares `a` and `b` for equality, behaves identically to

- `static_cast<unsigned char>(a) == static_cast<unsigned char>(b)`, if
  `char_type` is char,
- `a == b` otherwise.

2) Compares `a` and `b` in such a way that they are totally ordered, behaves
identically to

- `static_cast<unsigned char>(a) < static_cast<unsigned char>(b)`, if
  `char_type` is char,
- `a < b` otherwise.

See CharTraits for the general requirements on character traits for `X::eq` and
`X::lt`.

### Parameters

- **a, b** — character values to compare

### Return value

1) `true` if `a` and `b` are equal, `false` otherwise.

2) `true` if `a` is less than `b`, `false` otherwise.

### Complexity

Constant.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 467 | C++98 | for std::char_traits<char>, the semantics of `eq()` and
      `lt()` are the same as the built-in `==` and `<` on char respectively[1] |
      changed to built-in `==` and `<` on unsigned char

1. Most implementations call `std::memcmp()` for efficiency, which interprets
   the data as arrays of `unsigned char`. If `char` is signed on such
   implementations, `std::char_traits<char>` fails to satisfy the requirements
   of CharTraits.

---
*Source: https://en.cppreference.com/w/cpp/string/char_traits/cmp*
