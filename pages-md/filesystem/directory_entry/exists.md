# std::filesystem::directory_entry::exists

```cpp
bool exists() const;  // (1) (since C++17)
bool exists( std::error_code& ec ) const noexcept;  // (2) (since C++17)
```

Checks whether the pointed-to object exists. Effectively returns:

1) `std::filesystem::exists(status())`,

2) `std::filesystem::exists(status(ec))`.

Note that `status()` follows symlinks to their targets.

### Parameters

- **ec** — out-parameter for error reporting in the non-throwing overload

### Return value

`true` if the referred-to filesystem object exists.

### Exceptions

Any overload not marked `noexcept` may throw `std::bad_alloc` if memory
allocation fails.

1) Throws `std::filesystem::filesystem_error` on underlying OS API errors,
   constructed with `p` as the first path argument and the OS error code as the
   error code argument.

2) Sets a `std::error_code&` parameter to the OS API error code if an OS API
   call fails, and executes `ec.clear()` if no errors occur.

### Example

```cpp
#include <filesystem>
#include <iostream>

int main()
{
    for (auto const str:
    {
        "/usr/bin/cat",
        "/usr/bin/mouse",
        "/usr/bin/python",
        "/usr/bin/bison",
        "/usr/bin/yacc",
        "/usr/bin/c++",
    })
    {
        std::filesystem::directory_entry entry{str};

        std::cout << "directory entry " << entry
                  << (entry.exists() ? " exists\n" : " does not exist\n");
    }
}
```

Possible output:

```text
// Output on a POSIX system:
directory entry "/usr/bin/cat" exist
directory entry "/usr/bin/mouse" does not exist
directory entry "/usr/bin/python" exists
directory entry "/usr/bin/bison" exists
directory entry "/usr/bin/yacc" does not exist
directory entry "/usr/bin/c++" exists
```

### See also

- **exists (C++17)** — checks whether path refers to existing file system object
  (function)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/directory_entry/exists*
