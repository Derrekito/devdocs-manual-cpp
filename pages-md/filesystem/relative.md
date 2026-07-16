# std::filesystem::relative, std::filesystem::proximate

```cpp
path relative( const std::filesystem::path& p,
               std::error_code& ec );  // (1) (since C++17)
path relative( const std::filesystem::path& p,
               const std::filesystem::path& base = std::filesystem::current_path() );  // (2) (since C++17)
path relative( const std::filesystem::path& p,
               const std::filesystem::path& base,
               std::error_code& ec );  // (3) (since C++17)
path proximate( const std::filesystem::path& p,
                std::error_code& ec );  // (4) (since C++17)
path proximate( const std::filesystem::path& p,
                const std::filesystem::path& base = std::filesystem::current_path() );  // (5) (since C++17)
path proximate( const std::filesystem::path& p,
                const std::filesystem::path& base,
                std::error_code& ec );  // (6) (since C++17)
```

1) Returns `relative(p, current_path(), ec)`.

2,3) Returns `p` made relative to `base`. Resolves symlinks and normalizes both
   `p` and `base` before other processing. Effectively returns
   `std::filesystem::weakly_canonical(p).lexically_relative(std::filesystem::weakly_canonical(base))`
   or `std::filesystem::weakly_canonical(p,
   ec).lexically_relative(std::filesystem::weakly_canonical(base, ec))`, except
   the error code form returns `path()` at the first error occurrence, if any.

4) Returns `proximate(p, current_path(), ec)`.

5,6) Effectively returns
   `std::filesystem::weakly_canonical(p).lexically_proximate(std::filesystem::weakly_canonical(base))`
   or `std::filesystem::weakly_canonical(p,
   ec).lexically_proximate(std::filesystem::weakly_canonical(base, ec))`, except
   the error code form returns `path()` at the first error occurrence, if any.

### Parameters

- **p** — an existing path
- **base** — base path, against which `p` will be made relative/proximate
- **ec** — error code to store error status to

### Return value

1) `p` made relative against `current_path()`.

2,3) `p` made relative against `base`.

4) `p` made proximate against `current_path()`.

5,6) `p` made proximate against `base`.

### Exceptions

Any overload not marked `noexcept` may throw `std::bad_alloc` if memory
allocation fails.

2,5) Throws `std::filesystem::filesystem_error` on underlying OS API errors,
   constructed with `p` as the first path argument, `base` as the second path
   argument, and the OS error code as the error code argument.

1,3,4,6) Sets a `std::error_code&` parameter to the OS API error code if an OS
   API call fails, and executes `ec.clear()` if no errors occur.

### Example

```cpp
#include <filesystem>
#include <iostream>

void show(std::filesystem::path x, std::filesystem::path y)
{
    std::cout << "x:\t\t " << x << "\ny:\t\t " << y << '\n'
              << "relative(x, y):  "
              << std::filesystem::relative(x, y) << '\n'
              << "proximate(x, y): "
              << std::filesystem::proximate(x, y) << "\n\n";
}

int main()
{
    show("/a/b/c", "/a/b");
    show("/a/c", "/a/b");
    show("c", "/a/b");
    show("/a/b", "c");
}
```

Possible output:

```text
x:               "/a/b/c"
y:               "/a/b"
relative(x, y):  "c"
proximate(x, y): "c"

x:               "/a/c"
y:               "/a/b"
relative(x, y):  "../c"
proximate(x, y): "../c"

x:               "c"
y:               "/a/b"
relative(x, y):  ""
proximate(x, y): "c"

x:               "/a/b"
y:               "c"
relative(x, y):  ""
proximate(x, y): "/a/b"
```

### See also

- **path (C++17)** — represents a path (class)
- **absolute (C++17)** — composes an absolute path (function)
- **canonicalweakly_canonical (C++17)** — composes a canonical path (function)
- **lexically_normallexically_relativelexically_proximate** — converts path to
  normal form converts path to relative form converts path to proximate form
  (public member function of `std::filesystem::path`)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/relative*
