# std::filesystem::temp_directory_path

```cpp
path temp_directory_path();  // (1) (since C++17)
path temp_directory_path( std::error_code& ec );  // (2) (since C++17)
```

Returns the directory location suitable for temporary files.

### Parameters

(none)

### Return value

A directory suitable for temporary files. The path is guaranteed to exist and to
be a directory. The overload that takes `error_code&` argument returns an empty
path on error.

### Exceptions

Any overload not marked `noexcept` may throw `std::bad_alloc` if memory
allocation fails.

1) Throws `std::filesystem::filesystem_error` on underlying OS API errors,
   constructed with `path to be returned` as the first path argument and the OS
   error code as the error code argument.

2) Sets a `std::error_code&` parameter to the OS API error code if an OS API
   call fails, and executes `ec.clear()` if no errors occur.

### Notes

On POSIX systems, the path may be the one specified in the environment variables
`TMPDIR`, `TMP`, `TEMP`, `TEMPDIR`, and, if none of them are specified, the path
`"/tmp"` is returned.

On Windows systems, the path is typically the one returned by `GetTempPath`.

### Example

```cpp
#include <filesystem>
#include <iostream>
namespace fs = std::filesystem;

int main()
{
    std::cout << "Temp directory is " << fs::temp_directory_path() << '\n';
}
```

Possible output:

```text
Temp directory is "C:\Windows\TEMP\"
```

### See also

- **tmpfile** — creates and opens a temporary, auto-removing file (function)
- **current_path (C++17)** — returns or sets the current working directory
  (function)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/temp_directory_path*
