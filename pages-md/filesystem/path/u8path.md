# std::filesystem::u8path

```cpp
template< class Source >
std::filesystem::path u8path( const Source& source );  // (1) (since C++17) (deprecated in C++20)
template< class InputIt >
std::filesystem::path u8path( InputIt first, InputIt last );  // (2) (since C++17) (deprecated in C++20)
```

Constructs a path `p` from a UTF-8 encoded sequence of `char`s or
`char8_t`s(since C++20), supplied either as an `std::string`, or as
`std::string_view`, or as a null-terminated multibyte string, or as a `[first,
last)` iterator pair.

- If `path::value_type` is `char` and native encoding is UTF-8, constructs a
  path directly as if by `path(source)` or `path(first, last)`. Note: this is
  the typical situation of a POSIX system that uses Unicode, such as Linux.
- Otherwise, if `path::value_type` is `wchar_t` and native encoding is UTF-16
  (this is the situation on Windows), or if `path::value_type` is `char16_t`
  (native encoding guaranteed UTF-16) or `char32_t` (native encoding guaranteed
  UTF-32), then first converts the UTF-8 character sequence to a temporary
  string `tmp` of type `path::string_type` and then the new path is constructed
  as if by `path(tmp)`.
- Otherwise (for non-UTF-8 narrow character encodings and for non-UTF-16
  `wchar_t`), first converts the UTF-8 character sequence to a temporary
  UTF-32-encoded string `tmp` of type `std::u32string`, and then the new path is
  constructed as if by `path(tmp)` (this path is taken on a POSIX system with a
  non-Unicode multibyte or single-byte encoded filesystem).

### Parameters

- **source** — a UTF-8 encoded `std::string`, `std::string_view`, a pointer to a
  null-terminated multibyte string, or an input iterator with char value type
  that points to a null-terminated multibyte string
- **first, last** — pair of LegacyInputIterators that specify a UTF-8 encoded
  character sequence

**Type requirements**

**-`InputIt` must meet the requirements of LegacyInputIterator.**

**-The value type of `Source` or `InputIt` must be `char` or `char8_t`.(since C++20)**

### Return value

The path constructed from the input string after conversion from UTF-8 to the
filesystem's native character encoding.

### Exceptions

May throw `std::bad_alloc` if memory allocation fails.

### Notes

On systems where native path format differs from the generic path format
(neither Windows nor POSIX systems are examples of such OSes), if the argument
to this function is using generic format, it will be converted to native.

### Example

```cpp
#include <cstdio>
#ifdef _MSC_VER
#include <fcntl.h>
#include <io.h>
#else
#include <clocale>
#include <locale>
#endif
#include <filesystem>
#include <fstream>

int main()
{
#ifdef _MSC_VER
    _setmode(_fileno(stderr), _O_WTEXT);
#else
    std::setlocale(LC_ALL, "");
    std::locale::global(std::locale(""));
#endif

    std::filesystem::path p(u8"要らない.txt");
    std::ofstream(p) << "File contents"; // Prior to LWG2676 uses operator string_type()
                                         // on MSVC, where string_type is wstring, only
                                         // works due to non-standard extension.
                                         // Post-LWG2676 uses new fstream constructors

    // Native string representation can be used with OS-specific APIs
#ifdef _MSC_VER
    if (std::FILE* f = _wfopen(p.c_str(), L"r"))
#else
    if (std::FILE* f = std::fopen(p.c_str(), "r"))
#endif
    {
        for (int ch; (ch = fgetc(f)) != EOF; std::putchar(ch))
        {}
        std::fclose(f);
    }

    std::filesystem::remove(p);
}
```

Possible output:

```text
File contents
```

### See also

- **path (C++17)** — represents a path (class)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/path/u8path*
