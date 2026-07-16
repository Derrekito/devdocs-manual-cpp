# std::filesystem::path::remove_filename

```cpp
path& remove_filename();  // (since C++17)
```

Removes a single generic-format filename component (as returned by `filename`)
from the given generic-format path.

After this function completes, `has_filename` returns `false`.

### Parameters

(none)

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
    fs::path p;
    std::cout << std::boolalpha
              << (p = "foo/bar").remove_filename() << '\t' << p.has_filename() << '\n'
              << (p = "foo/").remove_filename() << '\t' << p.has_filename() << '\n'
              << (p = "/foo").remove_filename() << '\t' << p.has_filename() << '\n'
              << (p = "/").remove_filename() << '\t' << p.has_filename() << '\n'
              << (p = "").remove_filename() << '\t' << p.has_filename() << '\n';
}
```

Output:

```text
"foo/"  false
"foo/"  false
"/"     false
"/"     false
""      false
```

### See also

- **filename** — returns the filename path component (public member function)
- **replace_filename** — replaces the last path component with another path
  (public member function)
- **has_filename** — checks if the corresponding path element is not empty
  (public member function)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/path/remove_filename*
