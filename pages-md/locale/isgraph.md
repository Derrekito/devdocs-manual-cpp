# std::isgraph(std::locale)

```cpp
template< class CharT >
bool isgraph( CharT ch, const locale& loc );
```

Checks if the given character is classified as a graphic character (i.e.
printable, excluding space) by the given locale's `std::ctype` facet.

### Parameters

- **ch** — character
- **loc** — locale

### Return value

Returns `true` if the character is classified as graphic, `false` otherwise.

### Possible implementation

```cpp
template<class CharT>
bool isgraph(CharT ch, const std::locale& loc)
{
    return std::use_facet<std::ctype<CharT>>(loc).is(std::ctype_base::graph, ch);
}
```

### Example

Demonstrates the use of `isgraph()` with different locales (OS-specific).

```cpp
#include <iostream>
#include <locale>

int main()
{
    const wchar_t c = L'\u2a0c'; // quadruple integral

    std::locale loc1("C");
    std::cout << "isgraph('⨌', C locale) returned "
              << std::boolalpha << std::isgraph(c, loc1) << '\n';

    std::locale loc2("en_US.UTF-8");
    std::cout << "isgraph('⨌', Unicode locale) returned "
              << std::boolalpha << std::isgraph(c, loc2) << '\n';
}
```

Possible output:

```text
isgraph('⨌', C locale) returned false
isgraph('⨌', Unicode locale) returned true
```

### See also

- **isgraph** — checks if a character is a graphical character (function)
- **iswgraph** — checks if a wide character is a graphical character (function)

---
*Source: https://en.cppreference.com/w/cpp/locale/isgraph*
