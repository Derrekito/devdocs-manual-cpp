# std::filesystem::path::is_absolute,is_relative

```cpp
bool is_absolute() const;  // (1) (since C++17)
bool is_relative() const;  // (2) (since C++17)
```

Checks whether the path is absolute or relative. An absolute path is a path that
unambiguously identifies the location of a file without reference to an
additional starting location. The first version returns `true` if the path, in
native format, is absolute, `false` otherwise; the second version the other way
round.

### Parameters

(none)

### Return value

1) `true` if the path is absolute, `false` otherwise.

2) `false` if the path is absolute, `true` otherwise.

### Exceptions

May throw implementation-defined exceptions.

### Notes

The path `"/"` is absolute on a POSIX OS, but is relative on Windows.

### See also

- **absolute (C++17)** — composes an absolute path (function)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/path/is_absrel*
