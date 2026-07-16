# std::regex_traits<CharT>::getloc

```cpp
locale_type getloc() const;  // (since C++11)
```

Returns the current locale of the traits object.

If `imbue()` has been never called for this object, then the global locale at
the time of the call is returned. Otherwise, the locale passed to the last call
to `imbue()` is returned.

### Parameters

(none)

### Return value

The current locale of the traits object.

### Example

### See also

- **imbue** — sets the locale (public member function)

---
*Source: https://en.cppreference.com/w/cpp/regex/regex_traits/getloc*
