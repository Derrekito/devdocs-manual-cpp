# std::char_traits<char>::assign, std::char_traits<wchar_t>::assign, std::char_traits<char8_t>::assign, std::char_traits<char16_t>::assign, std::char_traits<char32_t>::assign

```cpp
static void assign( char_type& c1, const char_type& c2 );  // (until C++11)
static void assign( char_type& c1, const char_type& c2 ) noexcept;  // (since C++11) (constexpr since C++17)
static char_type* assign( char_type* ptr,
                          std::size_t count, char_type c2 );  // (2) (constexpr since C++20)
```

1) Assigns `c2` to `c1`, behaves identically to `c1 = c2`.

2) Assigns `c2` to each character in `count` characters in the character
   sequence pointed to by `ptr`.

See CharTraits for the general requirements on character traits for `X::assign`.

### Parameters

- **c1** — character to assign to
- **c2** — character value to assign
- **ptr** — pointer to a character sequence to assign to
- **count** — the length of the character sequence

### Return value

1) (none)

2) `ptr`

### Complexity

1) Constant.

2) Linear in `count`.

---
*Source: https://en.cppreference.com/w/cpp/string/char_traits/assign*
