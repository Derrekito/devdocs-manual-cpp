# std::basic_ios<CharT,Traits>::widen

```cpp
char_type widen( char c ) const;
```

Converts a character `c` to its equivalent in the current locale. The result is
converted from `char` to character type used within the stream if needed.

Effectively calls `std::use_facet< std::ctype<char_type> >(getloc()).widen(c)`.

### Parameters

- **c** — character to convert

### Return value

Character converted to `char_type`

### See also

- **narrow** — narrows characters (public member function)
- **do_widen [virtual]** — converts a character or characters from char to
  `CharT` (virtual protected member function of `std::ctype<CharT>`)
- **btowc** — widens a single-byte narrow character to wide character, if
  possible (function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_ios/widen*
