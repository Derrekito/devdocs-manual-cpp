# std::filesystem::copy_symlink

```cpp
void copy_symlink( const std::filesystem::path& from,
                   const std::filesystem::path& to);  // (1) (since C++17)
void copy_symlink( const std::filesystem::path& from,
                   const std::filesystem::path& to,
                   std::error_code& ec ) noexcept;  // (2) (since C++17)
```

Copies a symlink to another location.

1) Effectively calls `f(read_symlink(from), to)` where `f` is `create_symlink`
   or `create_directory_symlink` depending on whether `from` resolves to a file
   or directory.

2) Effectively calls `f(read_symlink(from, ec), to, ec)` where `f` is
   `create_symlink` or `create_directory_symlink` depending on whether `from`
   resolves to a file or directory.

### Parameters

- **from** — path to a symbolic link to copy
- **to** — destination path of the new symlink
- **ec** — out-parameter for error reporting in the non-throwing overload

### Return value

(none)

### Exceptions

Any overload not marked `noexcept` may throw `std::bad_alloc` if memory
allocation fails.

1) Throws `std::filesystem::filesystem_error` on underlying OS API errors,
   constructed with `from` as the first path argument, `to` as the second path
   argument, and the OS error code as the error code argument.

2) Sets a `std::error_code&` parameter to the OS API error code if an OS API
   call fails, and executes `ec.clear()` if no errors occur.

### Example

### See also

- **copy (C++17)** — copies files or directories (function)
- **copy_file (C++17)** — copies file contents (function)
- **create_symlinkcreate_directory_symlink (C++17)(C++17)** — creates a symbolic
  link (function)
- **read_symlink (C++17)** — obtains the target of a symbolic link (function)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/copy_symlink*
