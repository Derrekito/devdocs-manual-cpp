# std::filesystem::directory_entry::hard_link_count

```cpp
std::uintmax_t hard_link_count() const;  // (1) (since C++17)
std::uintmax_t hard_link_count( std::error_code& ec ) const noexcept;  // (2) (since C++17)
```

If the number of hard links is cached in this `directory_entry`, returns the
cached value. Otherwise, returns:

1) `std::filesystem::hard_link_count(path())`,

2) `std::filesystem::hard_link_count(path(), ec)`.

### Parameters

- **ec** — out-parameter for error reporting in the non-throwing overload

### Return value

The number of hard links for the referred-to filesystem object.

### Exceptions

Any overload not marked `noexcept` may throw `std::bad_alloc` if memory
allocation fails.

1) Throws `std::filesystem::filesystem_error` on underlying OS API errors,
   constructed with `p` as the first path argument and the OS error code as the
   error code argument.

2) Sets a `std::error_code&` parameter to the OS API error code if an OS API
   call fails, and executes `ec.clear()` if no errors occur.

### Example

### See also

- **hard_link_count (C++17)** — returns the number of hard links referring to
  the specific file (function)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/directory_entry/hard_link_count*
