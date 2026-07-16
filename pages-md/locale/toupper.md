# std::toupper(std::locale)

```cpp
template< class CharT >
CharT toupper( CharT ch, const locale& loc );
```

Converts the character `ch` to uppercase if possible, using the conversion rules
specified by the given locale's `std::ctype` facet.

### Parameters

- **ch** — character
- **loc** — locale

### Return value

Returns the uppercase form of `ch` if one is listed in the locale, otherwise
returns `ch` unchanged.

### Notes

Only 1:1 character mapping can be performed by this function, e.g. the uppercase
form of 'ß' is (with some exceptions) the two-character string "SS", which
cannot be obtained by `std::toupper`.

### Possible implementation

```cpp
template<class CharT>
CharT toupper(CharT ch, const std::locale& loc)
{
    return std::use_facet<std::ctype<CharT>>(loc).toupper(ch);
}
```

### Example

```cpp
#include <cwctype>
#include <iostream>
#include <locale>

int main()
{
    wchar_t c = L'\u017f'; // Latin small letter Long S ('ſ')

    std::cout << std::hex << std::showbase;

    std::cout << "in the default locale, toupper(" << (std::wint_t)c << ") = "
              << (std::wint_t)std::toupper(c, std::locale()) << '\n';

    std::cout << "in Unicode locale, toupper(" << (std::wint_t)c << ") = "
              << (std::wint_t)std::toupper(c, std::locale("en_US.utf8")) << '\n';
}
```

Possible output:

```text
in the default locale, toupper(0x17f) = 0x17f
in Unicode locale, toupper(0x17f) = 0x53
```

### See also

- **tolower(std::locale)** — converts a character to lowercase using the `ctype`
  facet of a locale (function template)
- **toupper** — converts a character to uppercase (function)
- **towupper** — converts a wide character to uppercase (function)

---
*Source: https://en.cppreference.com/w/cpp/locale/toupper*
