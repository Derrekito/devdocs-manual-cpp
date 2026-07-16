# std::filesystem::file_status::file_status

```cpp
file_status() noexcept : file_status(std::filesystem::file_type::none) {}  // (1) (since C++17)
file_status( const file_status& ) noexcept = default;  // (2) (since C++17)
file_status( file_status&& ) noexcept = default;  // (3) (since C++17)
explicit file_status(
    std::filesystem::file_type type,
    std::filesystem::perms permissions = std::filesystem::perms::unknown ) noexcept;  // (4) (since C++17)
```

Constructs a new `file_status` object.

1) Default constructor that calls (4) with `std::filesystem::file_type::none`.

2,3) Copy and move constructors are defaulted.

4) Initializes the file status object with `type` as type and `permissions` as
   permissions.

### Parameters

- **type** — type of the file status
- **permissions** — permissions of the file status

### Example

---
*Source: https://en.cppreference.com/w/cpp/filesystem/file_status/file_status*
