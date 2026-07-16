# std::filesystem::directory_entry::directory_entry

```cpp
directory_entry() noexcept = default;  // (1) (since C++17)
directory_entry( const directory_entry& ) = default;  // (2) (since C++17)
directory_entry( directory_entry&& ) noexcept = default;  // (3) (since C++17)
explicit directory_entry( const std::filesystem::path& p );  // (4) (since C++17)
directory_entry( const std::filesystem::path& p, std::error_code& ec );  // (5) (since C++17)
```

Constructs a new `directory_entry` object.

1) Default constructor.

2) Defaulted copy constructor.

3) Defaulted move constructor.

4,5) Initializes the directory entry with path `p` and calls `refresh` to update
   the cached attributes. If an error occurs, the non-throwing overload leaves
   the `directory_entry` holding a default-constructed path.

### Parameters

- **p** — path to the filesystem object to which the directory entry will refer
- **ec** — out-parameter for error reporting in the non-throwing overload

### Exceptions

Any overload not marked `noexcept` may throw `std::bad_alloc` if memory
allocation fails.

1-4) Throws `std::filesystem::filesystem_error` on underlying OS API errors,
   constructed with `p` as the first path argument and the OS error code as the
   error code argument.

5) Sets a `std::error_code&` parameter to the OS API error code if an OS API
   call fails, and executes `ec.clear()` if no errors occur.

### Example

---
*Source: https://en.cppreference.com/w/cpp/filesystem/directory_entry/directory_entry*
