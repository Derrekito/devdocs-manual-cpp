# std::filesystem::file_status::type

```cpp
std::filesystem::file_type type() const noexcept;  // (1) (since C++17)
void type( std::filesystem::file_type type ) noexcept;  // (2) (since C++17)
```

Accesses the file type information.

1) Returns file type information.

2) Sets file type to `type`.

### Parameters

- **type** — file type to set to

### Return value

1) File type information.

2) (none)

### Example

---
*Source: https://en.cppreference.com/w/cpp/filesystem/file_status/type*
