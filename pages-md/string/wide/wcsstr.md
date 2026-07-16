# std::wcsstr

```cpp
const wchar_t* wcsstr( const wchar_t* dest, const wchar_t* src );
wchar_t* wcsstr(       wchar_t* dest, const wchar_t* src );
```

Finds the first occurrence of the wide string `src` in the wide string pointed
to by `dest`. The terminating null characters are not compared.

### Parameters

- **dest** — pointer to the null-terminated wide string to examine
- **src** — pointer to the null-terminated wide string to search for

### Return value

Pointer to the first character of the found substring in `dest`, or a null
pointer if no such substring is found. If `src` points to an empty string,
`dest` is returned.

### Example

```cpp
#include <clocale>
#include <cwchar>
#include <iostream>

int main()
{
    wchar_t const* origin = L"アルファ, ベータ, ガンマ, アルファ, ベータ, ガンマ.";
    wchar_t const* target = L"ベータ";
    wchar_t const* result = origin;

    std::setlocale(LC_ALL, "en_US.utf8");
    std::wcout << L"Substring to find: \"" << target << L"\"\n"
               << L"String to search: \"" << origin << L"\"\n\n";

    for (; (result = std::wcsstr(result, target)) != nullptr; ++result)
        std::wcout << L"Found: \"" << result << L"\"\n";
}
```

Possible output:

```text
Substring to find: "ベータ"
String to search: "アルファ, ベータ, ガンマ, アルファ, ベータ, ガンマ."

Found: "ベータ, ガンマ, アルファ, ベータ, ガンマ."
Found: "ベータ, ガンマ."
```

### See also

- **find** — finds the first occurrence of the given substring (public member
  function of `std::basic_string<CharT,Traits,Allocator>`)
- **strstr** — finds the first occurrence of a substring of characters
  (function)
- **wcschr** — finds the first occurrence of a wide character in a wide string
  (function)
- **wcsrchr** — finds the last occurrence of a wide character in a wide string
  (function)

**C documentation for `wcsstr`**

---
*Source: https://en.cppreference.com/w/cpp/string/wide/wcsstr*
