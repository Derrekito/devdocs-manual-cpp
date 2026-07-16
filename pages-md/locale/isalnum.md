# std::isalnum(std::locale)

```cpp
template< class CharT >
bool isalnum( CharT ch, const locale& loc );
```

Checks if the given character is classified as an alphanumeric character by the
given locale's `std::ctype` facet.

### Parameters

- **ch** — character
- **loc** — locale

### Return value

Returns `true` if the character is classified as alphanumeric, `false`
otherwise.

### Possible implementation

```cpp
template<class CharT>
bool isalnum(CharT ch, const std::locale& loc)
{
    return std::use_facet<std::ctype<CharT>>(loc).is(std::ctype_base::alnum, ch);
}
```

### Example

Demonstrates the use of `isalnum()` with different locales (OS-specific).

```cpp
#include <iostream>
#include <locale>

int main()
{
    const wchar_t c = L'\u2135'; // mathematical symbol aleph

    std::locale loc1("C");
    std::cout << "isalnum('ℵ', C locale) returned "
              << std::boolalpha << std::isalnum(c, loc1) << '\n';

    std::locale loc2("en_US.UTF-8");
    std::cout << "isalnum('ℵ', Unicode locale) returned "
              << std::boolalpha << std::isalnum(c, loc2) << '\n';
}
```

Possible output:

```text
isalnum('ℵ', C locale) returned false
isalnum('ℵ', Unicode locale) returned true
```

### See also

- **isalnum** — checks if a character is alphanumeric (function)
- **iswalnum** — checks if a wide character is alphanumeric (function)

---
*Source: https://en.cppreference.com/w/cpp/locale/isalnum*
