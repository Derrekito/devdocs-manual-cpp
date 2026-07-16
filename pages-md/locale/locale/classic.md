# std::locale::classic

```cpp
static const locale& classic();
```

Obtains a reference to the C++ locale that implements the classic `"C"` locale
semantics. This locale, unlike the global locale, cannot be altered.

### Parameters

none

### Return value

Returns a reference to the `"C"` locale.

### Notes

Some of the standard-required facets, such as the UTF-8/UTF-32 conversion facet
`std::codecvt<char32_t, char, std::mbstate_t>`, have no equivalents in the `"C"`
locale, but they are nevertheless present in the locale returned by
`std::locale::classic()`, as in any other locale constructed in a C++ program.

### Example

### See also

- **global [static]** — changes the global locale (public static member
  function)

---
*Source: https://en.cppreference.com/w/cpp/locale/locale/classic*
