# std::iscntrl(std::locale)

```cpp
template< class CharT >
bool iscntrl( CharT ch, const locale& loc );
```

Checks if the given character is classified as a control character by the given
locale's `std::ctype` facet.

### Parameters

- **ch** — character
- **loc** — locale

### Return value

Returns `true` if the character is classified as a control character, `false`
otherwise.

### Possible implementation

```cpp
template<class CharT>
bool iscntrl(CharT ch, const std::locale& loc)
{
    return std::use_facet<std::ctype<CharT>>(loc).is(std::ctype_base::cntrl, ch);
}
```

### Example

Demonstrates the use of `iscntrl()` with different locales (OS-specific).

```cpp
#include <iostream>
#include <locale>

int main()
{
    const wchar_t CCH = L'\u0094'; // Destructive Backspace in Unicode

    std::locale loc1("C");
    std::cout << "iscntrl(CCH, C locale) returned "
              << std::boolalpha << std::iscntrl(CCH, loc1) << '\n';

    std::locale loc2("en_US.UTF8");
    std::cout << "iscntrl(CCH, Unicode locale) returned "
              << std::boolalpha << std::iscntrl(CCH, loc2) << '\n';
}
```

Possible output:

```text
iscntrl(CCH, C locale) returned false
iscntrl(CCH, Unicode locale) returned true
```

### See also

- **iscntrl** — checks if a character is a control character (function)
- **iswcntrl** — checks if a wide character is a control character (function)

---
*Source: https://en.cppreference.com/w/cpp/locale/iscntrl*
