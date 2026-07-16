# std::filesystem::directory_entry::replace_filename

```cpp
void replace_filename( const std::filesystem::path& p );  // (1) (since C++17)
void replace_filename( const std::filesystem::path& p, std::error_code& ec );  // (2) (since C++17)
```

Changes the filename of the directory entry.

Effectively modifies the path member by `path.replace_filename(p)` and calls
`refresh` to update the cached attributes. If an error occurs, the values of the
cached attributes are unspecified.

This function does not commit any changes to the filesystem.

### Parameters

- **p** — the path to append to the parent path of the currently stored path
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

### Example

```cpp
#include <filesystem>
#include <iostream>

int main()
{
    namespace fs = std::filesystem;
    {
        fs::directory_entry entry{"alpha"};
        std::cout << entry << '\n';
        entry.replace_filename("omega");
        std::cout << entry << '\n';
    }
    {
        fs::directory_entry entry{"/alpha/"};
        std::cout << entry << '\n';
        entry.replace_filename("omega");
        std::cout << entry << '\n';
    }
}
```

Output:

```text
"alpha"
"omega"
"/alpha/"
"/alpha/omega"
```

### See also

- **assign** — assigns contents (public member function)
- **replace_filename** — replaces the last path component with another path
  (public member function of `std::filesystem::path`)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/directory_entry/replace_filename*
