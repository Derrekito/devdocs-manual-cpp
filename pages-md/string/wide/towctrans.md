# std::towctrans

```cpp
std::wint_t towctrans( std::wint_t ch, std::wctrans_t desc );
```

Maps the wide character `ch` using the current C locale's `LC_CTYPE` mapping
category identified by `desc`.

If the value of `ch` is neither representable as a `wchar_t` nor equal to the
value of the macro `WEOF`, the behavior is undefined.

### Parameters

- **ch** — the wide character to map
- **desc** — the `LC_CTYPE` mapping, obtained from a call to `std::wctrans`

### Return value

The mapped value of `ch` using the mapping identified by `desc` in `LC_CTYPE`
facet of the current C locale.

### Example

The following example demonstrates katakana to hiragana character mapping.

```cpp
#include <algorithm>
#include <clocale>
#include <cwctype>
#include <iostream>

std::wstring tohira(std::wstring str)
{
    std::transform(str.begin(), str.end(), str.begin(), [](wchar_t c)
    {
         return std::towctrans(c, std::wctrans("tojhira"));
    });
    return str;
}

int main()
{
    std::setlocale(LC_ALL, "ja_JP.UTF-8");
    std::wstring kana = L"ヒラガナ";
    std::wcout << "katakana characters " << kana
               << " are " << tohira(kana) << " in hiragana\n";
}
```

Output:

```text
katakana characters ヒラガナ are ひらがな in hiragana
```

### See also

- **wctrans** — looks up a character mapping category in the current C locale
  (function)

**C documentation for `towctrans`**

---
*Source: https://en.cppreference.com/w/cpp/string/wide/towctrans*
