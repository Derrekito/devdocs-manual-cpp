# std::wcsspn

```cpp
size_t wcsspn( const wchar_t* dest, const wchar_t* src );
```

Returns the length of the maximum initial segment of the wide string pointed to
by `dest`, that consists of only the characters found in wide string pointed to
by `src`.

### Parameters

- **dest** — pointer to the null-terminated wide string to be analyzed
- **src** — pointer to the null-terminated wide string that contains the
  characters to search for

### Return value

The length of the maximum initial segment that contains only characters from
wide string pointed to by `src`.

### Example

```cpp
#include <cwchar>
#include <iostream>
#include <locale>

int main()
{
    wchar_t dest[] = L"白猫 黑狗 甲虫";
    const wchar_t src[] = L" 狗猫 白黑 ";
    const std::size_t len = std::wcsspn(dest, src);
    dest[len] = L'\0'; // terminates the segment to print it out

    std::wcout.imbue(std::locale("en_US.utf8"));
    std::wcout << L"The length of maximum initial segment is " << len << L".\n";
    std::wcout << L"The segment is \"" << dest << L"\".\n";
}
```

Possible output:

```text
The length of maximum initial segment is 6.
The segment is "白猫 黑狗 ".
```

### See also

- **wcscspn** — returns the length of the maximum initial segment that consists
  of only the wide *not* found in another wide string (function)
- **wcspbrk** — finds the first location of any wide character in one wide
  string, in another wide string (function)

**C documentation for `wcsspn`**

---
*Source: https://en.cppreference.com/w/cpp/string/wide/wcsspn*
