# std::wctype

```cpp
std::wctype_t wctype( const char* str );
```

Constructs a value of type `std::wctype_t` that describes a `LC_CTYPE` category
of wide character classification. It may be one of the standard classification
categories, or a locale-specific category, such as `"jkanji"`.

### Parameters

- **str** — C string holding the name of the desired category

The following values of `str` are supported in all C locales:

- **`"alnum"`** — identifies the category used by `std::iswalnum`
- **`"alpha"`** — identifies the category used by `std::iswalpha`
- **`"blank"`** — identifies the category used by `std::iswblank` (C++11)
- **`"cntrl"`** — identifies the category used by `std::iswcntrl`
- **`"digit"`** — identifies the category used by `std::iswdigit`
- **`"graph"`** — identifies the category used by `std::iswgraph`
- **`"lower"`** — identifies the category used by `std::iswlower`
- **`"print"`** — identifies the category used by `std::iswprint`
- **`"space"`** — identifies the category used by `std::iswspace`
- **`"upper"`** — identifies the category used by `std::iswupper`
- **`"xdigit"`** — identifies the category used by `std::iswxdigit`

### Return value

`std::wctype_t` object suitable for use with `std::iswctype` to classify wide
characters according to the named category of the current C locale or zero if
`str` does not name a category supported by the current C locale.

### See also

- **iswctype** — classifies a wide character according to the specified
  `LC_CTYPE` category (function)

**C documentation for `wctype`**

---
*Source: https://en.cppreference.com/w/cpp/string/wide/wctype*
