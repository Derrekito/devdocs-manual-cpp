# std::wcschr

```cpp
const wchar_t* wcschr( const wchar_t* str, wchar_t ch );
wchar_t* wcschr(       wchar_t* str, wchar_t ch );
```

Finds the first occurrence of the wide character `ch` in the wide string pointed
to by `str`.

### Parameters

- **str** — pointer to the null-terminated wide string to be analyzed
- **ch** — wide character to search for

### Return value

Pointer to the found character in `str`, or a null pointer if no such character
is found.

### Example

```cpp
#include <cwchar>
#include <iostream>
#include <locale>

int main()
{
    const wchar_t arr[] = L"白猫 黒猫 кошки";
    const wchar_t* cat = std::wcschr(arr, L'猫');
    const wchar_t* dog = std::wcschr(arr, L'犬');

    std::cout.imbue(std::locale("en_US.utf8"));

    if (cat)
        std::cout << "The character 猫 found at position " << cat - arr << '\n';
    else
        std::cout << "The character 猫 not found\n";

    if (dog)
        std::cout << "The character 犬 found at position " << dog - arr << '\n';
    else
        std::cout << "The character 犬 not found\n";
}
```

Output:

```text
The character 猫 found at position 1
The character 犬 not found
```

### See also

- **find** — finds the first occurrence of the given substring (public member
  function of `std::basic_string<CharT,Traits,Allocator>`)
- **strchr** — finds the first occurrence of a character (function)
- **wcsrchr** — finds the last occurrence of a wide character in a wide string
  (function)
- **wcspbrk** — finds the first location of any wide character in one wide
  string, in another wide string (function)

**C documentation for `wcschr`**

---
*Source: https://en.cppreference.com/w/cpp/string/wide/wcschr*
