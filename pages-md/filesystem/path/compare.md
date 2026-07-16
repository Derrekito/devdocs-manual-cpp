# std::filesystem::path::compare

```cpp
int compare( const path& p ) const noexcept;  // (1) (since C++17)
int compare( const string_type& str ) const;
int compare( std::basic_string_view<value_type> str ) const;  // (2) (since C++17)
int compare( const value_type* s ) const;  // (3) (since C++17)
```

Compares the lexical representations of the path and another path.

1) If `root_name().native().compare(p.root_name().native())` is nonzero, returns
   that value.

Otherwise, if `has_root_directory() != p.has_root_directory()`, returns a value
   less than zero if `has_root_directory()` is `false` and a value greater than
   zero otherwise.

Otherwise returns a value less than, equal to or greater than `​0​` if the
   relative portion of the path (`relative_path()`) is respectively
   lexicographically less than, equal to or greater than the relative portion of
   `p` (`p.relative_path()`). Comparison is performed element-wise, as if by
   iterating both paths from `begin()` to `end()` and comparing the result of
   `native()` for each element.

2) Equivalent to `compare(path(str))`.

3) Equivalent to `compare(path(s))`.

### Parameters

- **p** — a path to compare to
- **str** — a string or string view representing path to compare to
- **s** — a null-terminated string representing path to compare to

### Return value

A value less than `​0​` if the path is lexicographically less than the given
path.

A value equal to `​0​` if the path is lexicographically equal to the given path.

A value greater than `​0​` if the path is lexicographically greater than the
given path.

### Exceptions

2,3) May throw implementation-defined exceptions.

### Notes

For two-way comparisons, binary operators may be more suitable.

### Example

```cpp
#include <filesystem>
#include <iostream>
#include <string_view>
namespace fs = std::filesystem;

void demo(fs::path p1, fs::path p2, std::string_view msg)
{
    std::cout << p1;
    const int rc = p1.compare(p2);
    if (rc < 0)
        std::cout << " < ";
    else if (rc > 0)
        std::cout << " > ";
    else
        std::cout << " == ";
    std::cout << p2 << " \t: " << msg << '\n';
}

int main()
{
    demo("/a/b/", "/a/b/", "simple");
    demo("/a/b/", "/a/b/c", "simple");
    demo("/a/b/../b", "/a/b", "no canonical conversion");
    demo("/a/b", "/a/b/.", "no canonical conversion");
    demo("/a/b/", "a/c", "absolute paths order after relative ones");
}
```

Output:

```text
"/a/b/" == "/a/b/"      : simple
"/a/b/" < "/a/b/c"        : simple
"/a/b/../b" > "/a/b"        : no canonical conversion
"/a/b" < "/a/b/."        : no canonical conversion
"/a/b/" > "a/c"                : absolute paths order after relative ones
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2936 | C++17 | compared all path elements directly | root name and root
      directory handled separately

### See also

- **operator==operator!=operator<operator<=operator>operator>=operator<=>
  (C++17)(C++17)(until C++20)(C++17)(until C++20)(C++17)(until
  C++20)(C++17)(until C++20)(C++17)(until C++20)(C++20)** — lexicographically
  compares two paths (function)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/path/compare*
