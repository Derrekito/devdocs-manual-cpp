# std::char_traits<char>::eq_int_type, std::char_traits<wchar_t>::eq_int_type, std::char_traits<char8_t>::eq_int_type, std::char_traits<char16_t>::eq_int_type, std::char_traits<char32_t>::eq_int_type

```cpp
static bool eq_int_type( int_type c1, int_type c2 );  // (until C++11)
static constexpr bool eq_int_type( int_type c1, int_type c2 ) noexcept;  // (since C++11)
```

Checks whether two values of type `int_type` are equal.

See CharTraits for the general requirements on character traits for
`X::eq_int_type`.

### Parameters

- **c1, c2** — values to compare

### Return value

`true` if `c1` is equal to `c2`, `false` otherwise.

### Complexity

Constant.

---
*Source: https://en.cppreference.com/w/cpp/string/char_traits/eq_int_type*
