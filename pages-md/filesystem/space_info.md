# std::filesystem::space_info

```cpp
struct space_info {
    std::uintmax_t capacity;
    std::uintmax_t free;
    std::uintmax_t available;
};  // (since C++17)
```

Represents the filesystem information as determined by `filesystem::space`.

### Member objects

- **capacity** — total size of the filesystem, in bytes (public member object)
- **free** — free space on the filesystem, in bytes (public member object)
- **available** — free space available to a non-privileged process (may be equal
  or less than `free`) (public member object)

### Non-member functions

- ****operator==** (C++20)** — compares two `space_info`s (function)

## operator==(std::filesystem::space_info)

```cpp
friend bool operator==( const space_info&, const space_info& ) = default;  // (since C++20)
```

Checks if `capacity`, `free` and `available` of both arguments are equal
respectively.

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when
`std::filesystem::space_info` is an associated class of the arguments.

The `!=` operator is synthesized from `operator==`.

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

- **space (C++17)** — determines available free space on the file system
  (function)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/space_info*
