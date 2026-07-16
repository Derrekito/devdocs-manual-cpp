# std::filesystem::path::stem

```cpp
path stem() const;  // (since C++17)
```

Returns the filename identified by the generic-format path stripped of its
extension.

Returns the substring from the beginning of `filename()` up to and not including
the last period (`.`) character, with the following exceptions:

- If the first character in the filename is a period, that period is ignored (a
  filename like ".profile" is not treated as an extension).
- If the filename is one of the special filesystem components dot or dot-dot, or
  if it has no periods, the function returns the entire `filename()`.

### Parameters

(none)

### Return value

The stem of the filename identified by the path (i.e. the filename without the
final extension).

### Exceptions

May throw implementation-defined exceptions.

### Example

```cpp
#include <filesystem>
#include <iostream>
namespace fs = std::filesystem;

int main()
{
    for (const fs::path p : {"/foo/bar.txt", "/foo/.bar", "foo.bar.baz.tar"})
        std::cout << "path: " << p << ", stem: " << p.stem() << '\n';

    std::cout << '\n';

    for (fs::path p = "foo.bar.baz.tar"; !p.extension().empty(); p = p.stem())
        std::cout << "path: " << p << ", extension: " << p.extension() << '\n';
}
```

Output:

```text
path: "/foo/bar.txt", stem: "bar"
path: "/foo/.bar", stem: ".bar"
path: "foo.bar.baz.tar", stem: "foo.bar.baz"

path: "foo.bar.baz.tar", extension: ".tar"
path: "foo.bar.baz", extension: ".baz"
path: "foo.bar", extension: ".bar"
```

### See also

- **filename** — returns the filename path component (public member function)
- **extension** — returns the file extension path component (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/path/stem*
