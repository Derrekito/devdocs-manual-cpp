# std::filesystem::path::path

```cpp
path() noexcept;  // (1) (since C++17)
path( const path& p );  // (2) (since C++17)
path( path&& p ) noexcept;  // (3) (since C++17)
path( string_type&& source, format fmt = auto_format );  // (4) (since C++17)
template< class Source >
path( const Source& source, format fmt = auto_format );  // (5) (since C++17)
template< class InputIt >
path( InputIt first, InputIt last, format fmt = auto_format );  // (6) (since C++17)
template< class Source >
path( const Source& source, const std::locale& loc, format fmt = auto_format );  // (7) (since C++17)
template< class InputIt >
path( InputIt first, InputIt last, const std::locale& loc, format fmt = auto_format );  // (8) (since C++17)
```

Constructs a new `path` object.

1) Constructs an empty path.

2) Copy constructor. Constructs a path whose pathname, in both native and
   generic formats, is the same as that of `p`.

3) Move constructor. Constructs a path whose pathname, in both native and
   generic formats, is the same as that of `p`, `p` is left in valid but
   unspecified state.

4-6) Constructs the path from a character sequence (format interpreted as
   specified by `fmt`) provided by `source` (4,5), which is a pointer or an
   input iterator to a null-terminated character/wide character sequence, an
   `std::basic_string` or an `std::basic_string_view`, or represented as a pair
   of input iterators [`first`, `last`) (6). Any of the character types `char`,
   `char8_t`, (since C++20)`char16_t`, `char32_t`, `wchar_t` is allowed, and the
   method of conversion to the native character set depends on the character
   type used by `source`. If the source character type is `char`, the encoding
   of the source is assumed to be the native narrow encoding (so no conversion
   takes place on POSIX systems). If the source character type is `char8_t`,
   conversion from UTF-8 to native filesystem encoding is used. (since C++20) If
   the source character type is `char16_t`, conversion from UTF-16 to native
   filesystem encoding is used. If the source character type is `char32_t`,
   conversion from UTF-32 to native filesystem encoding is used. If the source
   character type is `wchar_t`, the input is assumed to be the native wide
   encoding (so no conversion takes places on Windows).

7,8) Constructs the path from a character sequence (format interpreted as
   specified by `fmt`) provided by `source` (7), which is a pointer or an input
   iterator to a null-terminated character sequence, an `std::string`, an
   `std::string_view`, or represented as a pair of input iterators [`first`,
   `last`) (8). The only character type allowed is `char`. Uses `loc` to perform
   the character encoding conversion. If `value_type` is `wchar_t`, converts
   from to wide using the `std::codecvt<wchar_t, char, std::mbstate_t>` facet of
   `loc`. Otherwise, first converts to wide using the `std::codecvt<wchar_t,
   char, std::mbstate_t>` facet and then converts to filesystem native character
   type using `std::codecvt<wchar_t,value_type>` facet of `loc`.

(5) and (7) participate in overload resolution only if `Source` and `path` are
not the same type, and either:

- `Source` is a specialization of `std::basic_string` or
  `std::basic_string_view`, or
- `std::iterator_traits<std::decay_t<Source>>::value_type` is valid and denotes
  a possibly const-qualified encoding character type (`char`, `char8_t`, (since
  C++20)`char16_t`, `char32_t`, or `wchar_t`).

### Parameters

- **p** — a path to copy
- **source** — `std::basic_string`, `std::basic_string_view`, pointer to a
  null-terminated character string, or input iterator with a character value
  type that points to a null-terminated character sequence (the character type
  must be `char` for overload (7))
- **first, last** — pair of LegacyInputIterators that specify a character
  sequence
- **fmt** — enumerator of type `path::format` which specifies how pathname
  format is to be interpreted
- **loc** — locale that defines encoding conversion to use

**Type requirements**

**-`InputIt` must meet the requirements of LegacyInputIterator.**

**-The value type of `InputIt` must be one of the character types `char`, `wchar_t`, `char8_t`, (since C++20)`char16_t` and `char32_t` to use the overload (6).**

**-The value type of `InputIt` must be `char` to use the overload (8).**

### Exceptions

2,4-8) May throw implementation-defined exceptions.

### Notes

For portable pathname generation from Unicode strings, see `u8path`.
*(until C++20)*

`path` constructor supports creation from UTF-8 string when the source is a
sequence of `char8_t`.
*(since C++20)*

### Example

```cpp
#include <filesystem>
#include <iostream>
namespace fs = std::filesystem;

int main()
{
    fs::path p1 = "/usr/lib/sendmail.cf"; // portable format
    fs::path p2 = "C:\\users\\abcdef\\AppData\\Local\\Temp\\"; // native format
    fs::path p3 = U"D:/猫.txt"; // UTF-32 string
    fs::path p4 = u8"~/狗.txt"; // UTF-8 string

    std::cout << "p1 = " << p1 << '\n'
              << "p2 = " << p2 << '\n'
              << "p3 = " << p3 << '\n'
              << "p4 = " << p4 << '\n';
}
```

Output:

```text
p1 = "/usr/lib/sendmail.cf"
p2 = "C:\\users\\abcdef\\AppData\\Local\\Temp\\"
p3 = "D:/猫.txt"
p4 = "~/狗.txt"
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3244 | C++17 | constraint that `Source` cannot be `path` was missing |
      added

### See also

- **u8path (C++17)(deprecated in C++20)** — creates a `path` from a UTF-8
  encoded source (function)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/path/path*
