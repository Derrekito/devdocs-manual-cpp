# std::filesystem::path::append, std::filesystem::path::operator/=

```cpp
path& operator/=( const path& p );  // (1) (since C++17)
template< class Source >
path& operator/=( const Source& source );  // (2) (since C++17)
template< class Source >
path& append( const Source& source );  // (3) (since C++17)
template< class InputIt >
path& append( InputIt first, InputIt last );  // (4) (since C++17)
```

1) If `p.is_absolute() || (p.has_root_name() && p.root_name() != root_name())`,
   then replaces the current path with `p` as if by `operator=(p)` and finishes.

* Otherwise, if `p.has_root_directory()`, then removes any root directory and
   the entire relative path from the generic format pathname of `*this`.

* Otherwise, if `has_filename() || (!has_root_directory() && is_absolute())`,
   then appends `path::preferred_separator` to the generic format of `*this`.

* Either way, then appends the native format pathname of `p`, omitting any
   root-name from its generic format, to the native format of `*this`.

```cpp
// Where "//host" is a root-name
path("//host")  / "foo" // the result is      "//host/foo" (appends with separator)
path("//host/") / "foo" // the result is also "//host/foo" (appends without separator)

// On POSIX,
path("foo") / ""      // the result is "foo/" (appends)
path("foo") / "/bar"; // the result is "/bar" (replaces)

// On Windows,
path("foo") / "C:/bar";  // the result is "C:/bar" (replaces)
path("foo") / "C:";      // the result is "C:"     (replaces)
path("C:") / "";         // the result is "C:"     (appends, without separator)
path("C:foo") / "/bar";  // yields "C:/bar"        (removes relative path, then appends)
path("C:foo") / "C:bar"; // yields "C:foo/bar"     (appends, omitting p's root-name)
```

2,3) Same as (1), but accepts any `std::basic_string`, `std::basic_string_view`,
   null-terminated multicharacter string, or an input iterator pointing to a
   null-terminated multicharacter sequence. Equivalent to `return
   operator/=(path(source));`.

4) Same as (1), but accepts any iterator pair that designates a multicharacter
   string. Equivalent to `return operator/=(path(first, last));`.

(2) and (3) participate in overload resolution only if `Source` and `path` are
not the same type, and either:

- `Source` is a specialization of `std::basic_string` or
  `std::basic_string_view`, or
- `std::iterator_traits<std::decay_t<Source>>::value_type` is valid and denotes
  a possibly const-qualified encoding character type (`char`, `char8_t`, (since
  C++20)`char16_t`, `char32_t`, or `wchar_t`).

### Parameters

- **p** — pathname to append
- **source** — `std::basic_string`, `std::basic_string_view`, null-terminated
  multicharacter string, or an input iterator pointing to a null-terminated
  multicharacter sequence, which represents a path name (either in portable or
  in native format)
- **first, last** — pair of LegacyInputIterators that specify a multicharacter
  sequence that represents a path name

**Type requirements**

**-`InputIt` must meet the requirements of LegacyInputIterator.**

**-The value type of `InputIt` must be one of the encoded character types (`char`, `wchar_t`, `char16_t` and `char32_t`).**

### Return value

`*this`

### Exceptions

May throw `std::bad_alloc` if memory allocation fails.

### Notes

These functions effectively yield an approximation of the meaning of the
argument path `p` in an environment where `*this` is the starting directory.

### Example

The output is produced on Windows.

```cpp
#include <filesystem>
#include <iostream>
namespace fs = std::filesystem;

int main()
{
    fs::path p1 = "C:";
    p1 /= "Users"; // does not insert a separator
    std::cout << "\"C:\" / \"Users\" == " << p1 << '\n';
    p1 /= "batman"; // inserts fs::path::preferred_separator, '\' on Windows
    std::cout << "\"C:\" / \"Users\" / \"batman\" == " << p1 << '\n';
}
```

Possible output:

```text
"C:" / "Users" == "C:Users"
"C:" / "Users" / "batman" == "C:Users\\batman"
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3244 | C++17 | constraint that `Source` cannot be `path` was missing |
      added

### See also

- **concatoperator+=** — concatenates two paths without introducing a directory
  separator (public member function)
- **operator/ (C++17)** — concatenates two paths with a directory separator
  (function)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/path/append*
