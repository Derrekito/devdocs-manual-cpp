# std::char_traits<char>::not_eof, std::char_traits<wchar_t>::not_eof, std::char_traits<char8_t>::not_eof, std::char_traits<char16_t>::not_eof, std::char_traits<char32_t>::not_eof

```cpp
static int_type not_eof( int_type e );  // (until C++11)
static constexpr int_type not_eof( int_type e ) noexcept;  // (since C++11)
```

Given `e`, produce a suitable value that is not equivalent to *eof*.

This function is typically used when a value other than *eof* needs to be
returned, such as in implementations of `std::basic_streambuf::overflow()`.

See CharTraits for the general requirements on character traits for
`X::not_eof`.

### Parameters

- **e** — value to analyze

### Return value

`e` if `e` and *eof* value are not equivalent, or some other non-eof value
otherwise.

### Complexity

Constant.

### See also

- **eof [static]** — returns an *eof* value (public static member function)

---
*Source: https://en.cppreference.com/w/cpp/string/char_traits/not_eof*
