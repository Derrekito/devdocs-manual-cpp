# std::filesystem::path::root_path

```cpp
path root_path() const;  // (since C++17)
```

Returns the root path of the path. If the path does not include root path,
returns `path()`.

Effectively returns `root_name() / root_directory()`.

### Parameters

(none)

### Return value

The root path of the path.

### Exceptions

May throw implementation-defined exceptions.

### Example

```cpp
#include <filesystem>
#include <iostream>
namespace fs = std::filesystem;

int main()
{
    std::cout << "Current root path is: " << fs::current_path().root_path() << '\n';
}
```

Possible output:

```text
Current root path is: "C:\"
```

### See also

- **root_name** — returns the root-name of the path, if present (public member
  function)
- **root_directory** — returns the root directory of the path, if present
  (public member function)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/path/root_path*
