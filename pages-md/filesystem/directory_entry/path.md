# std::filesystem::directory_entry::path

```cpp
const std::filesystem::path& path() const noexcept;  // (since C++17)
operator const std::filesystem::path& () const noexcept;  // (since C++17)
```

Returns the full path the directory entry refers to.

### Parameters

(none)

### Return value

The full path the directory entry refers to.

### Example

```cpp
#include <filesystem>
#include <fstream>
#include <iostream>

namespace fs = std::filesystem;

std::string get_stem(const fs::path& p) { return p.stem().string(); }
void create_file(const fs::path& p) { std::ofstream o{p}; }

int main()
{
    const fs::path dir{"tmp_dir"};
    fs::create_directory(dir);
    create_file(dir / "one");
    create_file(dir / "two");
    create_file(dir / "three");

    for (const auto& file : fs::directory_iterator(dir))
    {
        // Explicit conversion
        std::cout << get_stem(file.path()) << '\n';

        // Implicit conversion
        std::cout << get_stem(file) << '\n';
    }

    fs::remove_all(dir);
}
```

Possible output:

```text
two
two
one
one
three
three
```

### See also

- **path (C++17)** — represents a path (class)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/directory_entry/path*
