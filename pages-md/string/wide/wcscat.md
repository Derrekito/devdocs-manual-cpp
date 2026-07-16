# std::wcscat

```cpp
wchar_t* wcscat( wchar_t* dest, const wchar_t* src );
```

Appends a copy of the wide string pointed to by `src` to the end of the wide
string pointed to by `dest`. The wide character `src[0]` replaces the null
terminator at the end of `dest`. The resulting wide string is null-terminated.

The behavior is undefined if the destination array is not large enough for the
contents of both `src` and `dest` and the terminating null wide character.

The behavior is undefined if the strings overlap.

### Parameters

- **dest** — pointer to the null-terminated wide string to append to
- **src** — pointer to the null-terminated wide string to copy from

### Return value

Returns a copy of `dest`.

### Example

```cpp
#include <clocale>
#include <cwchar>
#include <iostream>

int main(void)
{
    wchar_t str[50] = L"Земля, прощай.";
    std::wcscat(str, L" ");
    std::wcscat(str, L"В добрый путь.");

    std::setlocale(LC_ALL, "en_US.utf8");
    std::wcout.imbue(std::locale("en_US.utf8"));
    std::wcout << str << '\n';
}
```

Possible output:

```text
Земля, прощай. В добрый путь.
```

### See also

- **wcsncat** — appends a certain amount of wide characters from one wide string
  to another (function)
- **strcat** — concatenates two strings (function)
- **wcscpy** — copies one wide string to another (function)

**C documentation for `wcscat`**

---
*Source: https://en.cppreference.com/w/cpp/string/wide/wcscat*
