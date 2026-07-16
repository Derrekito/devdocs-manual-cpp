# std::wmemchr

```cpp
const wchar_t* wmemchr( const wchar_t* ptr, wchar_t ch, std::size_t count );  // (1)
wchar_t* wmemchr(       wchar_t* ptr, wchar_t ch, std::size_t count );  // (2)
```

Locates the first occurrence of wide character `ch` in the initial `count` wide
characters of the wide character array pointed to by `ptr`.

If `count` is zero, the function returns a null pointer.

### Parameters

- **ptr** — pointer to the wide character array to be examined
- **ch** — wide character to search for
- **count** — number of wide characters to examine

### Return value

Pointer to the location of the wide character, or a null pointer if no such
character is found.

### Example

```cpp
#include <clocale>
#include <cwchar>
#include <iostream>
#include <locale>

int main()
{
    const wchar_t str[] = L"诺不轻信，故人不负我\0诺不轻许，故我不负人。";
    wchar_t target = L'许';
    const std::size_t sz = sizeof str / sizeof *str;
    if (const wchar_t* result = std::wmemchr(str, target, sz))
    {
        std::setlocale(LC_ALL, "en_US.utf8");
        std::wcout.imbue(std::locale("en_US.utf8"));
        std::wcout << "Found '" << target << "' at position " << result - str << '\n';
    }
}
```

Possible output:

```text
Found '许' at position 14
```

### See also

- **memchr** — searches an array for the first occurrence of a character
  (function)
- **strchr** — finds the first occurrence of a character (function)
- **wcschr** — finds the first occurrence of a wide character in a wide string
  (function)
- **findfind_iffind_if_not (C++11)** — finds the first element satisfying
  specific criteria (function template)

**C documentation for `wmemchr`**

---
*Source: https://en.cppreference.com/w/cpp/string/wide/wmemchr*
