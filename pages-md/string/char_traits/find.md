# std::char_traits<char>::find, std::char_traits<wchar_t>::find, std::char_traits<char8_t>::find, std::char_traits<char16_t>::find, std::char_traits<char32_t>::find

```cpp
static const char_type*
    find( const char_type* ptr, std::size_t count, const char_type& ch );  // (constexpr since C++17)
```

Searches for character `ch` within the first `count` characters of the sequence
pointed to by `ptr`.

See CharTraits for the general requirements on character traits for `X::find`.

### Parameters

- **ptr** — pointer to a character string to search
- **count** — the number of characters to analyze
- **ch** — the character to search for

### Return value

A pointer to the first character in the range specified by `[``ptr``,``ptr +
count``)` that compares equal to `ch`, or a null pointer if not found.

### Complexity

Linear in `count`.

---
*Source: https://en.cppreference.com/w/cpp/string/char_traits/find*
