# std::wcstof, std::wcstod, std::wcstold

```cpp
float       wcstof( const wchar_t* str, wchar_t** str_end );  // (since C++11)
double      wcstod( const wchar_t* str, wchar_t** str_end );
long double wcstold( const wchar_t* str, wchar_t** str_end );  // (since C++11)
```

Interprets a floating point value in a wide string pointed to by `str`.

Function discards any whitespace characters (as determined by `std::iswspace`)
until first non-whitespace character is found. Then it takes as many characters
as possible to form a valid floating-point representation and converts them to a
floating-point value. The valid floating-point value can be one of the
following:

- decimal floating-point expression. It consists of the following parts:

- hexadecimal floating-point expression. It consists of the following parts:
- infinity expression. It consists of the following parts:
- not-a-number expression. It consists of the following parts:
*(since C++11)*

- any other expression that may be accepted by the currently installed C locale

The functions sets the pointer pointed to by `str_end` to point to the wide
character past the last character interpreted. If `str_end` is a null pointer,
it is ignored.

### Parameters

- **str** — pointer to the null-terminated wide string to be interpreted
- **str_end** — pointer to a pointer to a wide character

### Return value

Floating point value corresponding to the contents of `str` on success. If the
converted value falls out of range of corresponding return type, range error
occurs and `HUGE_VAL`, `HUGE_VALF` or `HUGE_VALL` is returned. If no conversion
can be performed, `​0​` is returned.

### Example

```cpp
#include <cerrno>
#include <clocale>
#include <cwchar>
#include <iostream>
#include <string>

int main()
{
    const wchar_t* p = L"111.11 -2.22 0X1.BC70A3D70A3D7P+6 -Inf 1.18973e+4932zzz";
    wchar_t* end;
    std::wcout << "Parsing L\"" << p << "\":\n";
    for (double f = std::wcstod(p, &end); p != end; f = std::wcstod(p, &end))
    {
        std::wcout << "  '" << std::wstring(p, end-p) << "' -> ";
        p = end;
        if (errno == ERANGE)
        {
            std::wcout << "range error, got ";
            errno = 0;
        }
        std::wcout << f << '\n';
    }

    if (std::setlocale(LC_NUMERIC, "de_DE.utf8"))
    {
        std::wcout << L"With de_DE.utf8 locale:\n";
        std::wcout << L"  '123.45' -> " << std::wcstod(L"123.45", 0) << L'\n';
        std::wcout << L"  '123,45' -> " << std::wcstod(L"123,45", 0) << L'\n';
    }
}
```

Output:

```text
Parsing L"111.11 -2.22 0X1.BC70A3D70A3D7P+6 -Inf 1.18973e+4932zzz":
  '111.11' -> 111.11
  ' -2.22' -> -2.22
  ' 0X1.BC70A3D70A3D7P+6' -> 111.11
  ' -Inf' -> -inf
  ' 1.18973e+4932' -> range error, got inf
With de_DE.utf8 locale:
  '123.45' -> 123
  '123,45' -> 123.45
```

### See also

- **strtofstrtodstrtold** — converts a byte string to a floating-point value
  (function)

**C documentation for `wcstof`**

---
*Source: https://en.cppreference.com/w/cpp/string/wide/wcstof*
