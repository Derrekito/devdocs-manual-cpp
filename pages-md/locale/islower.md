# std::islower(std::locale)

```cpp
template< class CharT >
bool islower( CharT ch, const locale& loc );
```

Checks if the given character is classified as a lowercase alphabetic character
by the given locale's `std::ctype` facet.

### Parameters

- **ch** — character
- **loc** — locale

### Return value

Returns `true` if the character is classified as lowercase, `false` otherwise.

### Possible implementation

```cpp
template<class CharT>
bool islower(CharT ch, const std::locale& loc)
{
    return std::use_facet<std::ctype<CharT>>(loc).is(std::ctype_base::lower, ch);
}
```

### Example

Demonstrates the use of `islower()` with different locales (OS-specific).

```cpp
#include <iostream>
#include <locale>

int main()
{
    const wchar_t c = L'\u03c0'; // GREEK SMALL LETTER PI

    std::locale loc1("C");
    std::cout << std::boolalpha
              << "islower('π', C locale) returned "
              << std::islower(c, loc1) << '\n'
              << "isupper('π', C locale) returned "
              << std::isupper(c, loc1) << '\n';

    std::locale loc2("en_US.UTF8");
    std::cout << "islower('π', Unicode locale) returned "
              << std::islower(c, loc2) << '\n'
              << "isupper('π', Unicode locale) returned "
              << std::isupper(c, loc2) << '\n';
}
```

Possible output:

```text
islower('π', C locale) returned false
isupper('π', C locale) returned false
islower('π', Unicode locale) returned true
isupper('π', Unicode locale) returned false
```

### See also

- **islower** — checks if a character is lowercase (function)
- **iswlower** — checks if a wide character is lowercase (function)

---
*Source: https://en.cppreference.com/w/cpp/locale/islower*
