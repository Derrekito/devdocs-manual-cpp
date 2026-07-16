# std::wcscoll

```cpp
int wcscoll( const wchar_t* lhs, const wchar_t* rhs );
```

Compares two null-terminated wide strings according to the locale most recently
installed by `std::setlocale`, as defined by the `LC_COLLATE` category.

### Parameters

- **lhs, rhs** — pointers to the null-terminated wide strings to compare

### Return value

Negative value if `lhs` is *less than* (precedes) `rhs`.

`​0​` if `lhs` is *equal to* `rhs`.

Positive value if `lhs` is *greater than* (follows) `rhs`.

### Notes

Collation order is the dictionary order: the position of the letter in the
national alphabet (its *equivalence class*) has higher priority than its case or
variant. Within an equivalence class, lowercase characters collate before their
uppercase equivalents and locale-specific order may apply to the characters with
diacritics. In some locales, groups of characters compare as single *collation
units*. For example, `"ch"` in Czech follows `"h"` and precedes `"i"`, and
`"dzs"` in Hungarian follows `"dz"` and precedes `"g"`.

### Example

```cpp
#include <clocale>
#include <iostream>

void try_compare(const wchar_t* p1, const wchar_t* p2)
{
    if (std::wcscoll(p1, p2) < 0)
        std::wcout << p1 << " before " << p2 << '\n';
    else
        std::wcout << p2 << " before " << p1 << '\n';
}

int main()
{
    std::setlocale(LC_ALL, "en_US.utf8");
    std::wcout << "In the American locale: ";
    try_compare(L"hrnec", L"chrt");

    std::setlocale(LC_COLLATE, "cs_CZ.utf8");
    std::wcout << "In the Czech locale: ";
    try_compare(L"hrnec", L"chrt");

    std::setlocale(LC_COLLATE, "en_US.utf8");
    std::wcout << "In the American locale: ";
    try_compare(L"år", L"ängel");

    std::setlocale(LC_COLLATE, "sv_SE.utf8");
    std::wcout << "In the Swedish locale: ";
    try_compare(L"år", L"ängel");
}
```

Output:

```text
In the American locale: chrt before hrnec
In the Czech locale: hrnec before chrt
In the American locale: ängel before år
In the Swedish locale: år before ängel
```

### See also

- **strcoll** — compares two strings in accordance to the current locale
  (function)
- **do_compare [virtual]** — compares two strings using this facet's collation
  rules (virtual protected member function of `std::collate<CharT>`)
- **wcsxfrm** — transform a wide string so that `wcscmp` would produce the same
  result as `wcscoll` (function)

**C documentation for `wcscoll`**

---
*Source: https://en.cppreference.com/w/cpp/string/wide/wcscoll*
