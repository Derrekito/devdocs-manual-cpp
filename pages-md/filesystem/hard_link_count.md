# std::filesystem::hard_link_count

```cpp
std::uintmax_t hard_link_count( const std::filesystem::path& p );  // (1) (since C++17)
std::uintmax_t hard_link_count( const std::filesystem::path& p,
                                std::error_code& ec ) noexcept;  // (2) (since C++17)
```

Returns the number of hard links for the filesystem object identified by path
`p`.

The non-throwing overload returns `static_cast<uintmax_t>(-1)` on errors.

### Parameters

- **p** — path to examine
- **ec** — out-parameter for error reporting in the non-throwing overload

### Return value

The number of hard links for `p`.

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
    // On a POSIX-style filesystem, each directory has at least 2 hard links:
    // itself and the special member pathname "."
    fs::path p = fs::current_path();
    std::cout << "Number of hard links for current path is "
              << fs::hard_link_count(p) << '\n';

    // Each ".." is a hard link to the parent directory, so the total number
    // of hard links for any directory is 2 plus number of direct subdirectories
    p = fs::current_path() / ".."; // Each dot-dot is a hard link to parent
    std::cout << "Number of hard links for .. is "
              << fs::hard_link_count(p) << '\n';
}
```

Possible output:

```text
Number of hard links for current path is 2
Number of hard links for .. is 3
```

### See also

- **create_hard_link (C++17)** — creates a hard link (function)
- **hard_link_count** — returns the number of hard links referring to the file
  to which the directory entry refers (public member function of
  `std::filesystem::directory_entry`)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/hard_link_count*
