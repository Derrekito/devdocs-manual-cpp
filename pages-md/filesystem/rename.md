# std::filesystem::rename

```cpp
void rename( const std::filesystem::path& old_p,
             const std::filesystem::path& new_p );  // (1) (since C++17)
void rename( const std::filesystem::path& old_p,
             const std::filesystem::path& new_p,
             std::error_code& ec ) noexcept;  // (2) (since C++17)
```

Moves or renames the filesystem object identified by `old_p` to `new_p` as if by
the POSIX `rename`:

- If `old_p` is a non-directory file, then `new_p` must be one of:
- If `old_p` is a directory, then `new_p` must be one of:
- Symlinks are not followed: if `old_p` is a symlink, it is itself renamed, not
  its target. If `new_p` is an existing symlink, it is itself erased, not its
  target.

Rename fails if

- `new_p` ends with dot or with dot-dot.
- `new_p` names a non-existing directory ending with a directory separator.
- `old_p` is a directory which is an ancestor of `new_p`.

### Parameters

- **old_p** — path to move or rename
- **new_p** — target path for the move/rename operation
- **ec** — out-parameter for error reporting in the non-throwing overload

### Return value

(none)

### Exceptions

Any overload not marked `noexcept` may throw `std::bad_alloc` if memory
allocation fails.

1) Throws `std::filesystem::filesystem_error` on underlying OS API errors,
   constructed with `old_p` as the first path argument, `new_p` as the second
   path argument, and the OS error code as the error code argument.

2) Sets a `std::error_code&` parameter to the OS API error code if an OS API
   call fails, and executes `ec.clear()` if no errors occur.

### Example

```cpp
#include <filesystem>
#include <fstream>
namespace fs = std::filesystem;

int main()
{
    std::filesystem::path p = std::filesystem::current_path() / "sandbox";
    std::filesystem::create_directories(p / "from");
    std::ofstream{ p / "from/file1.txt" }.put('a');
    std::filesystem::create_directory(p / "to");

//  fs::rename(p / "from/file1.txt", p / "to/"); // error: "to" is a directory
    fs::rename(p / "from/file1.txt", p / "to/file2.txt"); // OK
//  fs::rename(p / "from", p / "to"); // error: "to" is not empty
    fs::rename(p / "from", p / "to/subdir"); // OK

    std::filesystem::remove_all(p);
}
```

### See also

- **rename** — renames a file (function)
- **removeremove_all (C++17)(C++17)** — removes a file or empty directory
  removes a file or directory and all its contents, recursively (function)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/rename*
