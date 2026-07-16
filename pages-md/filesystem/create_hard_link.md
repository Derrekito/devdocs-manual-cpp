# std::filesystem::create_hard_link

```cpp
void create_hard_link( const std::filesystem::path& target,
                       const std::filesystem::path& link );  // (1) (since C++17)
void create_hard_link( const std::filesystem::path& target,
                       const std::filesystem::path& link,
                       std::error_code& ec ) noexcept;  // (2) (since C++17)
```

Creates a hard link `link` with its target set to `target` as if by POSIX
`link()`: the pathname `target` must exist.

Once created, `link` and `target` are two logical names that refer to the same
file (they are `equivalent`). Even if the original name `target` is deleted, the
file continues to exist and is accessible as `link`.

### Parameters

- **target** — path of the file or directory to link to
- **link** — path of the new hard link
- **ec** — out-parameter for error reporting in the non-throwing overload

### Return value

(none)

### Exceptions

Any overload not marked `noexcept` may throw `std::bad_alloc` if memory
allocation fails.

1) Throws `std::filesystem::filesystem_error` on underlying OS API errors,
   constructed with `target` as the first path argument, `link` as the second
   path argument, and the OS error code as the error code argument.

2) Sets a `std::error_code&` parameter to the OS API error code if an OS API
   call fails, and executes `ec.clear()` if no errors occur.

### Notes

Some operating systems do not support hard links at all or support them only for
regular files.

Some file systems do not support hard links regardless of the operating system:
the FAT file system used on memory cards and flash drives, for example.

Some file systems limit the number of links per file.

Hardlinking to directories is typically restricted to the superuser.

Hard links typically cannot cross filesystem boundaries.

The special pathname dot (`"."`) is a hard link to its parent directory. The
special pathname dot-dot `".."` is a hard link to the directory that is the
parent of its parent.

### Example

```cpp
#include <filesystem>
#include <fstream>
#include <iostream>
namespace fs = std::filesystem;

int main()
{
    fs::create_directories("sandbox/subdir");
    std::ofstream("sandbox/a").put('a'); // create regular file
    fs::create_hard_link("sandbox/a", "sandbox/b");
    fs::remove("sandbox/a");
    // read from the original file via surviving hard link
    char c = std::ifstream("sandbox/b").get();
    std::cout << c << '\n';
    fs::remove_all("sandbox");
}
```

Output:

```text
a
```

### See also

- **create_symlinkcreate_directory_symlink (C++17)(C++17)** — creates a symbolic
  link (function)
- **hard_link_count (C++17)** — returns the number of hard links referring to
  the specific file (function)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/create_hard_link*
