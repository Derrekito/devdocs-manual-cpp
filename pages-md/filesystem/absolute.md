# std::filesystem::absolute

```cpp
path absolute( const std::filesystem::path& p );  // (1) (since C++17)
path absolute( const std::filesystem::path& p, std::error_code& ec );  // (2) (since C++17)
```

Returns a path referencing the same file system location as `p`, for which
`filesystem::path::is_absolute()` is `true`.

2) This non-throwing overload returns default-constructed path if an error
   occurs.

### Parameters

- **p** — path to convert to absolute form
- **ec** — out-parameter for error reporting in the non-throwing overload

### Return value

Returns an absolute (although not necessarily canonical) pathname referencing
the same file as `p`.

### Exceptions

Any overload not marked `noexcept` may throw `std::bad_alloc` if memory
allocation fails.

1) Throws `std::filesystem::filesystem_error` on underlying OS API errors,
   constructed with `p` as the first path argument and the OS error code as the
   error code argument.

2) Sets a `std::error_code&` parameter to the OS API error code if an OS API
   call fails, and executes `ec.clear()` if no errors occur.

### Notes

Implementations are encouraged to not consider `p` not existing to be an error.

For POSIX-based operating systems, `std::filesystem::absolute(p)` is equivalent
to `std::filesystem::current_path() / p` except for when `p` is the empty path.

For Windows, `std::filesystem::absolute` may be implemented as a call to
`GetFullPathNameW`.

### Example

```cpp
#include <filesystem>
#include <iostream>
namespace fs = std::filesystem;

int main()
{
    std::filesystem::path p = "foo.c";
    std::cout << "Current path is " << std::filesystem::current_path() << '\n';
    std::cout << "Absolute path for " << p << " is " << fs::absolute(p) << '\n';
}
```

Possible output:

```text
Current path is "/tmp/1666297965.0051296"
Absolute path for "foo.c" is "/tmp/1666297965.0051296/foo.c"
```

### See also

- **canonicalweakly_canonical (C++17)** — composes a canonical path (function)
- **relativeproximate (C++17)** — composes a relative path (function)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/absolute*
