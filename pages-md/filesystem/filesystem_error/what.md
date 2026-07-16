# std::filesystem::filesystem_error::what

```cpp
const char* what() const noexcept override;  // (since C++17)
```

Returns an explanatory byte string. This explanatory string contains the
explanatory string passed at the time of construction. Implementations are
encouraged to include the pathnames of `path1()` and `path2()` in native format
and the `std::system_error::what()` string inside the returned string as well.

### Parameters

(none)

### Return value

A C-stye explanatory byte string that contains the explanatory string passed at
the time of construction.

### Example

```cpp
#include <cstdio>
#include <filesystem>
#include <iostream>
#include <string_view>
namespace fs = std::filesystem;

void explain(std::string_view note, fs::filesystem_error const& ex)
{
    std::cout << note << " exception:\n"
              << "what(): " << ex.what() << '\n'
              << "path1(): " << ex.path1() << ", path2(): "
              << ex.path2() << "\n\n";
}

int main()
{
    try
    {
        std::filesystem::rename("/dev", "/null");
    }
    catch(fs::filesystem_error const& ex)
    {
        explain("fs::rename()", ex);
    }

    for (auto const path : {"/bool", "/bin/cat", "/bin/mouse"})
        try
        {
            std::filesystem::create_directory(path);
        }
        catch(fs::filesystem_error const& ex)
        {
            explain("fs::create_directory()", ex);
        }
}
```

Possible output:

```text
fs::rename() exception:
what(): filesystem error: cannot rename: Permission denied [/dev] [/null]
path1(): "/dev", path2(): "/null"

fs::create_directory() exception:
what(): filesystem error: cannot create directory: Permission denied [/bool]
path1(): "/bool", path2(): ""

fs::create_directory() exception:
what(): filesystem error: cannot create directory: File exists [/bin/cat]
path1(): "/bin/cat", path2(): ""

fs::create_directory() exception:
what(): filesystem error: cannot create directory: Read-only file system [/bin/mouse]
path1(): "/bin/mouse", path2(): ""
```

---
*Source: https://en.cppreference.com/w/cpp/filesystem/filesystem_error/what*
