# std::char_traits<char>::move, std::char_traits<wchar_t>::move, std::char_traits<char8_t>::move, std::char_traits<char16_t>::move, std::char_traits<char32_t>::move

```cpp
static char_type*
    move( char_type* dest, const char_type* src, std::size_t count );  // (constexpr since C++20)
```

Copies `count` characters from the character string pointed to by `src` to the
character string pointed to by `dest`.

Performs correctly even if the ranges `[``src``,``src + count``)` and
`[``dest``,``dest + count``)` overlap.

See CharTraits for the general requirements on character traits for `X::move`.

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

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 7 | C++98 | the copy was guaranteed to perform correctly if `src` is in
      `[``dest``,``dest + count``)`, but not the other way round (i.e. `dest` is
      in `[``src``,``src + count``)`) | also guaranteed

---
*Source: https://en.cppreference.com/w/cpp/string/char_traits/move*
