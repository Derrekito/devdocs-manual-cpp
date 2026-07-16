# std::mbstate_t

```cpp
struct mbstate_t;
```

The type mbstate_t is a trivial non-array type that can represent any of the
conversion states that can occur in an implementation-defined set of supported
multibyte character encoding rules. Zero-initialized value of `mbstate_t`
represents the initial conversion state, although other values of `mbstate_t`
may exist that also represent the initial conversion state.

Possible implementation of `mbstate_t` is a struct type holding an array
representing the incomplete multibyte character, an integer counter indicating
the number of bytes in the array that have been processed, and a representation
of the current shift state.

The following functions should not be called from multiple threads without
synchronization with the `std::mbstate_t*` argument of a null pointer due to
possible data races: `std::mbrlen`, `std::mbrtowc`, `std::mbsrtowcs`,
`std::mbtowc`, `std::wcrtomb`, `std::wcsrtombs`, `std::wctomb`.

### See also

- **mbsinit** — checks if the `std::mbstate_t` object represents initial shift
  state (function)

**C documentation for `mbstate_t`**

---
*Source: https://en.cppreference.com/w/cpp/string/multibyte/mbstate_t*
