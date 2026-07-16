# std::filesystem::path::generic_string, std::filesystem::path::generic_wstring, std::filesystem::path::generic_u8string, std::filesystem::path::generic_u16string, std::filesystem::path::generic_u32string

```cpp
template< class CharT, class Traits = std::char_traits<CharT>,
          class Alloc = std::allocator<CharT> >
std::basic_string<CharT,Traits,Alloc>
    generic_string( const Alloc& a = Alloc() ) const;  // (1) (since C++17)
std::string generic_string() const;
std::wstring generic_wstring() const;
std::u16string generic_u16string() const;
std::u32string generic_u32string() const;
std::string generic_u8string() const;  // (since C++17) (until C++20)
std::u8string generic_u8string() const;  // (since C++20)
```

Returns the internal pathname in generic pathname format, converted to specific
string type. Conversion, if any, is specified as follows:

- If `path::value_type` is char, conversion, if any, is system-dependent. This
  is the case on typical POSIX systems (such as Linux), where native encoding is
  UTF-8 and `string()` performs no conversion.
- Otherwise, if `path::value_type` is wchar_t, conversion, if any, is
  unspecified. This is the case on Windows, where wchar_t is 16 bit and the
  native encoding is UTF-16.
- Otherwise, if `path::value_type` is char16_t, native encoding is UTF-16 and
  the conversion method is unspecified.
- Otherwise, if `path::value_type` is char32_t, native encoding is UTF-32 and
  the conversion method is unspecified.
- Otherwise, if `path::value_type` is char8_t, native encoding is UTF-8 and the
  conversion method is unspecified.

The `/` character is used as the directory separator.

1) All memory allocations are performed by `a`.

3) The result encoding in the case of `u8string()` is always UTF-8.

### Parameters

- **a** — allocator to construct the string with

**Type requirements**

**-`CharT` must be one of the encoded character types (char, wchar_t, `char8_t`(since C++20), char16_t and char32_t).**

### Return value

The internal pathname in generic pathname format, converted to specified string
type.

### Exceptions

May throw implementation-defined exceptions.

### Example

```cpp
#include <cstddef>
#include <filesystem>
#include <iomanip>
#include <iostream>
#include <span>
#include <string_view>

void print(std::string_view rem, auto const& str)
{
    std::cout << rem << std::hex << std::uppercase << std::setfill('0');
    for (const auto b : std::as_bytes(std::span{str}))
        std::cout << std::setw(2) << std::to_integer<unsigned>(b) << ' ';
    std::cout << '\n';
}

int main()
{
    std::filesystem::path p{"/家/屋"};
    std::cout << p << '\n';

    print("string    : ", p.generic_string());
    print("u8string  : ", p.generic_u8string());
    print("u16string : ", p.generic_u16string());
    print("u32string : ", p.generic_u32string());
    print("wstring   : ", p.generic_wstring());
}
```

Possible output:

```text
"/家/屋"
string    : 2F E5 AE B6 2F E5 B1 8B
u8string  : 2F E5 AE B6 2F E5 B1 8B
u16string : 2F 00 B6 5B 2F 00 4B 5C
u32string : 2F 00 00 00 B6 5B 00 00 2F 00 00 00 4B 5C 00 00
wstring   : 2F 00 00 00 B6 5B 00 00 2F 00 00 00 4B 5C 00 00
```

### See also

- **stringwstringu8stringu16stringu32string** — returns the path in native
  pathname format converted to a string (public member function)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/path/generic_string*
