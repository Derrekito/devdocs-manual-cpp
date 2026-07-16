# std::wcscpy

```cpp
wchar_t* wcscpy( wchar_t* dest, const wchar_t* src );
```

Copies the wide string pointed to by `src` (including the terminating null wide
character) to wide character array pointed to by `dest`.

If the strings overlap, the behavior is undefined.

### Parameters

- **dest** — pointer to the wide character array to copy to
- **src** — pointer to the null-terminated wide string to copy from

### Return value

`dest`

### Example

```cpp
#include <clocale>
#include <cwchar>
#include <iostream>
#include <memory>

int main()
{
    const wchar_t* src = L"犬 means dog";
//  src[0] = L'狗'; // can't modify string literal
    auto dst = std::make_unique<wchar_t[]>(std::wcslen(src) + 1); // +1 for the null
    std::wcscpy(dst.get(), src);
    dst[0] = L'狗';
    std::setlocale(LC_ALL, "en_US.utf8");
    std::wcout.imbue(std::locale(""));
    std::wcout << src << '\n' << dst.get() << '\n';
}
```

Output:

```text
犬 means dog
狗 means dog
```

### See also

- **wcsncpy** — copies a certain amount of wide characters from one string to
  another (function)
- **wmemcpy** — copies a certain amount of wide characters between two
  non-overlapping arrays (function)
- **strcpy** — copies one string to another (function)

**C documentation for `wcscpy`**

---
*Source: https://en.cppreference.com/w/cpp/string/wide/wcscpy*
