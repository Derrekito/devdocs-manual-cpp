# std::filesystem::path::root_directory

```cpp
path root_directory() const;  // (since C++17)
```

Returns the root directory of the generic-format path. If the path (in generic
format) does not include root directory, returns `path()`.

### Parameters

(none)

### Return value

The root directory of the path.

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
              << "root name " << p.root_name() << '\n'
              << "root directory " << p.root_directory() << '\n'
              << "relative path " << p.relative_path() << '\n';
}
```

Possible output:

```text
The current path "C:\Users\abcdef\Local Settings\temp" decomposes into:
root name "C:"
root directory "\"
relative path "Users\abcdef\Local Settings\temp"
```

### See also

- **root_name** — returns the root-name of the path, if present (public member
  function)
- **root_path** — returns the root path of the path, if present (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/path/root_directory*
