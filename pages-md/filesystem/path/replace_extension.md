# std::filesystem::path::replace_extension

```cpp
path& replace_extension( const path& replacement = path() );  // (since C++17)
```

Replaces the extension with `replacement` or removes it when the default value
of `replacement` is used.

Firstly, if this path has an `extension()`, it is removed from the
generic-format view of the pathname.

Then, a dot character is appended to the generic-format view of the pathname, if
`replacement` is not empty and does not begin with a dot character.

Then `replacement` is appended as if by `operator+=(replacement)`.

### Parameters

- **replacement** — the extension to replace with

### Return value

`*this`

### Exceptions

May throw implementation-defined exceptions.

### Notes

The type of `replacement` is `std::filesystem::path` even though it is not
intended to represent an object on the file system in order to correctly account
for the filesystem character encoding.

### Example

```cpp
#include <filesystem>
#include <iomanip>
#include <iostream>
#include <utility>

int main()
{
    const int width1{18}, width2{11}; // columns' width

    std::cout << std::left << std::setw(width1) << "Path:"
              << std::setw(width2) << "Ext:" << "Result:\n";
    for (const auto& [p, e] : {
            std::make_pair("/foo/bar.jpg", ".png"),
            {"/foo/bar.jpg", "png"},
            {"/foo/bar.jpg", "."},
            {"/foo/bar.jpg", ""},
            {"/foo/bar.", "png"},
            {"/foo/bar", ".png"},
            {"/foo/bar", "png"},
            {"/foo/bar", "."},
            {"/foo/bar", ""},
            {"/foo/.", ".png"},
            {"/foo/.", "png"},
            {"/foo/.", "."},
            {"/foo/.", ""},
            {"/foo/", ".png"},
            {"/foo/", "png"}})
    {
        std::filesystem::path path{p}, ext{e};
        std::cout << std::setw(width1) << path << std::setw(width2) << ext;
        path.replace_extension(ext);
        std::cout << path << '\n';
    }
}
```

Output:

```text
Path:             Ext:       Result:
"/foo/bar.jpg"    ".png"     "/foo/bar.png"
"/foo/bar.jpg"    "png"      "/foo/bar.png"
"/foo/bar.jpg"    "."        "/foo/bar."
"/foo/bar.jpg"    ""         "/foo/bar"
"/foo/bar."       "png"      "/foo/bar.png"
"/foo/bar"        ".png"     "/foo/bar.png"
"/foo/bar"        "png"      "/foo/bar.png"
"/foo/bar"        "."        "/foo/bar."
"/foo/bar"        ""         "/foo/bar"
"/foo/."          ".png"     "/foo/..png"
"/foo/."          "png"      "/foo/..png"
"/foo/."          "."        "/foo/.."
"/foo/."          ""         "/foo/."
"/foo/"           ".png"     "/foo/.png"
"/foo/"           "png"      "/foo/.png"
```

### See also

- **extension** — returns the file extension path component (public member
  function)
- **filename** — returns the filename path component (public member function)
- **stem** — returns the stem path component (filename without the final
  extension) (public member function)
- **has_extension** — checks if the corresponding path element is not empty
  (public member function)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/path/replace_extension*
