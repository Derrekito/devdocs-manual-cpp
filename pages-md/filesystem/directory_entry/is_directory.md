# std::filesystem::directory_entry::is_directory

```cpp
bool is_directory() const;  // (1) (since C++17)
bool is_directory( std::error_code& ec ) const noexcept;  // (2) (since C++17)
```

Checks whether the pointed-to object is a directory. Effectively returns:

1) `std::filesystem::is_directory(status())`,

2) `std::filesystem::is_directory(status(ec))`.

### Parameters

- **ec** — out-parameter for error reporting in the non-throwing overload

### Return value

`true` if the referred-to filesystem object is a directory, `false` otherwise.

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
#include <string_view>

namespace fs = std::filesystem;

void check_directory(fs::directory_entry const& d, std::string_view rem = "")
{
    std::cout << "is_directory(" << d << "): " << d.is_directory() << rem << '\n';
}

int main()
{
    fs::directory_entry d1(".");
    fs::directory_entry d2("file.txt");
    fs::directory_entry d3("new_dir");

    std::cout << std::boolalpha;

    check_directory(d1);
    check_directory(d2);
    check_directory(d3, " (has not been created yet).");

    std::filesystem::create_directory("new_dir");

    check_directory(d3, " (before refresh).");
    d3.refresh();
    check_directory(d3, " (after refresh).");
}
```

Possible output:

```text
is_directory("."): true
is_directory("file.txt"): false
is_directory("new_dir"): false (has not been created yet).
is_directory("new_dir"): false (before refresh).
is_directory("new_dir"): true (after refresh).
```

### See also

- **is_directory (C++17)** — checks whether the given path refers to a directory
  (function)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/directory_entry/is_directory*
