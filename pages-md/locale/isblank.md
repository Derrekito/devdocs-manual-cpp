# std::isblank(std::locale)

```cpp
template< class CharT >
bool isblank( CharT ch, const locale& loc );  // (since C++11)
```

Checks if the given character is classified as a blank character by the given
locale's `std::ctype` facet.

### Parameters

- **ch** — character
- **loc** — locale

### Return value

Returns `true` if the character is classified as a blank character, `false`
otherwise.

### Possible implementation

```cpp
template<class CharT>
bool isblank(CharT ch, const std::locale& loc)
{
    return std::use_facet<std::ctype<CharT>>(loc).is(std::ctype_base::blank, ch);
}
```

### Example

Demonstrates the use of `isblank()` with different locales (OS-specific).

```cpp
#include <iostream>
#include <locale>

void try_with(wchar_t c, const char* loc)
{
    std::wcout << "isblank('" << c << "', locale(\"" << loc << "\")) returned "
               << std::boolalpha
               << std::isblank(c, std::locale(loc)) << '\n';
}

int main()
{
    const wchar_t IDEO_SPACE = L'\u3000'; // Unicode character 'IDEOGRAPHIC SPACE'
    try_with(IDEO_SPACE, "C");
    try_with(IDEO_SPACE, "en_US.UTF-8");
}
```

Possible output:

```text
isblank(' ', locale("C")) returned false
isblank(' ', locale("en_US.UTF-8")) returned true
```

### See also

- **isblank (C++11)** — checks if a character is a blank character (function)
- **iswblank (C++11)** — checks if a wide character is a blank character
  (function)

---
*Source: https://en.cppreference.com/w/cpp/locale/isblank*
