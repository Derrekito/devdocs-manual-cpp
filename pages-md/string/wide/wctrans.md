# std::wctrans

```cpp
std::wctrans_t wctrans( const char* str );
```

Constructs a value of type `std::wctrans_t` that describes a `LC_CTYPE` category
of wide character mapping. It may be one of the standard mappings, or a
locale-specific mapping, such as `"tojhira"` or `"tojkata"`.

### Parameters

- **str** — C string holding the name of the desired mapping. The following
  values of `str` are supported in all C locales: Value of `str` Effect
  `"toupper"` identifies the mapping used by `towupper` `"tolower"` identifies
  the mapping used by `towlower`
- **`"toupper"`** — identifies the mapping used by `towupper`
- **`"tolower"`** — identifies the mapping used by `towlower`

### Return value

`std::wctrans_t` object suitable for use with `std::towctrans` to map wide
characters according to the named mapping of the current C locale or zero if
`str` does not name a mapping supported by the current C locale.

### Example

### See also

- **towctrans** — performs character mapping according to the specified
  `LC_CTYPE` mapping category (function)

**C documentation for `wctrans`**

---
*Source: https://en.cppreference.com/w/cpp/string/wide/wctrans*
