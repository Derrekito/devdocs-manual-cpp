# std::wbuffer_convert

```cpp
template<
    class Codecvt,
    class Elem = wchar_t,
    class Tr = std::char_traits<Elem>
> class wbuffer_convert : public std::basic_streambuf<Elem, Tr>  // (since C++11) (deprecated in C++17)
```

`std::wbuffer_convert` is a wrapper over stream buffer of type
`std::basic_streambuf<char>` which gives it the appearance of
`std::basic_streambuf<Elem>`. All I/O performed through `std::wbuffer_convert`
undergoes character conversion as defined by the facet `Codecvt`.
`std::wbuffer_convert` assumes ownership of the conversion facet, and cannot use
a facet managed by a locale. The standard facets suitable for use with
`std::wbuffer_convert` are `std::codecvt_utf8` for UTF-8/UCS-2 and UTF-8/UCS-4
conversions and `std::codecvt_utf8_utf16` for UTF-8/UTF-16 conversions.

This class template makes the implicit character conversion functionality of
`std::basic_filebuf` available for any `std::basic_streambuf`.

### Member types

- **`state_type`** — `Codecvt::state_type`

### Member functions

- **(constructor)** — constructs a new `wbuffer_convert` (public member
  function)
- **operator=** — the copy assignment operator is deleted (public member
  function)
- **(destructor)** — destructs the `wbuffer_convert` and its conversion facet
  (public member function)
- **rdbuf** — returns or replaces the underlying narrow stream buffer (public
  member function)
- **state** — returns the current conversion state (public member function)

### See also

  Character conversions | locale-defined multibyte (UTF-8, GB18030) | UTF-8 |
      UTF-16
  UTF-16 | `mbrtoc16` / `c16rtomb` (with C11's DR488) |
      `codecvt`<char16_t,char,mbstate_t> `codecvt_utf8_utf16`<char16_t>
      `codecvt_utf8_utf16`<char32_t> `codecvt_utf8_utf16`<wchar_t> | N/A
  UCS-2 | `c16rtomb` (without C11's DR488) | `codecvt_utf8`<char16_t> |
      `codecvt_utf16`<char16_t>
  UTF-32 | `mbrtoc32` / `c32rtomb` | `codecvt`<char32_t,char,mbstate_t>
      `codecvt_utf8`<char32_t> | `codecvt_utf16`<char32_t>
  system wchar_t: UTF-32 (non-Windows) UCS-2 (Windows) | `mbsrtowcs` /
      `wcsrtombs` `use_facet`<`codecvt` <wchar_t,char,mbstate_t>>(`locale`) |
      `codecvt_utf8`<wchar_t> | `codecvt_utf16`<wchar_t>

- **wstring_convert (C++11)(deprecated in C++17)** — performs conversions
  between a wide string and a byte string (class template)
- **codecvt_utf8 (C++11)(deprecated in C++17)(removed in C++26)** — converts
  between UTF-8 and UCS-2/UCS-4 (class template)
- **codecvt_utf8_utf16 (C++11)(deprecated in C++17)(removed in C++26)** —
  converts between UTF-8 and UTF-16 (class template)

---
*Source: https://en.cppreference.com/w/cpp/locale/wbuffer_convert*
