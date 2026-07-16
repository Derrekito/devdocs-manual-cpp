# std::filesystem::path::make_preferred

```cpp
path& make_preferred();  // (since C++17)
```

Converts all directory separators in the generic-format view of the path to the
preferred directory separator.

For example, on Windows, where `\` is the preferred separator, the path
`foo/bar` will be converted to `foo\bar`.

### Parameters

(none)

### Return value

`*this`

### Exceptions

May throw implementation-defined exceptions.

### Example

Windows can use `/` as a separator, but prefers `\`, so `make_preferred`
converts the forward slashes to backslashes. On the other hand, POSIX does not
use `\` as a separator, because backslashes are valid filename characters — the
Windows path on POSIX actually refers to a file with the name `"a\\b\\c"`. For
this reason the "separators" are not converted.

```cpp
#include <filesystem>
#include <iostream>

int main()
{
    std::filesystem::path
        windows_path("a\\b\\c"),
        posix_path("a/b/c");

    std::cout
        << "Windows path: "
        << windows_path << " -> "
        << windows_path.make_preferred() << '\n'
        << "POSIX path: "
        << posix_path << " -> "
        << posix_path.make_preferred() << '\n';
}
```

Output:

```text
// on Windows
Windows path: "a\\b\\c" -> "a\\b\\c"
POSIX path: "a/b/c" -> "a\\b\\c"

// on POSIX
Windows path: "a\\b\\c" -> "a\\b\\c"
POSIX path: "a/b/c" -> "a/b/c"
```

### See also

- **constexpr value_type preferred_separator [static]** — alternative directory
  separator which may be used in addition to the portable `/`. On Windows, this
  is the backslash character `\`. On POSIX, this is the same forward slash `/`
  as the portable separator (public static member constant)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/path/make_preferred*
