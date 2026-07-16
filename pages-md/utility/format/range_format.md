# std::range_format

```cpp
enum class range_format {
    disabled,
    map,
    set,
    sequence,
    string,
    debug_string
};  // (since C++23)
```

Specifies how a range should be formatted.

- **`disabled`** — disallows range default formatter to format range
- **`map`** — allows to format range as map representation with modified
  brackets `"{"`, `"}"` and separator `": "` for underlying *pair-like* types in
  the following format: { key-1 : value-1, ..., key-n : value-n }
- **`set`** — allows to format range as set representation with modified
  brackets `"{"` and `"}"` in the following format: { key-1, ..., key-n }
- **`sequence`** — allows to format range as sequence representation with
  default brackets `"["`, `"]"` and separator `", "` in the following format: [
  element-1, ..., element-n ]
- **`string`** — allows to format range as string
- **`debug_string`** — allows to format range as escaped string

### See also

- **formatter (C++20)** — defines formatting rules for a given type (class
  template)
- **format_kind (C++23)** — selects a suited `std::range_format` for a range
  (variable template)

---
*Source: https://en.cppreference.com/w/cpp/utility/format/range_format*
