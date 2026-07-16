# std::codecvt_base

```cpp
class codecvt_base;
```

The class `std::codecvt_base` provides the conversion status constants which are
inherited and used by the `std::codecvt` facets.

### Member types

- **`enum result { ok, partial, error, noconv };`** — Unscoped enumeration type

- **`ok`** — conversion was completed with no error
- **`partial`** — not all source characters were converted
- **`error`** — encountered an invalid character
- **`noconv`** — no conversion required, input and output types are the same

### Notes

The value `std::codecvt_base::partial` is used to indicate that either the
destination range is too short to receive the results of the conversion or the
input is truncated in the middle of an otherwise valid multibyte character.

### See also

- **codecvt** — converts between character encodings, including UTF-8, UTF-16,
  UTF-32 (class template)

---
*Source: https://en.cppreference.com/w/cpp/locale/codecvt_base*
