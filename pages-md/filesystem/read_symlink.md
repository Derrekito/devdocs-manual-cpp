# std::filesystem::read_symlink

```cpp
std::filesystem::path read_symlink( const std::filesystem::path& p );  // (1) (since C++17)
std::filesystem::path read_symlink( const std::filesystem::path& p,
                                    std::error_code& ec );  // (2) (since C++17)
```

If the path `p` refers to a symbolic link, returns a new path object which
refers to the target of that symbolic link.

It is an error if `p` does not refer to a symbolic link.

The non-throwing overload returns an empty path on errors.

### Parameters

- **p** — path to a symlink
- **ec** — out-parameter for error reporting in the non-throwing overload

### Return value

The target of the symlink (which may not necessarily exist).

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

namespace fs = std::filesystem;

int main()
{
    for (fs::path p : {"/usr/bin/gcc", "/bin/cat", "/bin/mouse"})
    {
        std::cout << p;
        fs::exists(p) ?
            fs::is_symlink(p) ?
                std::cout << " -> " << fs::read_symlink(p) << '\n' :
                std::cout << " exists but it is not a symlink\n" :
            std::cout << " does not exist\n";
    }
}
```

Possible output:

```text
"/usr/bin/gcc" -> "gcc-5"
"/bin/cat" exists but it is not a symlink
"/bin/mouse" does not exist
```

### See also

- **is_symlink (C++17)** — checks whether the argument refers to a symbolic link
  (function)
- **create_symlinkcreate_directory_symlink (C++17)(C++17)** — creates a symbolic
  link (function)
- **copy_symlink (C++17)** — copies a symbolic link (function)
- **statussymlink_status (C++17)(C++17)** — determines file attributes
  determines file attributes, checking the symlink target (function)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/read_symlink*
