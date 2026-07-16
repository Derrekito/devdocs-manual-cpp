# std::filesystem::exists

```cpp
bool exists( std::filesystem::file_status s ) noexcept;  // (1) (since C++17)
bool exists( const std::filesystem::path& p );  // (2) (since C++17)
bool exists( const std::filesystem::path& p, std::error_code& ec ) noexcept;  // (3) (since C++17)
```

Checks if the given file status or path corresponds to an existing file or
directory.

1) Equivalent to `status_known(s) && s.type() != file_type::not_found`.

2,3) Let `s` be a `std::filesystem::file_status` determined as if by `status(p)`
   or `status(p, ec)` (symlinks are followed), respectively. Returns
   `exists(s)`. The non-throwing overload calls `ec.clear()` if
   `status_known(s)`.

### Parameters

- **s** — file status to check
- **p** — path to examine
- **ec** — out-parameter for error reporting in the non-throwing overload

### Return value

`true` if the given path or file status corresponds to an existing file or
directory, `false` otherwise.

### Exceptions

Any overload not marked `noexcept` may throw `std::bad_alloc` if memory
allocation fails.

2) Throws `std::filesystem::filesystem_error` on underlying OS API errors,
   constructed with `p` as the first path argument and the OS error code as the
   error code argument.

3) Sets a `std::error_code&` parameter to the OS API error code if an OS API
   call fails, and executes `ec.clear()` if no errors occur.

No filesystem exception is thrown if object does not exist (use return value).

### Notes

The information provided by this function is usually also provided as a
byproduct of directory iteration. During directory iteration, calling
`exists(*iterator)` is less efficient than `exists(iterator->status())`.

### Example

```cpp
#include <cstdint>
#include <filesystem>
#include <fstream>
#include <iostream>
namespace fs = std::filesystem;

void demo_exists(const fs::path& p, fs::file_status s = fs::file_status{})
{
    std::cout << p;
    if (fs::status_known(s) ? fs::exists(s) : fs::exists(p))
        std::cout << " exists\n";
    else
        std::cout << " does not exist\n";
}

int main()
{
    const fs::path sandbox{"sandbox"};
    fs::create_directory(sandbox);
    std::ofstream{sandbox/"file"}; // create regular file
    fs::create_symlink("non-existing", sandbox/"symlink");

    demo_exists(sandbox);

    for (const auto& entry : fs::directory_iterator(sandbox))
        demo_exists(entry, entry.status()); // use cached status from directory entry

    fs::remove_all(sandbox);
}
```

Output:

```text
"sandbox" exists
"sandbox/symlink" does not exist
"sandbox/file" exists
```

### See also

- **statussymlink_status (C++17)(C++17)** — determines file attributes
  determines file attributes, checking the symlink target (function)
- **file_status (C++17)** — represents file type and permissions (class)
- **exists** — checks whether directory entry refers to existing file system
  object (public member function of `std::filesystem::directory_entry`)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/exists*
