# std::filesystem::path::operator=

```cpp
path& operator=( const path& p );  // (1) (since C++17)
path& operator=( path&& p ) noexcept;  // (2) (since C++17)
path& operator=( string_type&& source );  // (3) (since C++17)
template< class Source >
path& operator=( const Source& source );  // (4) (since C++17)
```

1) Replaces the contents of `*this` with a pathname whose both native and
   generic format representations equal those of `p`.

2) Replaces the contents of `*this` with a pathname whose both native and
   generic format representations equal those of `p`, possibly using move
   semantics: `p` is left in a valid, but unspecified state.

3) Replaces the contents of `*this` with a new path value constructed from
   detected-format `source`, which is left in valid, but unspecified state.
   Equivalent to `assign(std::move(source))`.

4) Replaces the contents of `*this` with a new path value constructed from
   detected-format `source` as if by overload (4) of the path constructor.
   Equivalent to `assign(source)`.

(4) participates in overload resolution only if `Source` and `path` are not the
same type, and either:

- `Source` is a specialization of `std::basic_string` or
  `std::basic_string_view`, or
- `std::iterator_traits<std::decay_t<Source>>::value_type` is valid and denotes
  a possibly const-qualified encoding character type (`char`, `char8_t`, (since
  C++20)`char16_t`, `char32_t`, or `wchar_t`).

### Parameters

- **p** — a path to assign
- **source** — a `std::basic_string`, `std::basic_string_view`, pointer to a
  null-terminated character/wide character string, or an input iterator that
  points to a null-terminated character/wide character sequence. The character
  type must be one of `char`, `char8_t`, (since C++20)`char16_t`, `char32_t`,
  `wchar_t`

### Return value

`*this`

### Example

```cpp
#include <filesystem>
namespace fs = std::filesystem;

int main()
{
    fs::path p = "C:/users/abcdef/AppData/Local";
    p = p / "Temp"; // move assignment
    const wchar_t* wstr = L"D:/猫.txt";
    p = wstr; // assignment from a source
}
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3244 | C++17 | constraint that `Source` cannot be `path` was missing |
      added

### See also

- **assign** — assigns contents (public member function)
- **(constructor)** — constructs a `path` (public member function)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/path/operator%3D*
