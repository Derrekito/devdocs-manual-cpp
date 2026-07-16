# std::filesystem::path::begin, std::filesystem::path::end

```cpp
iterator begin() const;  // (1) (since C++17)
iterator end() const;  // (2) (since C++17)
```

1) Returns an iterator to the first element of the path. If the path is empty,
   the returned iterator is equal to `end()`.

2) Returns an iterator one past the last element of the path. Dereferencing this
   iterator is undefined behavior.

The sequence denoted by this pair of iterators consists of the following:

1. root-name (if any).
1. root-directory (if any).
1. Sequence of file-names, omitting any directory separators.
1. If there is a directory separator after the last file-name in the path, the
   last element before the end iterator is an empty element.

### Parameters

(none)

### Return value

1) Iterator to the first element of the path.

2) Iterator one past the end of the path

### Exceptions

May throw implementation-defined exceptions.

### Example

```cpp
#include <filesystem>
#include <iostream>
namespace fs = std::filesystem;

int main()
{
    const fs::path p =
#   ifdef _WIN32
        "C:\\users\\abcdef\\AppData\\Local\\Temp\\";
#   else
        "/home/user/.config/Cppcheck/Cppcheck-GUI.conf";
#   endif
    std::cout << "Examining the path " << p << " through iterators gives\n";
    for (auto it = p.begin(); it != p.end(); ++it)
        std::cout << *it << " │ ";
    std::cout << '\n';
}
```

Possible output:

```text
--- Windows ---
Examining the path "C:\users\abcdef\AppData\Local\Temp\" through iterators gives
"C:" │ "/" │ "users" │ "abcdef" │ "AppData" │ "Local" │ "Temp" │ "" │

--- UNIX ---
Examining the path "/home/user/.config/Cppcheck/Cppcheck-GUI.conf" through iterators gives
"/" │ "home" │ "user" │ ".config" │ "Cppcheck" │ "Cppcheck-GUI.conf" │
```

---
*Source: https://en.cppreference.com/w/cpp/filesystem/path/begin*
