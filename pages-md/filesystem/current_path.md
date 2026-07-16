# std::filesystem::current_path

```cpp
path current_path();  // (1) (since C++17)
path current_path( std::error_code& ec );  // (2) (since C++17)
void current_path( const std::filesystem::path& p );  // (3) (since C++17)
void current_path( const std::filesystem::path& p,
                   std::error_code& ec ) noexcept;  // (4) (since C++17)
```

Returns or changes the current path.

1,2) Returns the absolute path of the current working directory, obtained as if
   (in native format) by POSIX `getcwd`. (2) returns `path()` if error occurs.

3,4) Changes the current working directory to `p`, as if by POSIX `chdir`.

### Parameters

- **p** — path to change the current working directory to
- **ec** — out-parameter for error reporting in the non-throwing overloads

### Return value

1,2) Returns the current working directory.

3,4) (none)

### Exceptions

Any overload not marked `noexcept` may throw `std::bad_alloc` if memory
allocation fails.

1) Throws `std::filesystem::filesystem_error` on underlying OS API errors,
   constructed with the OS error code as the error code argument.

2) Sets a `std::error_code&` parameter to the OS API error code if an OS API
   call fails, and executes `ec.clear()` if no errors occur.

3) Throws `std::filesystem::filesystem_error` on underlying OS API errors,
   constructed with `p` as the first path argument and the OS error code as the
   error code argument.

4) Sets a `std::error_code&` parameter to the OS API error code if an OS API
   call fails, and executes `ec.clear()` if no errors occur.

### Notes

The current working directory is the directory, associated with the process,
that is used as the starting location in pathname resolution for relative paths.

The current path as returned by many operating systems is a dangerous global
variable. It may be changed unexpectedly by third-party or system library
functions, or by another thread.

### Example

```cpp
#include <filesystem>
#include <iostream>
namespace fs = std::filesystem;

int main()
{
    std::cout << "Current path is " << fs::current_path() << '\n'; // (1)
    fs::current_path(fs::temp_directory_path()); // (3)
    std::cout << "Current path is " << fs::current_path() << '\n';
}
```

Possible output:

```text
Current path is "D:/local/ConsoleApplication1"
Current path is "E:/Temp"
```

### See also

- **temp_directory_path (C++17)** — returns a directory suitable for temporary
  files (function)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/current_path*
