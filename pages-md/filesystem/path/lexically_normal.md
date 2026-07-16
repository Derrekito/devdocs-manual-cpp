# std::filesystem::path::lexically_normal, std::filesystem::path::lexically_relative, std::filesystem::path::lexically_proximate

```cpp
path lexically_normal() const;  // (1) (since C++17)
path lexically_relative( const path& base ) const;  // (2) (since C++17)
path lexically_proximate( const path& base ) const;  // (3) (since C++17)
```

1) Returns `*this` converted to normal form in its generic format.

2) Returns `*this` made relative to `base`. First, if `root_name() !=
   base.root_name()` is `true` or `is_absolute() != base.is_absolute()` is
   `true` or `(!has_root_directory() && base.has_root_directory())` is `true` or
   any filename in `relative_path()` or `base.relative_path()` can be
   interpreted as a root-name, returns a default-constructed path. Otherwise,
   first determines the first mismatched element of `*this` and `base` as if by
   `auto [a, b] = mismatch(begin(), end(), base.begin(), base.end())`, then if
   `a == end()` and `b == base.end()`, returns `path(".")`, otherwise, define N
   as the number of nonempty filename elements that are neither dot nor dot-dot
   in `[b, base.end())`, minus the number of dot-dot filename elements, If N <
   0, returns a default-constructed path, otherwise, if N = 0 and `a == end() ||
   a->empty()`, returns `path(".")`, otherwise returns an object composed from a
   default-constructed `path()` followed by N applications of
   `operator/=(path(".."))`, followed by one application of `operator/=` for
   each element in the half-open range `[``a``,``end()``)`.

3) If the value of `lexically_relative(base)` is not an empty path, return it.
   Otherwise return `*this`.

### Parameters

(none)

### Return value

1) The normal form of the path.

2) The relative form of the path.

3) The proximate form of the path.

### Exceptions

May throw implementation-defined exceptions.

### Notes

These conversions are purely lexical. They do not check that the paths exist, do
not follow symlinks, and do not access the filesystem at all. For
symlink-following counterparts of `lexically_relative` and
`lexically_proximate`, see `relative` and `proximate`.

On Windows, the returned `path` has backslashes (the preferred separators).

On POSIX, no filename in a relative path is acceptable as a root-name.

### Example

```cpp
#include <cassert>
#include <filesystem>
#include <iostream>
namespace fs = std::filesystem;

int main()
{
    assert(fs::path("a/./b/..").lexically_normal() == "a/");
    assert(fs::path("a/.///b/../").lexically_normal() == "a/");
    assert(fs::path("/a/d").lexically_relative("/a/b/c") == "../../d");
    assert(fs::path("/a/b/c").lexically_relative("/a/d") == "../b/c");
    assert(fs::path("a/b/c").lexically_relative("a") == "b/c");
    assert(fs::path("a/b/c").lexically_relative("a/b/c/x/y") == "../..");
    assert(fs::path("a/b/c").lexically_relative("a/b/c") == ".");
    assert(fs::path("a/b").lexically_relative("c/d") == "../../a/b");
    assert(fs::path("a/b").lexically_relative("/a/b") == "");
    assert(fs::path("a/b").lexically_proximate("/a/b") == "a/b");
}
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3070 | C++17 | a filename that can also be a root-name may cause
      surprising result | treated as error case
  LWG 3096 | C++17 | trailing "/" and "/." are handled incorrectly | corrected

### See also

- **relativeproximate (C++17)** — composes a relative path (function)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/path/lexically_normal*
