# std::filesystem::path::replace_filename

```cpp
path& replace_filename( const path& replacement );  // (since C++17)
```

Replaces a single filename component with `replacement`.

Equivalent to: `remove_filename(); return operator/=(replacement);`.

### Parameters

- **replacement** — `path` used for replacing the filename component

### Return value

`*this`

### Exceptions

May throw implementation-defined exceptions.

### Example

```cpp
#include <filesystem>
#include <iostream>
namespace fs = std::filesystem;

int main()
{
    std::cout << fs::path("/foo").replace_filename("bar") << '\n'
              << fs::path("/").replace_filename("bar") << '\n'
              << fs::path("").replace_filename("pub") << '\n';
}
```

Output:

```text
"/bar"
"/bar"
"pub"
```

### See also

- **replace_extension** — replaces the extension (public member function)
- **filename** — returns the filename path component (public member function)
- **remove_filename** — removes filename path component (public member function)
- **has_filename** — checks if the corresponding path element is not empty
  (public member function)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/path/replace_filename*
