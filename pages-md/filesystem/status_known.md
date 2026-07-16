# std::filesystem::status_known

```cpp
bool status_known( std::filesystem::file_status s ) noexcept;  // (1) (since C++17)
```

Checks if the given file status is known, equivalent to `s.type() !=
file_type::none`.

### Parameters

- **s** — file status to check

### Return value

`true` if the given file status is a known file status.

### Notes

Despite the name, the function checks for the file status of `file_type::none`
(meaning an error occurred), not `file_type::unknown` (meaning file exists, but
its type cannot be determined).

### See also

- **statussymlink_status (C++17)(C++17)** — determines file attributes
  determines file attributes, checking the symlink target (function)
- **statussymlink_status** — status of the file designated by this directory
  entry; status of the file/symlink designated by this directory entry (public
  member function of `std::filesystem::directory_entry`)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/status_known*
