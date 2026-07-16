# std::towupper

```cpp
std::wint_t towupper( std::wint_t ch );
```

Converts the given wide character to uppercase, if possible.

If the value of `ch` is neither representable as a `wchar_t` nor equal to the
value of the macro `WEOF`, the behavior is undefined.

### Parameters

- **ch** — wide character to be converted

### Return value

Uppercase version of `ch` or unmodified `ch` if no uppercase version is listed
in the current C locale.

### Notes

Only 1:1 character mapping can be performed by this function, e.g. the uppercase
form of 'ß' is (with some exceptions) the two-character string "SS", which
cannot be obtained by `std::towupper`.

ISO 30112 specifies which pairs of Unicode characters are included in this
mapping.

### Example

The latin letter 'ſ' (U+017F) is the alternative lowercase form of 'S' (U+0053).

```cpp
#include <clocale>
#include <cwctype>
#include <iostream>

int main()
{
    wchar_t c = L'\u017f'; // Latin small letter Long S ('ſ')

    std::cout << std::hex << std::showbase;
    std::cout << "in the default locale, towupper("
              << static_cast<std::wint_t>(c) << ") = "
              << std::towupper(c) << '\n';

    std::setlocale(LC_ALL, "en_US.utf8");
    std::cout << "in Unicode locale, towupper("
              << static_cast<std::wint_t>(c) << ") = "
              << std::towupper(c) << '\n';
}
```

Output:

```text
in the default locale, towupper(0x17f) = 0x17f
in Unicode locale, towupper(0x17f) = 0x53
```

### See also

- **towlower** — converts a wide character to lowercase (function)
- **toupper(std::locale)** — converts a character to uppercase using the ctype
  facet of a locale (function template)
- **toupper** — converts a character to uppercase (function)

**C documentation for `towupper`**

---
*Source: https://en.cppreference.com/w/cpp/string/wide/towupper*
