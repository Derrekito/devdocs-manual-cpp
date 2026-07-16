# std::char_traits<char>::copy, std::char_traits<wchar_t>::copy, std::char_traits<char8_t>::copy, std::char_traits<char16_t>::copy, std::char_traits<char32_t>::copy

```cpp
static char_type*
    copy( char_type* dest, const char_type* src, std::size_t count );  // (constexpr since C++20)
```

Copies `count` characters from the character string pointed to by `src` to the
character string pointed to by `dest`.

If `[``dest``,``dest + count``)` and `[``src``,``src + count``)` overlap, the
behavior is undefined.

See CharTraits for the general requirements on character traits for `X::copy`.

### Parameters

- **dest** — pointer to a character string to copy to
- **src** — pointer to a character string to copy from
- **count** — the number of characters to copy

### Return value

`dest`

### Exceptions

Throws nothing.

### Complexity

Linear in `count`.

### See also

- **assign [static]** — assigns a character (public static member function)

---
*Source: https://en.cppreference.com/w/cpp/string/char_traits/copy*
