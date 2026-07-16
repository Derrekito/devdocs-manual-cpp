# std::filesystem::directory_entry::refresh

```cpp
void refresh();  // (1) (since C++17)
void refresh( std::error_code& ec ) noexcept;  // (2) (since C++17)
```

Examines the filesystem object referred to by this directory entry and stores
its attributes for retrieval with `status`, `exists`, `is_regular_file`, and
other status accessors.

If an error occurs, the value of any cached attributes is unspecified.

### Parameters

- **ec** — out-parameter for error reporting in the non-throwing overload

### Return value

(none)

### Exceptions

Any overload not marked `noexcept` may throw `std::bad_alloc` if memory
allocation fails.

1) Throws `std::filesystem::filesystem_error` on underlying OS API errors,
   constructed with `p` as the first path argument and the OS error code as the
   error code argument.

2) Sets a `std::error_code&` parameter to the OS API error code if an OS API
   call fails, and executes `ec.clear()` if no errors occur.

### Notes

Many low-level OS APIs for directory traversal retrieve file attributes along
with the next directory entry. The constructors and the non-const member
functions of `std::filesystem::directory_iterator` store these attributes, if
any, in the pointed-to `std::filesystem::directory_entry` without calling
**`directory_entry::refresh`**, which makes it possible to examine the
attributes of the directory entries as they are being iterated over, without
making additional system calls.

### Example

### See also

- **statussymlink_status** — status of the file designated by this directory
  entry; status of the file/symlink designated by this directory entry (public
  member function)
- **exists** — checks whether directory entry refers to existing file system
  object (public member function)
- **is_block_file** — checks whether the directory entry refers to block device
  (public member function)
- **is_character_file** — checks whether the directory entry refers to a
  character device (public member function)
- **is_directory** — checks whether the directory entry refers to a directory
  (public member function)
- **is_fifo** — checks whether the directory entry refers to a named pipe
  (public member function)
- **is_other** — checks whether the directory entry refers to an *other* file
  (public member function)
- **is_regular_file** — checks whether the directory entry refers to a regular
  file (public member function)
- **is_socket** — checks whether the directory entry refers to a named IPC
  socket (public member function)
- **is_symlink** — checks whether the directory entry refers to a symbolic link
  (public member function)
- **file_size** — returns the size of the file to which the directory entry
  refers (public member function)
- **hard_link_count** — returns the number of hard links referring to the file
  to which the directory entry refers (public member function)
- **last_write_time** — gets the time of the last data modification of the file
  to which the directory entry refers (public member function)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/directory_entry/refresh*
