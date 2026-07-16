# std::isupper(std::locale)

```cpp
template< class CharT >
bool isupper( CharT ch, const locale& loc );
```

Checks if the given character is classified as an uppercase alphabetic character
by the given locale's `std::ctype` facet.

### Parameters

- **ch** — character
- **loc** — locale

### Return value

Returns `true` if the character is classified as uppercase, `false` otherwise.

### Possible implementation

```cpp
template<class CharT>
bool isupper(CharT ch, const std::locale& loc)
{
    return std::use_facet<std::ctype<CharT>>(loc).is(std::ctype_base::upper, ch);
}
```

### Example

Demonstrates the use of `std::isupper()` with different locales (OS-specific).

```cpp
#include <iostream>
#include <locale>

int main()
{
    const wchar_t c = L'\u00de'; // LATIN CAPITAL LETTER THORN

    std::locale loc1("C");
    std::cout << std::boolalpha
              << "isupper('Þ', C locale) returned " << std::isupper(c, loc1) << '\n'
              << "islower('Þ', C locale) returned " << std::islower(c, loc1) << '\n';

    std::locale loc2("en_US.UTF8");
    std::cout << "isupper('Þ', Unicode locale) returned "
              << std::isupper(c, loc2) << '\n'
              << "islower('Þ', Unicode locale) returned "
              << std::islower(c, loc2) << '\n';
}
```

Possible output:

```text
isupper('Þ', C locale) returned false
islower('Þ', C locale) returned false
isupper('Þ', Unicode locale) returned true
islower('Þ', Unicode locale) returned false
```

### See also

- **isupper** — checks if a character is an uppercase character (function)
- **iswupper** — checks if a wide character is an uppercase character (function)

---
*Source: https://en.cppreference.com/w/cpp/locale/isupper*
