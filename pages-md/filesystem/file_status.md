# std::filesystem::file_status

```cpp
class file_status;  // (since C++17)
```

Stores information about the type and permissions of a file.

### Member functions

- **(constructor)** — constructs a `file_status` object (public member function)
- **operator=** — assigns contents (public member function)
- **(destructor)** — implicit destructor (public member function)
- **type** — gets or sets the type of the file (public member function)
- **permissions** — gets or sets the permissions of the file (public member
  function)

### Non-member functions

- **operator== (C++20)** — compares two `file_status` objects (function)

### See also

- **statussymlink_status (C++17)(C++17)** — determines file attributes
  determines file attributes, checking the symlink target (function)
- **statussymlink_status** — status of the file designated by this directory
  entry; status of the file/symlink designated by this directory entry (public
  member function of `std::filesystem::directory_entry`)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/file_status*
