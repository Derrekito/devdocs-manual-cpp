# std::char_traits<char>::to_char_type, std::char_traits<wchar_t>::to_char_type, std::char_traits<char8_t>::to_char_type, std::char_traits<char16_t>::to_char_type, std::char_traits<char32_t>::to_char_type

```cpp
static char_type to_char_type( int_type c );  // (until C++11)
static constexpr char_type to_char_type( int_type c ) noexcept;  // (since C++11)
```

Converts `c` to `char_type`. If there is no equivalent `char_type` value (such
as when `c` is a copy of the `eof()` value), the result is unspecified.

See CharTraits for the general requirements on character traits for
`X::to_char_type`.

### Parameters

- **c** — value to convert

### Return value

A value equivalent to `c`.

### Complexity

Constant.

---
*Source: https://en.cppreference.com/w/cpp/string/char_traits/to_char_type*
