# std::filesystem::path::relative_path

```cpp
path relative_path() const;  // (since C++17)
```

Returns path relative to root-path, that is, a pathname composed of every
generic-format component of `*this` after root-path. If `*this` is an empty
path, returns an empty path.

### Parameters

(none)

### Return value

Path relative to the root path.

### Exceptions

May throw implementation-defined exceptions.

### Example

```cpp
#include <filesystem>
#include <iostream>
namespace fs = std::filesystem;

int main()
{
    fs::path p = fs::current_path();

    std::cout << "The current path " << p << " decomposes into:\n"
              << "root-path " << p.root_path() << '\n'
              << "relative path " << p.relative_path() << '\n';
}
```

Possible output:

```text
The current path "C:\Users\abcdef\Local Settings\temp" decomposes into:
root-path "C:\"
relative path "Users\abcdef\Local Settings\temp"
```

### See also

- **root_name** — returns the root-name of the path, if present (public member
  function)
- **root_directory** — returns the root directory of the path, if present
  (public member function)
- **root_path** — returns the root path of the path, if present (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/path/relative_path*
