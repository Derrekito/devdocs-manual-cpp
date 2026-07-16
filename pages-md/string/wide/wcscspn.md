# std::wcscspn

```cpp
std::size_t wcscspn( const wchar_t* dest, const wchar_t* src );
```

Returns the length of the maximum initial segment of the wide string pointed to
by `dest`, that consists of only the characters *not* found in wide string
pointed to by `src`.

### Parameters

- **dest** — pointer to the null-terminated wide string to be analyzed
- **src** — pointer to the null-terminated wide string that contains the
  characters to search for

### Return value

The length of the maximum initial segment that contains only characters not
found in the character string pointed to by `src`.

### Example

The output below was obtained using clang (libc++).

```cpp
#include <cwchar>
#include <iostream>
#include <locale>

int main()
{
    wchar_t dest[] = L"白猫 黑狗 甲虫";
    //                      └───┐
    const wchar_t* src = L"甲虫,黑狗";

    const std::size_t len = std::wcscspn(dest, src);
    dest[len] = L'\0'; // terminates the segment to print it out

    std::wcout.imbue(std::locale("en_US.utf8"));
    std::wcout << L"The length of maximum initial segment is " << len << L".\n";
    std::wcout << L"The segment is \"" << dest << L"\".\n";
}
```

Possible output:

```text
The length of maximum initial segment is 3.
The segment is "白猫 ".
```

### See also

- **wcsspn** — returns the length of the maximum initial segment that consists
  of only the wide characters found in another wide string (function)
- **wcspbrk** — finds the first location of any wide character in one wide
  string, in another wide string (function)

**C documentation for `wcscspn`**

---
*Source: https://en.cppreference.com/w/cpp/string/wide/wcscspn*
