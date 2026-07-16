# std::isspace(std::locale)

```cpp
template< class CharT >
bool isspace( CharT ch, const locale& loc );
```

Checks if the given character is classified as a whitespace character by the
given locale's `std::ctype` facet.

### Parameters

- **ch** — character
- **loc** — locale

### Return value

Returns `true` if the character is classified as a whitespace character, `false`
otherwise.

### Possible implementation

```cpp
template<class CharT>
bool isspace(CharT ch, const std::locale& loc)
{
    return std::use_facet<std::ctype<CharT>>(loc).is(std::ctype_base::space, ch);
}
```

### Example

Demonstrates the use of `std::isspace()` with different locales (OS-specific).

```cpp
#include <iostream>
#include <locale>

void try_with(wchar_t c, const char* loc)
{
    std::wcout << "isspace('" << c << "', locale(\"" << loc << "\")) returned "
               << std::boolalpha << std::isspace(c, std::locale(loc)) << '\n';
}

int main()
{
    const wchar_t EM_SPACE = L'\u2003'; // Unicode character 'EM SPACE'
    try_with(EM_SPACE, "C");
    try_with(EM_SPACE, "en_US.UTF8");
}
```

Possible output:

```text
isspace(' ', locale("C")) returned false
isspace(' ', locale("en_US.UTF8")) returned true
```

### See also

- **isspace** — checks if a character is a space character (function)
- **iswspace** — checks if a wide character is a space character (function)

---
*Source: https://en.cppreference.com/w/cpp/locale/isspace*
