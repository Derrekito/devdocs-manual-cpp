# std::tolower(std::locale)

```cpp
template< class CharT >
CharT tolower( CharT ch, const locale& loc );
```

Converts the character `ch` to lowercase if possible, using the conversion rules
specified by the given locale's `std::ctype` facet.

### Parameters

- **ch** — character
- **loc** — locale

### Return value

Returns the lowercase form of `ch` if one is listed in the locale, otherwise
return `ch` unchanged.

### Notes

Only 1:1 character mapping can be performed by this function, e.g. the Greek
uppercase letter 'Σ' has two lowercase forms, depending on the position in a
word: 'σ' and 'ς'. A call to `std::tolower` cannot be used to obtain the correct
lowercase form in this case.

### Possible implementation

```cpp
template<class CharT>
CharT tolower(CharT ch, const std::locale& loc)
{
    return std::use_facet<std::ctype<CharT>>(loc).tolower(ch);
}
```

### Example

```cpp
#include <cwctype>
#include <iostream>
#include <locale>

int main()
{
    wchar_t c = L'\u0190'; // Latin capital open E ('Ɛ')

    std::cout << std::hex << std::showbase;

    std::cout << "in the default locale, tolower(" << (std::wint_t)c << ") = "
              << (std::wint_t)std::tolower(c, std::locale()) << '\n';

    std::cout << "in Unicode locale, tolower(" << (std::wint_t)c << ") = "
              << (std::wint_t)std::tolower(c, std::locale("en_US.utf8")) << '\n';
}
```

Possible output:

```text
in the default locale, tolower(0x190) = 0x190
in Unicode locale, tolower(0x190) = 0x25b
```

### See also

- **toupper(std::locale)** — converts a character to uppercase using the ctype
  facet of a locale (function template)
- **tolower** — converts a character to lowercase (function)
- **towlower** — converts a wide character to lowercase (function)

---
*Source: https://en.cppreference.com/w/cpp/locale/tolower*
