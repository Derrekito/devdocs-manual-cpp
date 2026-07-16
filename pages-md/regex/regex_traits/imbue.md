# std::regex_traits<CharT>::imbue

```cpp
locale_type imbue( locale_type loc );  // (since C++11)
```

Replaces the current locale with a copy of `loc`. If `loc` is different than the
current locale, then all cached data is invalidated.

After the call `getloc() == loc`.

### Parameters

- **loc** — the locale to imbue

### Return value

The current locale of the traits object.

If `imbue()` has been never called for this object, then the global locale at
the time of the call is returned. Otherwise, the locale passed to the last call
to `imbue()` is returned.

### Example

### See also

- **getloc** — gets the locale (public member function)

---
*Source: https://en.cppreference.com/w/cpp/regex/regex_traits/imbue*
