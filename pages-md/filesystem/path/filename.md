# std::filesystem::path::filename

```cpp
path filename() const;  // (since C++17)
```

Returns the generic-format filename component of the path.

Equivalent to `relative_path().empty() ? path() : *--end()`.

### Parameters

(none)

### Return value

The filename identified by the path.

### Exceptions

May throw implementation-defined exceptions.

### Example

```cpp
#include <filesystem>
#include <iostream>
namespace fs = std::filesystem;

int main()
{
    std::cout << fs::path("/foo/bar.txt").filename() << '\n'
              << fs::path("/foo/.bar").filename() << '\n'
              << fs::path("/foo/bar/").filename() << '\n'
              << fs::path("/foo/.").filename() << '\n'
              << fs::path("/foo/..").filename() << '\n'
              << fs::path(".").filename() << '\n'
              << fs::path("..").filename() << '\n'
              << fs::path("/").filename() << '\n'
              << fs::path("//host").filename() << '\n';
}
```

Output:

```text
"bar.txt"
".bar"
""
"."
".."
"."
".."
""
"host"
```

### See also

- **extension** — returns the file extension path component (public member
  function)
- **stem** — returns the stem path component (filename without the final
  extension) (public member function)
- **replace_filename** — replaces the last path component with another path
  (public member function)
- **has_filename** — checks if the corresponding path element is not empty
  (public member function)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/path/filename*
