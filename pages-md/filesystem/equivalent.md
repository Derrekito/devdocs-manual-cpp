# std::filesystem::equivalent

```cpp
bool equivalent( const std::filesystem::path& p1,
                 const std::filesystem::path& p2 );  // (1) (since C++17)
bool equivalent( const std::filesystem::path& p1,
                 const std::filesystem::path& p2,
                 std::error_code& ec ) noexcept;  // (2) (since C++17)
```

Checks whether the paths `p1` and `p2` resolve to the same file system entity.

If either `p1` or `p2` does not exist, an error is reported.

The non-throwing overload returns `false` on errors.

### Parameters

- **p1, p2** — paths to check for equivalence
- **ec** — out-parameter for error reporting in the non-throwing overload

### Return value

`true` if the `p1` and `p2` refer to the same file or directory and their file
status is the same. `false` otherwise.

### Exceptions

Any overload not marked `noexcept` may throw `std::bad_alloc` if memory
allocation fails.

1) Throws `std::filesystem::filesystem_error` on underlying OS API errors,
   constructed with `p1` as the first path argument, `p2` as the second path
   argument, and the OS error code as the error code argument.

2) Sets a `std::error_code&` parameter to the OS API error code if an OS API
   call fails, and executes `ec.clear()` if no errors occur.

### Notes

Two paths are considered to resolve to the same file system entity if the two
candidate entities the paths resolve to are located on the same device at the
same location. For POSIX, this means that the `st_dev` and `st_ino` members of
their POSIX `stat` structure, obtained as if by POSIX `stat()`, are equal.

In particular, all hard links for the same file or directory are equivalent, and
a symlink and its target on the same file system are equivalent.

### Example

```cpp
#include <cstdint>
#include <filesystem>
#include <iostream>
namespace fs = std::filesystem;

int main()
{
    // hard link equivalency
    fs::path p1 = ".";
    fs::path p2 = fs::current_path();
    if (fs::equivalent(p1, p2))
        std::cout << p1 << " is equivalent to " << p2 << '\n';

    // symlink equivalency
    for (const fs::path lib : {"/lib/libc.so.6", "/lib/x86_64-linux-gnu/libc.so.6"})
    {
        try
        {
            p2 = lib.parent_path() / fs::read_symlink(lib);
        }
        catch (std::filesystem::filesystem_error const& ex)
        {
            std::cout << ex.what() << '\n';
            continue;
        }

        if (fs::equivalent(lib, p2))
            std::cout << lib << " is equivalent to " << p2 << '\n';
    }
}
```

Possible output:

```text
"." is equivalent to "/var/tmp/test"
filesystem error: read_symlink: No such file or directory [/lib/libc.so.6]
"/lib/x86_64-linux-gnu/libc.so.6" is equivalent to "/lib/x86_64-linux-gnu/libc-2.23.so"
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2937 | C++17 | error condition specified incorrectly | corrected

### See also

- **compare** — compares the lexical representations of two paths
  lexicographically (public member function of `std::filesystem::path`)
- **operator==operator!=operator<operator<=operator>operator>=operator<=>
  (C++17)(C++17)(until C++20)(C++17)(until C++20)(C++17)(until
  C++20)(C++17)(until C++20)(C++17)(until C++20)(C++20)** — lexicographically
  compares two paths (function)
- **statussymlink_status (C++17)(C++17)** — determines file attributes
  determines file attributes, checking the symlink target (function)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/equivalent*
