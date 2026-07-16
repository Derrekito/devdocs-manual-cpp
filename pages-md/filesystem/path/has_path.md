# std::filesystem::path::has_root_path, std::filesystem::path::has_root_name, std::filesystem::path::has_root_directory, std::filesystem::path::has_relative_path, std::filesystem::path::has_parent_path, std::filesystem::path::has_filename, std::filesystem::path::has_stem, std::filesystem::path::has_extension

```cpp
bool has_root_path() const;  // (1) (since C++17)
bool has_root_name() const;  // (2) (since C++17)
bool has_root_directory() const;  // (3) (since C++17)
bool has_relative_path() const;  // (4) (since C++17)
bool has_parent_path() const;  // (5) (since C++17)
bool has_filename() const;  // (6) (since C++17)
bool has_stem() const;  // (7) (since C++17)
bool has_extension() const;  // (8) (since C++17)
```

Checks whether the path contains the corresponding path element.

1) Checks whether `root_path()` is empty.

2) Checks whether `root_name()` is empty.

3) Checks whether `root_directory()` is empty.

4) Checks whether `relative_path()` is empty.

5) Checks whether `parent_path()` is empty.

6) Checks whether `filename()` is empty.

7) Checks whether `stem()` is empty.

8) Checks whether `extension()` is empty.

### Parameters

(none)

### Return value

`true` if the corresponding path component is not empty, `false` otherwise.

### Exceptions

May throw implementation-defined exceptions.

### Example

### See also

- **empty** — checks if the path is empty (public member function)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/path/has_path*
