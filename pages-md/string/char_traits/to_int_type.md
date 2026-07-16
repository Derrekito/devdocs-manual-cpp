# std::char_traits<char>::to_int_type, std::char_traits<wchar_t>::to_int_type, std::char_traits<char8_t>::to_int_type, std::char_traits<char16_t>::to_int_type, std::char_traits<char32_t>::to_int_type

```cpp
static int_type to_int_type( char_type c );  // (until C++11)
static constexpr int_type to_int_type( char_type c ) noexcept;  // (since C++11)
```

Converts `c` to `int_type`.

See CharTraits for the general requirements on character traits for
`X::to_int_type`.

### Parameters

- **c** — value to convert

### Return value

A value equivalent to `c`.

### Complexity

Constant.

### Notes

For every valid value of `char_type`, there must be a unique value of `int_type`
distinct from `eof()`.

---
*Source: https://en.cppreference.com/w/cpp/string/char_traits/to_int_type*
