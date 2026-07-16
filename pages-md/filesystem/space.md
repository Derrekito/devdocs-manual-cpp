# std::filesystem::space

```cpp
std::filesystem::space_info space( const std::filesystem::path& p );  // (1) (since C++17)
std::filesystem::space_info space( const std::filesystem::path& p,
                                   std::error_code& ec ) noexcept;  // (2) (since C++17)
```

Determines the information about the filesystem on which the pathname `p` is
located, as if by POSIX `statvfs`.

Populates and returns an object of type `filesystem::space_info`, set from the
members of the POSIX `struct statvfs` as follows:

- `space_info.capacity` is set as if by `f_blocks * f_frsize`.
- `space_info.free` is set to `f_bfree * f_frsize`.
- `space_info.available` is set to `f_bavail * f_frsize`.
- Any member that could not be determined is set to
  `static_cast<std::uintmax_t>(-1)`.

The non-throwing overload sets all members to `static_cast<std::uintmax_t>(-1)`
on error.

### Parameters

- **p** — path to examine
- **ec** — out-parameter for error reporting in the non-throwing overload

### Return value

The filesystem information (a `filesystem::space_info` object).

### Exceptions

Any overload not marked `noexcept` may throw `std::bad_alloc` if memory
allocation fails.

1) Throws `std::filesystem::filesystem_error` on underlying OS API errors,
   constructed with `p` as the first path argument and the OS error code as the
   error code argument.

2) Sets a `std::error_code&` parameter to the OS API error code if an OS API
   call fails, and executes `ec.clear()` if no errors occur.

### Notes

`space_info.available` may be less than `space_info.free`.

### Example

```cpp
#include <cstdint>
#include <filesystem>
#include <iostream>

void print_space_info(auto const& dirs, int width = 15)
{
    (std::cout << std::left).imbue(std::locale("en_US.UTF-8"));
    for (const auto s : {"Capacity", "Free", "Available", "Dir"})
        std::cout << "│ " << std::setw(width) << s << ' ';
    for (std::cout << '\n'; auto const& dir : dirs)
    {
        std::error_code ec;
        const std::filesystem::space_info si = std::filesystem::space(dir, ec);
        for (auto x : {si.capacity, si.free, si.available})
            std::cout << "│ " << std::setw(width) << static_cast<std::intmax_t>(x) << ' ';
        std::cout << "│ " << dir << '\n';
    }
}

int main()
{
    const auto dirs = {"/dev/null", "/tmp", "/home", "/proc", "/null"};
    print_space_info(dirs);
}
```

Possible output:

```text
│ Capacity        │ Free            │ Available       │ Dir
│ 84,417,331,200  │ 24,069,705,728  │ 21,492,748,288  │ /dev/null
│ 84,417,331,200  │ 24,069,705,728  │ 21,492,748,288  │ /tmp
│ 250,321,567,744 │ 37,623,181,312  │ 25,152,159,744  │ /home
│ 0               │ 0               │ 0               │ /proc
│ -1              │ -1              │ -1              │ /null
```

### See also

- **space_info (C++17)** — information about free and available space on the
  filesystem (class)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/space*
