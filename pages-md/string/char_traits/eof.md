# std::char_traits<char>::eof, std::char_traits<wchar_t>::eof, std::char_traits<char8_t>::eof, std::char_traits<char16_t>::eof, std::char_traits<char32_t>::eof

```cpp
static int_type eof();  // (until C++11)
static constexpr int_type eof() noexcept;  // (since C++11)
```

Returns a value not equivalent to any valid value of type `char_type`.

See CharTraits for the general requirements on character traits for `X::eof`.

### Parameters

(none)

### Return value

  `char_type` | `eof()`
  char | `EOF`
  wchar_t | `WEOF`
  char8_t | an implementation-defined constant that cannot appear as a valid
      UTF-8 code unit
  char16_t | an implementation-defined constant that cannot appear as a valid
      UTF-16 code unit
  char32_t | an implementation-defined constant that cannot appear as a Unicode
      code point

### Complexity

Constant.

### See also

- **not_eof [static]** — checks whether a character is *eof* value (public
  static member function)

---
*Source: https://en.cppreference.com/w/cpp/string/char_traits/eof*
