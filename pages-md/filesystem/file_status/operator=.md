# std::filesystem::file_status::operator=

```cpp
file_status& operator=( const file_status& other ) noexcept = default;  // (1) (since C++17)
file_status& operator=( file_status&& other ) noexcept = default;  // (2) (since C++17)
```

Copy- or move-assigns another file status object.

### Parameters

- **other** — another file_status object to assign

### Return value

`*this`

### Example

---
*Source: https://en.cppreference.com/w/cpp/filesystem/file_status/operator%3D*
