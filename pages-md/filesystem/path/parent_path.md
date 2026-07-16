# std::filesystem::path::parent_path

```cpp
path parent_path() const;  // (since C++17)
```

Returns the path to the parent directory.

If `has_relative_path()` returns false, the result is a copy of `*this`.
Otherwise, the result is a path whose generic format pathname is the longest
prefix of the generic format pathname of `*this` that produces one fewer element
in its iteration.

### Parameters

(none)

### Return value

The path to the parent directory, or a copy of `*this` if not
`has_relative_path()`.

### Exceptions

May throw implementation-defined exceptions.

### Example

```cpp
#include <filesystem>
#include <iostream>
namespace fs = std::filesystem;

int main()
{
    for (fs::path p : {"/var/tmp/example.txt", "/", "/var/tmp/."})
        std::cout << "The parent path of " << p
                  << " is " << p.parent_path() << '\n';
}
```

Possible output:

```text
The parent path of "/var/tmp/example.txt" is "/var/tmp"
The parent path of "/" is "/"
The parent path of "/var/tmp/." is "/var/tmp"
```

### See also

- **root_name** — returns the root-name of the path, if present (public member
  function)
- **root_directory** — returns the root directory of the path, if present
  (public member function)
- **root_path** — returns the root path of the path, if present (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/path/parent_path*
