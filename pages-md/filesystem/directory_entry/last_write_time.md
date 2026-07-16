# std::filesystem::directory_entry::last_write_time

```cpp
std::filesystem::file_time_type last_write_time() const;  // (1) (since C++17)
std::filesystem::file_time_type last_write_time( std::error_code& ec ) const noexcept;  // (2) (since C++17)
```

If the last modification time is cached in this `directory_entry`, returns the
cached value. Otherwise, returns:

1) `std::filesystem::last_write_time(path())`.

2) `std::filesystem::last_write_time(path(), ec)`.

### Parameters

- **ec** — out-parameter for error reporting in the non-throwing overload

### Return value

The last modification time for the referred-to filesystem object.

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
#include <chrono>
#include <ctime>
#include <filesystem>
#include <format>
#include <iostream>
#include <string>

std::string to_string(const std::filesystem::file_time_type& ftime)
{
#if __cpp_lib_format
    return std::format("{:%c}", ftime);
#else
    std::time_t cftime = std::chrono::system_clock::to_time_t(
        std::chrono::file_clock::to_sys(ftime));
    std::string str = std::asctime(std::localtime(&cftime));
    str.pop_back(); // rm the trailing '\n' put by `asctime`
    return str;
#endif
}

int main()
{
    auto dir = std::filesystem::current_path();
    using Entry = std::filesystem::directory_entry;
    for (Entry const& entry : std::filesystem::directory_iterator(dir))
        std::cout << to_string(entry.last_write_time()) << " : "
                  << entry.path().filename() << '\n';
}
```

Possible output:

```text
Wed Sep  6 13:37:13.960314156 2023 : "main.cpp"
Wed Sep  6 13:37:42.690271828 2023 : "a.out"
```

### See also

- **last_write_time (C++17)** — gets or sets the time of the last data
  modification (function)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/directory_entry/last_write_time*
