# std::filesystem::is_other

```cpp
bool is_other( std::filesystem::file_status s ) noexcept;  // (1) (since C++17)
bool is_other( const std::filesystem::path& p );  // (2) (since C++17)
bool is_other( const std::filesystem::path& p, std::error_code& ec ) noexcept;  // (3) (since C++17)
```

Checks if the given file status or path corresponds to a file of type *other*
type. That is, the file exists, but is neither regular file, nor directory nor a
symlink.

1) Equivalent to `exists(s) && !is_regular_file(s) && !is_directory(s) &&
   !is_symlink(s)`.

2,3) Equivalent to `is_other(status(p))` or `is_other(status(p, ec))`,
   respectively.

### Parameters

- **s** — file status to check
- **p** — path to examine
- **ec** — error code to store the error status to

### Return value

`true` if the file indicated by `p` or if the type indicated `s` refers to a
file that is not regular file, directory, or a symlink, `false` otherwise. The
non-throwing overload returns `false` if an error occurs.

### Exceptions

Any overload not marked `noexcept` may throw `std::bad_alloc` if memory
allocation fails.

2,3) Sets a `std::error_code&` parameter to the OS API error code if an OS API
   call fails, and executes `ec.clear()` if no errors occur.

### Example

### See also

- **statussymlink_status (C++17)(C++17)** — determines file attributes
  determines file attributes, checking the symlink target (function)
- **file_status (C++17)** — represents file type and permissions (class)
- **status_known (C++17)** — checks whether file status is known (function)
- **is_block_file (C++17)** — checks whether the given path refers to block
  device (function)
- **is_character_file (C++17)** — checks whether the given path refers to a
  character device (function)
- **is_directory (C++17)** — checks whether the given path refers to a directory
  (function)
- **is_fifo (C++17)** — checks whether the given path refers to a named pipe
  (function)
- **is_regular_file (C++17)** — checks whether the argument refers to a regular
  file (function)
- **is_socket (C++17)** — checks whether the argument refers to a named IPC
  socket (function)
- **is_symlink (C++17)** — checks whether the argument refers to a symbolic link
  (function)
- **exists (C++17)** — checks whether path refers to existing file system object
  (function)
- **is_other** — checks whether the directory entry refers to an *other* file
  (public member function of `std::filesystem::directory_entry`)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/is_other*
