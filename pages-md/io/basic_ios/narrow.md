# std::basic_ios<CharT,Traits>::narrow

```cpp
char narrow( char_type c, char dfault ) const;
```

Converts a current locale-specific character `c` to its standard equivalent. The
result is converted from `char_type` to `char` if needed. If no conversion can
be performed, the function returns `dfault`.

Effectively calls `std::use_facet< std::ctype<char_type> >(getloc()).narrow(c,
dfault);`.

### Parameters

- **c** — character to convert
- **dfault** — character to return if the conversion was unsuccessful

### Return value

Character converted to its standard equivalent and then to `char`. `dfault` is
returned if the conversion fails.

### See also

- **widen** — widens characters (public member function)
- **narrow** — invokes `do_narrow` (public member function of
  `std::ctype<CharT>`)
- **wctob** — narrows a wide character to a single-byte narrow character, if
  possible (function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_ios/narrow*
