# std::filesystem::directory_entry::is_regular_file

```cpp
bool is_regular_file() const;  // (1) (since C++17)
bool is_regular_file( std::error_code& ec ) const noexcept;  // (2) (since C++17)
```

Checks whether the pointed-to object is a regular file. Effectively returns:

1) `std::filesystem::is_regular_file(status())`.

2) `std::filesystem::is_regular_file(status(ec))`.

### Parameters

- **ec** — out-parameter for error reporting in the non-throwing overload

### Return value

`true` if the referred-to filesystem object is a regular file, `false`
otherwise.

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
#include <string>

namespace fs = std::filesystem;

int main(int argc, const char* argv[])
{
    // Print out all regular files in a directory 'dir'.
    try
    {
        const auto dir{argc == 2 ? fs::path{argv[1]} : fs::current_path()};

        std::cout << "Current dir: " << dir << '\n'
                  << std::string(40, '-') << '\n';

        for (fs::directory_entry const& entry : fs::directory_iterator(dir))
            if (entry.is_regular_file())
                std::cout << entry.path().filename() << '\n';
    }
    catch(const fs::filesystem_error& e)
    {
        std::cout << e.what() << '\n';
    }
}
```

Possible output:

```text
Current dir: "/tmp/1588616534.9884143"
----------------------------------------
"main.cpp"
"a.out"
```

### See also

- **is_regular_file (C++17)** — checks whether the argument refers to a regular
  file (function)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/directory_entry/is_regular_file*
