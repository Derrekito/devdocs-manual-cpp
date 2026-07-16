# std::filesystem::path::format

```cpp
enum format {
    native_format,
    generic_format,
    auto_format
};  // (since C++17)
```

Determines how string representations of pathnames are interpreted by the
constructors of `std::filesystem::path` that accept strings.

### Constants

- **`native_format`** — Native pathname format
- **`generic_format`** — Generic pathname format
- **`auto_format`** — Implementation-defined pathname format, auto-detected
  where possible

### Notes

On POSIX systems, there is no difference between native and generic format.

### See also

- **(constructor)** — constructs a `path` (public member function)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/path/format*
