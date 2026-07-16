# std::char_traits<char>::compare, std::char_traits<wchar_t>::compare, std::char_traits<char8_t>::compare, std::char_traits<char16_t>::compare, std::char_traits<char32_t>::compare

```cpp
static int compare( const char_type* s1, const char_type* s2,
                    std::size_t count );  // (constexpr since C++17)
```

Compares the first `count` characters of the character strings `s1` and `s2`.
The comparison is done lexicographically.

If `count` is zero, strings are considered equal.

See CharTraits for the general requirements on character traits for
`X::compare`.

### Parameters

- **s1, s2** — pointers to character strings to compare
- **count** — the number of characters to compare from each character string

### Return value

Negative value if `s1` is *less than* `s2`.

`​0​` if `s1` is *equal to* `s2`.

Positive value if `s1` is *greater than* `s2`.

### Complexity

Linear in `count`.

---
*Source: https://en.cppreference.com/w/cpp/string/char_traits/compare*
