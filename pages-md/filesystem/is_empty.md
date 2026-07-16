# std::filesystem::is_empty

```cpp
bool is_empty( const std::filesystem::path& p );  // (1) (since C++17)
bool is_empty( const std::filesystem::path& p, std::error_code& ec );  // (2) (since C++17)
```

Checks whether the given path refers to an empty file or directory.

### Parameters

- **p** — path to examine
- **ec** — error code to modify in case of error

### Return value

`true` if the path indicated by `p` refers to an empty file or directory,
`false` otherwise. The non-throwing overload returns `false` if an error occurs.

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
#include <cstdio>
#include <filesystem>
#include <fstream>
#include <iostream>

int main()
{
    namespace fs = std::filesystem;

    const fs::path tmp_dir{fs::temp_directory_path()};
    std::cout << std::boolalpha
              << "Temp dir: " << tmp_dir << '\n'
              << "is_empty(): " << fs::is_empty(tmp_dir) << '\n';

    const fs::path tmp_name{tmp_dir / std::tmpnam(nullptr)};
    std::cout << "Temp file: " << tmp_name << '\n';

    std::ofstream file{tmp_name.string()};
    std::cout << "is_empty(): " << fs::is_empty(tmp_name) << '\n';
    file << "cppreference.com";
    file.flush();
    std::cout << "is_empty(): " << fs::is_empty(tmp_name) << '\n'
              << "file_size(): " << fs::file_size(tmp_name) << '\n';
    file.close();
    fs::remove(tmp_name);
}
```

Possible output:

```text
Temp dir: "/tmp"
is_empty(): false
Temp file: "/tmp/fileCqd9DM"
is_empty(): true
is_empty(): false
file_size(): 16
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3013 | C++17 | `error_code` overload marked noexcept but can allocate
      memory | noexcept removed

### See also

- **statussymlink_status (C++17)(C++17)** — determines file attributes
  determines file attributes, checking the symlink target (function)
- **exists (C++17)** — checks whether path refers to existing file system object
  (function)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/is_empty*
