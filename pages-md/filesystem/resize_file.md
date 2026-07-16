# std::filesystem::resize_file

```cpp
void resize_file( const std::filesystem::path& p,
                  std::uintmax_t new_size );  // (1) (since C++17)
void resize_file( const std::filesystem::path& p,
                  std::uintmax_t new_size,
                  std::error_code& ec ) noexcept;  // (2) (since C++17)
```

Changes the size of the regular file named by `p` as if by POSIX `truncate`: if
the file size was previously larger than `new_size`, the remainder of the file
is discarded. If the file was previously smaller than `new_size`, the file size
is increased and the new area appears as if zero-filled.

### Parameters

- **p** — path to resize
- **new_size** — size that the file will now have
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

### Notes

On systems that support sparse files, increasing the file size does not increase
the space it occupies on the file system: space allocation takes place only when
non-zero bytes are written to the file.

### Example

Demonstrates the effect creating a sparse file has on the free space.

```cpp
#include <filesystem>
#include <fstream>
#include <iostream>
#include <locale>

int main()
{
    auto p = std::filesystem::temp_directory_path() / "example.bin";
    std::ofstream{p}.put('a');
    std::cout.imbue(std::locale{"en_US.UTF8"});
    std::cout << "File size:  " << std::filesystem::file_size(p) << '\n'
              << "Free space: " << std::filesystem::space(p).free << '\n';
    std::filesystem::resize_file(p, 64*1024); // resize to 64 KB
    std::cout << "File size:  " << std::filesystem::file_size(p) << '\n'
              << "Free space: " << std::filesystem::space(p).free << '\n';
    std::filesystem::remove(p);
}
```

Possible output:

```text
File size:  1
Free space: 42,954,108,928
File size:  65,536
Free space: 42,954,108,928
```

### See also

- **file_size (C++17)** — returns the size of a file (function)
- **space (C++17)** — determines available free space on the file system
  (function)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/resize_file*
