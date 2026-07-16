# std::wcslen

```cpp
std::size_t wcslen( const wchar_t* str );
```

Returns the length of a wide string, that is the number of non-null wide
characters that precede the terminating null wide character.

The behavior is undefined if there is no null character in the wide character
array pointed to by `str`.

### Parameters

- **str** — pointer to the null-terminated wide string to be examined

### Return value

The length of the null-terminated wide string `str`.

### Possible implementation

```cpp
std::size_t wcslen(const wchar_t* start)
{
    // NB: start is not checked for nullptr!
    const wchar_t* end = start;
    while (*end != L'\0')
        ++end;
    return end - start;
}
```

### Example

```cpp
#include <clocale>
#include <cwchar>
#include <iostream>

int main()
{
    const wchar_t* str = L"爆ぜろリアル！弾けろシナプス！パニッシュメントディス、ワールド！";

    std::setlocale(LC_ALL, "en_US.utf8");
    std::wcout.imbue(std::locale("en_US.utf8"));
    std::wcout << "The length of \"" << str << "\" is " << std::wcslen(str) << '\n';
}
```

Output:

```text
The length of "爆ぜろリアル！弾けろシナプス！パニッシュメントディス、ワールド！" is 32
```

### See also

- **strlen** — returns the length of a given string (function)
- **mblen** — returns the number of bytes in the next multibyte character
  (function)

**C documentation for `wcslen`**

---
*Source: https://en.cppreference.com/w/cpp/string/wide/wcslen*
