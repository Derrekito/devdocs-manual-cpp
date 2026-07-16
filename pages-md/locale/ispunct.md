# std::ispunct(std::locale)

```cpp
template< class CharT >
bool ispunct( CharT ch, const locale& loc );
```

Checks if the given character is classified as a punctuation character by the
given locale's `std::ctype` facet.

### Parameters

- **ch** — character
- **loc** — locale

### Return value

Returns `true` if the character is classified as punctuation, `false` otherwise.

### Possible implementation

```cpp
template<class CharT>
bool ispunct(CharT ch, const std::locale& loc)
{
    return std::use_facet<std::ctype<CharT>>(loc).is(std::ctype_base::punct, ch);
}
```

### Example

Demonstrates the use of `std::ispunct()` with different locales (OS-specific).

```cpp
#include <iostream>
#include <locale>

int main()
{
    const wchar_t c = L'\u214b'; // upside-down ampersand

    std::locale loc1("C");
    std::cout << "ispunct('⅋', C locale) returned "
              << std::boolalpha << std::ispunct(c, loc1) << '\n';

    std::locale loc2("en_US.UTF-8");
    std::cout << "ispunct('⅋', Unicode locale) returned "
              << std::boolalpha << std::ispunct(c, loc2) << '\n';
}
```

Possible output:

```text
isalpha('⅋', C locale) returned false
isalpha('⅋', Unicode locale) returned true
```

### See also

- **ispunct** — checks if a character is a punctuation character (function)
- **iswpunct** — checks if a wide character is a punctuation character
  (function)

---
*Source: https://en.cppreference.com/w/cpp/locale/ispunct*
