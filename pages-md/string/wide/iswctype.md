# std::iswctype

```cpp
int iswctype( std::wint_t wc, std::wctype_t desc );
```

Classifies the wide character `wc` using the current C locale's `LC_CTYPE`
category identified by `desc`.

If the value of `wc` is neither representable as a `wchar_t` nor equal to the
value of the macro `WEOF`, the behavior is undefined.

### Parameters

- **wc** — the wide character to classify
- **desc** — the `LC_CTYPE` category, obtained from a call to `std::wctype`

### Return value

Non-zero if the character `wc` has the property identified by `desc` in
`LC_CTYPE` facet of the current C locale, zero otherwise.

### Example

```cpp
#include <clocale>
#include <cwctype>
#include <iostream>

bool classify(wchar_t wc, const std::string& cat)
{
    return std::iswctype(wc, std::wctype(cat.c_str()));
}

int main()
{
    std::setlocale(LC_ALL, "ja_JP.UTF-8");
    std::cout << "The character \u6c34 is...\n";
    for (std::string s : {"digit", "alpha", "space", "cntrl", "jkanji"})
        std::cout << s << "? " << std::boolalpha << classify(L'\u6c34', s) << '\n';
}
```

Output:

```text
The character 水 is...
digit? false
alpha? true
space? false
cntrl? false
jkanji? true
```

### See also

- **wctype** — looks up a character classification category in the current C
  locale (function)

**C documentation for `iswctype`**

---
*Source: https://en.cppreference.com/w/cpp/string/wide/iswctype*
