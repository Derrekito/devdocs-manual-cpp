# std::chars_format

```cpp
enum class chars_format {
    scientific = /*unspecified*/,
    fixed = /*unspecified*/,
    hex = /*unspecified*/,
    general = fixed | scientific
};  // (since C++17)
```

A BitmaskType used to specify floating-point formatting for `std::to_chars` and
`std::from_chars`.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_to_chars` | 201611L | (C++17) | Elementary string conversions
      (`std::to_chars`, `std::from_chars`)

### Example

### See also

- **to_chars (C++17)** — converts an integer or floating-point value to a
  character sequence (function)
- **from_chars (C++17)** — converts a character sequence to an integer or
  floating-point value (function)

---
*Source: https://en.cppreference.com/w/cpp/utility/chars_format*
