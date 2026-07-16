# std::filesystem::last_write_time

```cpp
std::filesystem::file_time_type last_write_time( const std::filesystem::path& p );  // (1) (since C++17)
std::filesystem::file_time_type last_write_time( const std::filesystem::path& p,
                                                 std::error_code& ec ) noexcept;  // (2) (since C++17)
void last_write_time( const std::filesystem::path& p,
                      std::filesystem::file_time_type new_time );  // (3) (since C++17)
void last_write_time( const std::filesystem::path& p,
                      std::filesystem::file_time_type new_time,
                      std::error_code& ec ) noexcept;  // (4) (since C++17)
```

1,2) Returns the time of the last modification of `p`, determined as if by
   accessing the member `st_mtime` of the POSIX `stat` (symlinks are followed).
   The non-throwing overload returns `file_time_type::min()` on errors.

3,4) Changes the time of the last modification of `p`, as if by POSIX `futimens`
   (symlinks are followed).

### Parameters

- **p** — path to examine or modify
- **new_time** — new modification time
- **ec** — out-parameter for error reporting in the non-throwing overload

### Return value

1,2) The time of the last modification of `p`.

3,4) (none)

### Exceptions

Any overload not marked `noexcept` may throw `std::bad_alloc` if memory
allocation fails.

1,3) Throws `std::filesystem::filesystem_error` on underlying OS API errors,
   constructed with `p` as the first path argument and the OS error code as the
   error code argument.

2,4) Sets a `std::error_code&` parameter to the OS API error code if an OS API
   call fails, and executes `ec.clear()` if no errors occur.

### Notes

It is not guaranteed that immediately after setting the write time, the value
returned by (1,2) is the same as what was passed as the argument to (3,4)
because the file system's time may be more granular than
`filesystem::file_time_type`.

### Example

```cpp
#include <chrono>
#include <filesystem>
#include <format>
#include <fstream>
#include <iostream>

using namespace std::chrono_literals;

int main()
{
    auto p = std::filesystem::temp_directory_path() / "example.bin";
    std::ofstream{p.c_str()}.put('a'); // create file

    std::filesystem::file_time_type ftime = std::filesystem::last_write_time(p);
    std::cout << std::format("File write time is {}\n", ftime);

    // move file write time 1 hour to the future
    std::filesystem::last_write_time(p, ftime + 1h);

    // read back from the filesystem
    ftime = std::filesystem::last_write_time(p);
    std::cout << std::format("File write time is {}\n", ftime);

    std::filesystem::remove(p);
}
```

Possible output:

```text
File write time is 2023-09-04 19:33:24.702639224
File write time is 2023-09-04 20:33:24.702639224
```

### See also

- **file_time_type (C++17)** — represents file time values (typedef)
- **last_write_time** — gets the time of the last data modification of the file
  to which the directory entry refers (public member function of
  `std::filesystem::directory_entry`)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/last_write_time*
