# std::filesystem::filesystem_error::path1, std::filesystem::filesystem_error::path2

```cpp
const std::filesystem::path& path1() const noexcept;  // (since C++17)
const std::filesystem::path& path2() const noexcept;  // (since C++17)
```

Returns the paths that were stored in the exception object.

### Parameters

(none)

### Return value

References to the copy of the `path` parameters stored by the constructor.

### Example

```cpp
#include <cstdio>
#include <filesystem>
#include <iostream>

int main()
{
    const std::filesystem::path old_p{std::tmpnam(nullptr)},
                                new_p{std::tmpnam(nullptr)};
    try {
        std::filesystem::rename(old_p, new_p); // throws since old_p does not exist
    }
    catch(std::filesystem::filesystem_error const& ex) {
        std::cout
            << "what():  " << ex.what() << '\n'
            << "path1(): " << ex.path1() << '\n'
            << "path2(): " << ex.path2() << '\n';
    }
}
```

Possible output:

```text
what():  filesystem error: cannot rename: No such file or directory [/tmp/fileIzzRLB] [/tmp/fileiUDWlV]
path1(): "/tmp/fileIzzRLB"
path2(): "/tmp/fileiUDWlV"
```

---
*Source: https://en.cppreference.com/w/cpp/filesystem/filesystem_error/path*
