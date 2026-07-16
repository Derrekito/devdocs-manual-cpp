# std::wcspbrk

```cpp
const wchar_t* wcspbrk( const wchar_t* dest, const wchar_t* src );
wchar_t* wcspbrk(       wchar_t* dest, const wchar_t* src );
```

Finds the first character in wide string pointed to by `dest`, that is also in
wide string pointed to by `src`.

### Parameters

- **dest** — pointer to the null-terminated wide string to be analyzed
- **src** — pointer to the null-terminated wide string that contains the
  characters to search for

### Return value

Pointer to the first character in `dest`, that is also in `src`, or a null
pointer if no such character exists.

### Notes

The name stands for "wide character string pointer break", because it returns a
pointer to the first of the separator ("break") characters.

### Example

```cpp
#include <cwchar>
#include <iomanip>
#include <iostream>

int main()
{
    const wchar_t* str = L"Hello world, friend of mine!";
    const wchar_t* sep = L" ,!";

    unsigned int cnt = 0;
    do
    {
        str = std::wcspbrk(str, sep); // find separator
        std::wcout << std::quoted(str) << L'\n';
        if (str)
            str += std::wcsspn(str, sep); // skip separator
        ++cnt; // increment word count
    } while (str && *str);

    std::wcout << L"There are " << cnt << L" words\n";
}
```

Output:

```text
" world, friend of mine!"
", friend of mine!"
" of mine!"
" mine!"
"!"
There are 5 words
```

### See also

- **wcscspn** — returns the length of the maximum initial segment that consists
  of only the wide *not* found in another wide string (function)
- **wcschr** — finds the first occurrence of a wide character in a wide string
  (function)
- **strpbrk** — finds the first location of any character from a set of
  separators (function)

**C documentation for `wcspbrk`**

---
*Source: https://en.cppreference.com/w/cpp/string/wide/wcspbrk*
