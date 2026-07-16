# std::filesystem::path::concat, std::filesystem::path::operator+=

```cpp
path& operator+=( const path& p );  // (1) (since C++17)
path& operator+=( const string_type& str );
path& operator+=( std::basic_string_view<value_type> str );  // (2) (since C++17)
path& operator+=( const value_type* ptr );  // (3) (since C++17)
path& operator+=( value_type x );  // (4) (since C++17)
template< class CharT >
path& operator+=( CharT x );  // (5) (since C++17)
template< class Source >
path& operator+=( const Source& source );  // (6) (since C++17)
template< class Source >
path& concat( const Source& source );  // (7) (since C++17)
template< class InputIt >
path& concat( InputIt first, InputIt last );  // (8) (since C++17)
```

Concatenates the current path and the argument

1-3,6,7) Appends `path(p).native()` to the pathname stored in `*this` in the
   native format. This directly manipulates the value of `native()` and may not
   be portable between operating systems.

4,5) Same as `return *this += std::basic_string_view(&x, 1);`.

8) Same as `return *this += path(first, last);`.

(6) and (7) participate in overload resolution only if `Source` and `path` are
not the same type, and either:

- `Source` is a specialization of `std::basic_string` or
  `std::basic_string_view`, or
- `std::iterator_traits<std::decay_t<Source>>::value_type` is valid and denotes
  a possibly const-qualified encoding character type (`char`, `char8_t`, (since
  C++20)`char16_t`, `char32_t`, or `wchar_t`).

### Parameters

- **p** — path to append
- **str** — string or string view to append
- **ptr** — pointer to the beginning of a null-terminated string to append
- **x** — single character to append
- **source** — `std::basic_string`, `std::basic_string_view`, null-terminated
  multicharacter string, or an input iterator pointing to a null-terminated
  multicharacter sequence, which represents a path name (either in portable or
  in native format)
- **first, last** — pair of LegacyInputIterators that specify a multicharacter
  sequence that represents a path name

**Type requirements**

**-`InputIt` must meet the requirements of LegacyInputIterator.**

**-The value type of `InputIt` must be one of the encoded character types (`char`, `wchar_t`, `char16_t` and `char32_t`).**

**-`CharT` must be one of the encoded character types (`char`, `wchar_t`, `char16_t` and `char32_t`).**

### Return value

`*this`

### Exceptions

May throw `std::bad_alloc` if memory allocation fails.

### Notes

Unlike with `append()` or `operator/=`, additional directory separators are
never introduced.

### Example

```cpp
#include <filesystem>
#include <iostream>
#include <string>

int main()
{
    std::filesystem::path p1; // an empty path
    p1 += "var"; // does not insert a separator
    std::cout << R"("" + "var" --> )" << p1 << '\n';
    p1 += "lib"; // does not insert a separator
    std::cout << R"("var" + "lib" --> )" << p1 << '\n';
    auto str = std::string{"1234567"};
    p1.concat(std::begin(str) + 3, std::end(str) - 1);
    std::cout << "p1.concat --> " << p1 << '\n';
}
```

Output:

```text
"" + "var" --> "var"
"var" + "lib" --> "varlib"
p1.concat --> "varlib456"
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3055 | C++17 | the specification of concatenating a single character was
      ill-formed | made well-formed
  LWG 3244 | C++17 | constraint that `Source` cannot be `path` was missing |
      added

### See also

- **appendoperator/=** — appends elements to the path with a directory separator
  (public member function)
- **operator/ (C++17)** — concatenates two paths with a directory separator
  (function)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/path/concat*
