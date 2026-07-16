# std::isalpha(std::locale)

```cpp
template< class CharT >
bool isalpha( CharT ch, const locale& loc );
```

Checks if the given character is classified as an alphabetic character by the
given locale's `std::ctype` facet.

### Parameters

- **ch** — character
- **loc** — locale

### Return value

Returns `true` if the character is classified as alphabetic, `false` otherwise.

### Possible implementation

```cpp
template<class CharT>
bool isalpha(CharT ch, const std::locale& loc)
{
    return std::use_facet<std::ctype<CharT>>(loc).is(std::ctype_base::alpha, ch);
}
```

### Example

Demonstrates the use of `isalpha()` with different locales (OS-specific).

```cpp
#include <iostream>
#include <locale>

int main()
{
    const wchar_t c = L'\u042f'; // cyrillic capital letter ya

    std::locale loc1("C");
    std::cout << "isalpha('Я', C locale) returned "
              << std::boolalpha << std::isalpha(c, loc1) << '\n';

    std::locale loc2("en_US.UTF8");
    std::cout << "isalpha('Я', Unicode locale) returned "
              << std::boolalpha << std::isalpha(c, loc2) << '\n';
}
```

Possible output:

```text
isalpha('Я', C locale) returned false
isalpha('Я', Unicode locale) returned true
```

### See also

- **isalpha** — checks if a character is alphabetic (function)
- **iswalpha** — checks if a wide character is alphabetic (function)

---
*Source: https://en.cppreference.com/w/cpp/locale/isalpha*
